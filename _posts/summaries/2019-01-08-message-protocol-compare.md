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
网络程序开发就离不开客户端与服务器，而客户端与服务器交互的核心之一就是通信协议，通信协议的选择往往关系到通信的稳定性、通信效率（消息序列化/反序列化时间）、数据量（序列化后的字节大小）等等。目前主流开源的通信协议有：Json, Message Pack, Protocol Buffers, Flat Buffers, JDK Serialize等等，我将依次就开发便捷度、序列化效率、数据量等维度从Java开发者的角度作出对比分析。

# 样本数据 {#sample-data}
假定我们有一个装备背包，里面装有玩家拥有的装备，装备的数据结构如下:
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
long id = 100000000000L;
Equip[] equips = new Equip[NUM];
for (int i = 0; i < NUM; i++) {
    equips[i] = new Equip().random(id++);
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
开发便捷度：★☆

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
开发便捷度：★☆

# Protocol Buffers {#protobuf}
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
开发便捷度：★★★

# FlatBuffers {#flatbuffer}
<https://google.github.io/flatbuffers/><br><br>
Flatbuffer支持的编程语言有C++、Java、C#、Python、Go、Dart、JS、TS、C、PHP、Lobster、Rust，是Google专门为游戏开发而设计的通信协议，序列化后的数据可以直接读取，无需反序列化，因此可大量节省反序列化的时间。同样可根据协议文件生成编程语言代码。开发步骤如下：<br><br>
①. 编写通信协议文件（.fbs）
```
namespace com.digisky.canglong.msgtest.tester.flatbuffer;

table FbEquipInventoryInit {
    type:int; // 背包类型
    equips:[FbEquip];// 拥有的装备
    extralCapacity:int;// 扩展容量
}

table FbEquip {
    id:int64;// 实体id
    lv:int;// 等级
    quality:int;// 品级
    star:int;// 星级
    atts:[int];// 额外属性
}

root_type FbEquipInventoryInit;
```
②. 使用flatc.exe工具生成编程语言代码
```
flatc --java -o ../../java/ Test.fbs
```
③. 填充通信消息类数据
```
FlatBufferBuilder builder = new FlatBufferBuilder();
int[] equipsIndex = new int[equips.length];
for (int i = 0; i < equips.length; i++) {
    Equip equip = equips[i];
    // atts
    int attOffset = FbEquip.createAttsVector(builder, equip.getExtralAtts());
    int equipOffset = FbEquip.createFbEquip(builder, equip.getId(), equip.getLv(), equip.getQuality(),
            equip.getStar(), attOffset);
    equipsIndex[i] = equipOffset;
}
// 一定要先create里层的FbEquip然后再createEquipsVector，否则会报错
int equipsOffset = FbEquipInventoryInit.createEquipsVector(builder, equipsIndex);
FbEquipInventoryInit.startFbEquipInventoryInit(builder);
// type
FbEquipInventoryInit.addType(builder, 1);
// extralCapacity
FbEquipInventoryInit.addExtralCapacity(builder, Start.NUM);
// equips
FbEquipInventoryInit.addEquips(builder, equipsOffset);
int end = FbEquipInventoryInit.endFbEquipInventoryInit(builder);
builder.finish(end);
```
④. 序列化
```
byte[] bytes = builder.sizedByteArray()
```
⑤. 反序列化
```
ByteBuffer bb = ByteBuffer.wrap(bytes);
FbEquipInventoryInit init = FbEquipInventoryInit.getRootAsFbEquipInventoryInit(bb);
```
开发便捷度：★★★

# BIO {#bio}
BIO是当前项目正在使用的通信协议，由于该组件是从约2011年左右的老项目继承过来的，所以其开发效率、通信效率都不高，再此一起加入分析与对比。BIO的核心思想是将所有的数据都通过Map的形式封装，因为Map具有通用性与扩展性（可以put进去任何类型的数据），因此可将任意条消息合并成一条消息，但同时也导致单条消息巨大，客户端容易假死。并且Map可随意添加节点的特性项目后期不易维护，Map的格式下客户端与服务器对接消息十分艰难。<br><br>
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
开发便捷度：☆

# 测试对比 {#compare}
测试环境<br>
**CPU:Intel Core i3-4150 @3.50GHz**<br>
**JDK:Oracle JDK 1.8.0_111**<br>
结果如下表：
![image](../../../../styles/images/blog/20190117/msg_proto_compare.png)
## JDK {#compare-jdk}
优点：
1. 无需依赖其他库，可直接使用
2. 开发时时可将通信类作为公共模块(Java Dependent Module)，客户端与服务器只用维护一套代码

缺点：
1. 只能与java项目通信，不能跨语言。
2. 性能偏低。

## Json {#compare-json}
优点：
1. 支持的编程语言很多，使用广泛。
2. 数据具备可读性。

缺点：
1. 数据量较大，性能较低。
2. 没有相关的代码生成工具。
3. 安全性较低，明文数据需要加密。

## MessagePack {#compare-msgpack}
优点：
1. 支持的编程语言特别多，凡是听说过的编程语言都支持。
2. 数据量非常小，数据大小方面排名第一。

缺点：
1. 序列化与反序列化耗时较长，接近json的水平。
2. 没有相关的代码生成工具。
3. 无法向下兼容，增/删字段或调整字段位置会导致旧消息无法解析
4. ~~网上有测试数据表明反序列化比较占内存，可能是部分编程语言的锅~~

## Protobuf {#compare-protobuf}
优点：
1. 支持主流的编程语言（C++、Java、C#、Python、Go、Dart）。
2. 有proto协议文件，与代码生成，开发效率高，对接方便。
3. 在序列化、反序列化时间、数据大小上表现都较为优秀（领先JDK、Json）。
4. 每个字段有编号，可以很好的做到向下兼容。

缺点：
1. 生成的代码非常庞大，增大了项目的代码体积，打开生成的代码会导致IDE卡顿或假死。
2. 序列化、反序列化时间上不及FlatBuffers，数据大小上不及Msgpack

## FlatBuffer {#compare-flatbuffer}
优点：
1. 支持主流的编程语言（C++、Java、C#、Python、Go、Dart、JS、TS、C、PHP、Lobster、Rust）。
2. 有fbs协议文件，与代码生成，开发效率高，对接方便，且生成的代码量小，对项目代码体积影响较小。
3. 在序列化、反序列化时间上非常优秀，尤其反序列化耗时几乎为0（无需反序列化）。
4. 比较节省内存，适合移动开发。

缺点：
1. 与Protobuf相比数据较大，但也远远领先于Json。
2. 数据填充必须从里层到外层，否则会出错。
<br>

# 总结 {#sumary}

|通信协议|语言覆盖|对接方便度|开发难易度|序列化耗时|反序列化耗时|数据量|内存消耗|总评|
|-|
|BIO|★|☆|☆|★★|★★|☆|☆|7|
|ProtoBuf|★☆|★★★|★★★|★★☆|★★☆|★★☆|★★|17
|Json|★★★|★☆|★★★|★☆|★☆|★|★|12.5
|MessagePack|★★☆|★☆|★★☆|★☆|★|★★★|★☆|13.5
|JDK|☆|★★|★★☆|★|★|★☆|★★|10.5
|FlatBufer|★★|★★★|★★|★★★|★★★|★★|★★★|18

# 方案选择 {#choice}
* 传输数据最小（最省流量，适合休闲类游戏、三消类游戏、剧情类游戏）<br>
MessagePack(Gzip) + 自制代码生成工具<br>
根据本次测试数据，MessagePack在数据大小上比ProtoBuf约小40%（gzip后约小20%），序列化时间比Protobuf约多5%-25%，反序列化比Protobuf约多几十-几百ms

* 速度最快（延迟最小，适合MOBA、FPS等类型的游戏）<br>
FlatBuffer + KCP/UDP <br>
根据本次测试数据，FlatBuffer在数据大小上比ProtoBuf约大25%（gzip后约大18%），序列化时间比Protobuf约少5%-10%，反序列化比Protobuf约少几十-几百ms

* 使用最广泛、综合成绩不错<br>
Protocol Buffer<br>
如果不追求极致的数据压缩或者消息编码速度，可选择综合性能较好的ProtoBuf，因为使用较广泛，团队成员对其熟悉程度比较高，可降低一定的学习成本；部分开发框架都集成了Protobuf，可拿来直接使用，降低了部分开发成本。

<br>

> 测试代码地址：<https://github.com/lijixue/msg-test><br><br>
> 测试完整数据：<https://docs.qq.com/sheet/DSHRoRHVFY09OSlNu>