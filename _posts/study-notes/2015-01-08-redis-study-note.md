---
layout: post
title: Redis笔记
date: 2015-01-08 10:31:24 +0800
categories: 学习笔记
tag: sublime
---

* content
{:toc}


# 指令集
* 简单密码认证：`auth`
* 关闭连接：`quit`
* 查询所有键值：`keys *`

## 持久化
* 将数据同步保存到磁盘：`save`
* 将数据异步保存到磁盘：`bgsave`
* 返回上次成功将数据保存到磁盘的Unix时戳：`lastsave`
* 将数据同步保存到磁盘，然后关闭服务：`shundown`

## 远程服务控制
* 提供服务器的信息和统计：`info`
* 实时转储收到的请求：`monitor`
* 改变复制策略设置：`slaveof`
* 在运行时配置Redis服务器：`config`


 
## 对value操作的命令
 
* 确认一个key是否存在：`exists(key)`
* 删除一个key：`del(key)`
* 返回值的类型：`type(key)`
* 返回满足给定pattern的所有key：`keys(pattern)`
* 随机返回key空间的一个：`randomkey`
* 重命名key：`keyrename(oldname, newname)`
* 返回当前数据库中key的数目：`dbsize`
* 设定一个key的活动时间（s）：`expire`
* 获得一个key的活动时间：`ttl`
* 按索引查询：`select(index)`
* 移动当前数据库中的key到dbindex数据库：`move(key, dbindex)`
* 删除当前选择数据库中的所有key：`flushdb`
* 删除所有数据库中的所有key：`flushall`
 
## 对String操作的命令
 
* 给数据库中名称为key的string赋予值value：`set(key, value)`
* 返回数据库中名称为key的string的value：`get(key)`
* 给名称为key的string赋予上一次的value：`getset(key, value)`
* 返回库中多个string的value：`mget(key1, key2,…, key N)`
* 添加string，名称为key，值为value：`setnx(key, value)`
* 向库中添加string，设定过期时间time：`setex(key, time, value)`
* 批量设置多个string的值：`mset(key N, value N)`
* 如果所有名称为key i的string都不存在：`msetnx(key N, value N)`
* 名称为key的string增1操作：`incr(key)`
* 名称为key的string增加integer：`incrby(key, integer)`
* 名称为key的string减1操作：`decr(key)`
* 名称为key的string减少integer：`decrby(key, integer)`
* 名称为key的string的值附加value：`append(key, value)`
* 返回名称为key的string的value的子串：`substr(key, start, end)`
 
## 对List操作的命令
 
* 在名称为key的list尾添加一个值为value的元素：`rpush(key, value)`
* 在名称为key的list头添加一个值为value的 元素：`lpush(key, value)`
* 返回名称为key的list的长度：`llen(key)`
* 返回名称为key的list中start至end之间的元素：`lrange(key, start, end)`
* 截取名称为key的list：`ltrim(key, start, end)`
* 返回名称为key的list中index位置的元素：`lindex(key, index)`
* 给名称为key的list中index位置的元素赋值：`lset(key, index, value)`
* 删除count个key的list中值为value的元素：`lrem(key, count, value)`
* 返回并删除名称为key的list中的首元素：`lpop(key)`
* 返回并删除名称为key的list中的尾元素：`rpop(key)`
* lpop命令的block版本。：`blpop(key1, key2,… key N, timeout)`
* rpop的block版本。：`brpop(key1, key2,… key N, timeout)`
* 返回并删除名称为srckey的list的尾元素，并将该元素添加到名称为dstkey的list的头部：`rpoplpush(srckey, dstkey)`
 
## 对Set操作的命令
 
* 向名称为key的set中添加元素member：`sadd(key, member)`
* 删除名称为key的set中的元素member：`srem(key, member) `
* 随机返回并删除名称为key的set中一个元素：`spop(key) `
* 移到集合元素：`smove(srckey, dstkey, member) `
* 返回名称为key的set的基数：`scard(key) `
* member是否是名称为key的set的元素：`sismember(key, member) `
* 求交集：`sinter(key1, key2,…key N) `
* 求交集并将交集保存到dstkey的集合：`sinterstore(dstkey, (keys)) `
* 求并集：`sunion(key1, (keys)) `
* 求并集并将并集保存到dstkey的集合：`sunionstore(dstkey, (keys)) `
* 求差集：`sdiff(key1, (keys)) `
* 求差集并将差集保存到dstkey的集合：`sdiffstore(dstkey, (keys)) `
* 返回名称为key的set的所有元素：`smembers(key) `
* 随机返回名称为key的set的一个元素：`srandmember(key) `
 
## 对Hash操作的命令
 
* 向名称为key的hash中添加元素field：`hset(key, field, value)`
* 返回名称为key的hash中field对应的value：`hget(key, field)`
* 返回名称为key的hash中field i对应的value：`hmget(key, (fields))`
* 向名称为key的hash中添加元素field：`hmset(key, (fields))`
* 将名称为key的hash中field的value增加integer：`hincrby(key, field, integer)`
* 名称为key的hash中是否存在键为field的域：`hexists(key, field)`
* 删除名称为key的hash中键为field的域：`hdel(key, field)`
* 返回名称为key的hash中元素个数：`hlen(key)`
* 返回名称为key的hash中所有键：`hkeys(key)`
* 返回名称为key的hash中所有键对应的value：`hvals(key)`
* 返回名称为key的hash中所有的键（field）及其对应的value：`hgetall(key)`

> 原文发表在我的cnblog博客：<http://www.cnblogs.com/emberd/p/4210218.html>，于2017/07/20迁入