---
layout: post
title: 通信协议多维度对比
date: 2019-01-08 0:13:22 +0800
categories: 总结归纳
tag: 游戏服务器
---

* content
{:toc}

# 背景 {#background}
网络程序开发就离不开客户端与服务器，而客户端与服务器交互的核心之一就是通信协议，通信协议的选择往往关系到通信的稳定性、通信效率（消息序列化/反序列化时间）、数据量（序列化后的字节大小）等等。目前主流开源的通信协议有：Json, Google Protocol Buffer, Message Pack, Facebook Thrift, Flat Buffer等等，我将依次就开发便捷度、序列化效率、数据量等维度从Java开发者的角度作出对比分析。

# 样本数据 {#sample-data}
假定我们有一个装备背包，里面装有玩家拥有的装备（为模拟较大数据的情况，假定有100万件装备），装备的数据结构如下:
```
public class Equip {
    /** 装备唯一id */
    private long id;
    /** 装备等级 */
    private int lv;
    /** 装备品质 */
    private int quality;
    /** 装备星级 */
    private int star;
    /** 装备附加属性 */
    private int[] extralAtts = new int[5];
}
```
为模拟真实数据，我们在生成每件装备时，均随机填充装备的各个字段：
```
// 构造测试数据
AtomicLong id = new AtomicLong(100000000000L);
Equip[] equips = new Equip[NUM];
for (int i = 0; i < NUM; i++) {
    equips[i] = new Equip().random(id.getAndIncrement());
}

public Equip random(long id) {
    this.id = id;
    lv = new Random().nextInt(100);
    quality = new Random().nextInt(100);
    star = new Random().nextInt(100);
    for (int i = 0; i < extralAtts.length; i++) {
        extralAtts[i] = new Random().nextInt(10000);
    }
    return this;
}
```

# Json {#json}
Json在web开发中应用广泛，编程语言基本都支持，序列化后的数据具备可读性，但由于包含数据的字段名，因此数据较为庞大，数据量大时，可考虑使用压缩。Java开发中使用广泛并且效率较高的Json库有Fastjson和Jsoniter，以下是采用Json通信协议的开发步骤。<br><br>
①. 编写通信消息类
```
public class EquipInventoryInit {
    /** 背包类型 */
    private int type;
    /** 背包里的装备 */
    private EquipMsg[] equips;
    /** 背包的扩展容量 */
    private int extralCapacity;
}

public class EquipMsg {
    /** 装备唯一id */
    private long id;
    /** 装备等级 */
    private int lv;
    /** 装备品质 */
    private int quality;
    /** 装备星级 */
    private int star;
    /** 装备附加属性 */
    private int[] extralAtts = new int[5];
}
```

②. 填充通信消息类数据
```
private static EquipInventoryInit createMsg(Equip[] equips) {
    EquipInventoryInit initMsg = new EquipInventoryInit();
    initMsg.setType(1);
    initMsg.setExtralCapacity(NUM);
    initMsg.setEquips(new EquipMsg[equips.length]);
    for (int i = 0; i < equips.length; i++) {
        initMsg.getEquips()[i] = new EquipMsg().fillByEquip(equips[i]);
    }
    return initMsg;
}

public EquipMsg fillByEquip(Equip equip) {
    id = equip.getId();
    lv = equip.getLv();
    quality = equip.getQuality();
    star = equip.getStar();
    for (int i = 0; i < extralAtts.length; i++) {
        extralAtts[i] = equip.getExtralAtts()[i];
    }
    return this;
}
```

## Fastjson {#fastjson}
<https://github.com/alibaba/fastjson><br><br>
③. 序列化
```
byte[] fastjsonBytes = JSON.toJSONBytes(initMsg);
```
④. 反序列化
```
EquipInventoryInit o = JSON.parseObject(fastjsonBytes, EquipInventoryInit.class);
```
## Jsoniter {#jsoniter}
<http://jsoniter.com/><br>
<https://github.com/json-iterator><br><br>
③. 序列化
```
byte[] jsoniterBytes = JsonStream.serialize(initMsg).getBytes();
```
④. 反序列化
```
EquipInventoryInit jsoniterMsg = JsonIterator.deserialize(fastjsonBytes, EquipInventoryInit.class);
```

# MessagePack {#msgpack}
<https://msgpack.org/><br><br>
MessagePack是一种类似于Json、但是速度更快、体积更小，支持非常多的编程语言（50多种）。性能非常好，但是对消息类的字段顺序要求严格，若字段顺序不一致或字段个数不符，都会导致反序列化失败，并且序列化后的数据不具备可读性。开发步骤：<br><br>
①. 编写通信消息类（与Json的方式一致，这里略过）<br>
②. 填充通信消息类数据（一样，也略过）<br>
③. 序列化
```
MessagePack msgpack = new MessagePack();
byte[] msgpackBytes = msgpack.write(initMsg);
```
④. 反序列化
```
EquipInventoryInit msgpackMsg = msgpack.read(msgpackBytes, EquipInventoryInit.class);
```

# Google Protocol Buffer {#protobuf}
<https://developers.google.com/protocol-buffers/><br><br>
Protobuf支持的编程语言有C++、Java、C#、Python、Go、Dart，可根据协议文件生成编程语言代码，每个字段有单独的编号，可解决向下兼容的问题。序列化后的数据为二进制，不可读，但性能较好，因开发方式友好，效率较高，使用比较广泛。开发步骤如下：<br><br>
①. 编写通信协议文件（.proto）
```
syntax = "proto3";

// java包
option java_package = "com.digisky.canglong.msgtest.protobuf";
// java类名
option java_outer_classname="TestProto";

// 装备背包初始化
message PbEquipInventoryInit {
    uint32 type = 1;// 背包类型
    repeated PbEquip equips = 2;// 拥有的装备
    uint32 extralCapacity = 3;// 扩展容量
}

// 装备实体
message PbEquip {
    uint64 id = 1;// 实体id
    uint32 lv = 2;// 等级
    uint32 quality = 3;// 品级
    uint32 star = 4;// 星级
    repeated PbExtralAtts atts = 5;// 额外属性
}

// 附加属性
message PbExtralAtts {
    enum PbAttType {
        ATK = 0;// 攻击
        DEF = 1;// 防御
        MATK = 2;// 魔攻
        MDEF = 3;// 魔御
        LF = 4;// 生命
    }
    PbAttType attType = 1;// 属性类型
    uint32 value = 2;//
}
```

②. 使用protoc.exe工具生成编程语言代码
```
protoc -I=$SRC_DIR --java_out=$DST_DIR $SRC_DIR/Test.proto
```

③. 填充通信消息类数据
```
Builder builder = PbEquipInventoryInit.newBuilder();
builder.setType(1);
builder.setExtralCapacity(NUM);
for (Equip equip : equips) {
    TestProto.PbEquip.Builder equipBuilder = TestProto.PbEquip.newBuilder();
    equipBuilder.setId(equip.getId());
    equipBuilder.setLv(equip.getLv());
    equipBuilder.setQuality(equip.getQuality());
    equipBuilder.setStar(equip.getStar());
    int[] extralAtts = equip.getExtralAtts();
    for (int i = 0; i < extralAtts.length; i++) {
        PbExtralAtts.Builder attBuilder = PbExtralAtts.newBuilder();
        attBuilder.setAttTypeValue(i);
        attBuilder.setValue(extralAtts[i]);
        equipBuilder.addAtts(attBuilder);
    }

    builder.addEquips(equipBuilder.build());
}
```
④. 序列化
```
byte[] protobufBytes = builder.build().toByteArray();
```
⑤. 反序列化
```
Builder protobufMsg = PbEquipInventoryInit.newBuilder().mergeFrom(protobufBytes);
```



# BIO {#bio}
BIO是我当前项目正在使用的通信协议，由于该组件是从约2011年左右的老项目继承过来的，所以其开发效率、通信效率都不高，再此一起加入分析与对比。BIO的核心思想是将所有的数据都通过Map的形式封装，因为Map具有通用性与扩展性（可以put进去任何类型的数据），因此可将任意条消息合并成一条消息，但同时也导致单条消息巨大，客户端容易假死。并且Map可随意添加节点的特性项目后期不易维护，Map的格式下客户端与服务器对接消息十分艰难。<br><br>
①. 构造与填充通信消息Map
```
Map msg = new HashMap();
Map r = new HashMap();
msg.put("r", r);
r.put("type", 1);
r.put("cap", NUM);
List equipList = new ArrayList();
r.put("equips", equipList);
for (Equip equip : equips) {
    Map equipMsg = new HashMap();
    equipMsg.put("id", equip.getId());
    equipMsg.put("lv", equip.getLv());
    equipMsg.put("quality", equip.getQuality());
    equipMsg.put("star", equip.getStar());
    Map extralAttsMsg = new HashMap();
    equipMsg.put("atts", extralAttsMsg);
    int[] extralAtts = equip.getExtralAtts();
    for (int i = 0; i < extralAtts.length; i++) {
        extralAttsMsg.put(i, extralAtts[i]);
    }
    equipList.add(equipMsg);
}
```
③. 序列化
```
byte[] mapBytes = BioHelper.mapToBytes(msg, 1024);
```
④. 反序列化
```
Map map = BioHelper.mapFromBytes(mapBytes);
```

# 性能对比

# 方案选择


> 本文代码已提交至<https://github.com/lijixue/msg-test>