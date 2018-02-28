---
layout: post
title: 使用jdk自带工具解决线上java程序问题
date: 2018-02-26 16:13:22 +0800
categories: 总结归纳
tag: jdk
---

* content
{:toc}

# 问题 {#description}
我们有时会在线上生产环境（正式服务器）遇到测试服很难碰到的问题，例如内存泄漏、GC过于频繁、线程死锁等问题，因为该类问题需要一定数量的用户基数，因此很难在测试服遇到。正式服一般没有debug日志，也无法打断点，无法像在测试服一样进行常规问题的排查，但JDK为我们提供了很多排查该类问题的工具。熟练使用这些工具，能帮助我们快速解决上述问题，同时也是java进阶必备能力。

# 常用JDK自带工具 {#tools}

## jinfo
显示JVM的详细信息

用法：

连接到正在运行的进程：`jinfo [option] <pid>`

连接到一个核心文件：`jinfo [option] <executable <core>`

连接到远程调试服务器：`jinfo [option] [server_id@]<remote server IP or hostname>`

参数介绍：
```
-flag <name> 打印指定变量名的虚拟机参数
-flag [+|-]<name> 启用或禁用指定变量名的虚拟机参数
-flag <name>=<value> 设置虚拟机的特定参数
-flags 打印虚拟机参数
-sysprops 打印Java系统属性
<无参数> 打印虚拟机参数和Java系统属性
-h | -help 显示帮助
```

## jmap
获得运行中的jvm的堆的快照，从而可以离线分析堆，以检查内存泄漏，检查一些严重影响性能的大对象的创建，检查系统中什么对象最多，各种对象所占内存的大小等等

用法：

连接到正在运行的进程：`jmap [option] <pid>`

连接到一个核心文件：`jmap [option] <executable <core>`

连接到远程调试服务器：`jmap [option] [server_id@]<remote server IP or hostname>`

参数介绍：
```
<无参数> 打印与Solaris pmap相同的信息
-heap 打印Java堆的汇总信息
-histo[:live] 打印java堆对象的柱状图；如果有“live”子选项，类加载统计只打印指定数量的活跃对象
-clstats 打印类加载统计
-finalizerinfo 打印等待终止的对象
-dump:<dump-options> 生成hprof二进制格式的java堆快照，栗子：jmap -dump:live,format=b,file=heap.bin <pid>
	快照选项：
	live 只快照活跃对象，如果该参数没有被指定，堆中所有对象都会被快照
	format=b 二进制格式
-F 强制执行，与-dump或-histo一起使用，来强制执行，当进程未响应的时候。此时“live”子选项无效。
-h | -help 显示帮助
-J<flag> 传递参数给运行时系统
```

## jps
jps(Java Virtual Machine Process Status Tool)是JDK 1.5提供的一个显示当前所有java进程pid的命令，可以显示主机中运行的java进程，与bash命令`ps -ef |grep java`很类似。

用法：

`jps [-q] [-mlvV] [<hostid>]`

参数介绍：
```
-q 安静模式，只显示进程id
-m 输出传递给main方法的参数
-l 输出启动类的完整类名
-v 输出传递给JVM的参数
-V 输出通过flag文件传递给JVM的参数
```
我比较喜欢的用法为：`jps -lv`

## jstack
输出指定Java进程的线程堆栈信息

用法：

连接到正在运行的进程：`jstack [-l] <pid>`

连接到被挂起的进程：`jstack -F [-m] [-l] <pid>`

连接到一个核心文件：`jstack [-m] [-l] <executable> <core>`

连接到远程调试服务器：`jstack [-m] [-l] [server_id@]<remote server IP or hostname>`

参数介绍：
```
-l 输出同步锁信息
-m 检测死锁并输出线程的栈信息
```
根据实际情况决定是否需要-l与-m参数

## jstat
对java进程的资源和性能进行实时的监控，包括了对该进程的classloader、compiler、gc情况。也可以监视虚拟机内存内的堆和非堆的大小及其内存使用量，以及加载类的数量。

用法：

`jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]`

参数介绍：

```
<option> 选项，使用jstat --options可以查看可用的选项列表，如下：
	class：统计classloader的行为
	compiler：统计hotspot just-in-time编译器的行为
	gc：统计gc行为
	gccapacity：统计堆中代的容量、空间
	gccause：垃圾收集统计，包括最近引用垃圾收集的事件，基本同gcutil，比gcutil多了两列
	gcnew：统计新生代的行为
	gcnewcapacity：统计新生代的大小和空间
	gcold：统计旧生代的行为
	gcoldcapacity：统计旧生代的大小和空间
	gcpermcapacity：统计永久代的大小和空间
	gcutil：垃圾收集统计
	printcompilation：hotspot编译方法统计
-t 额外显示时间戳
<vmid> 虚拟机id
-h<lines> 每n次采样，显示标题一次
<interval> 采样间隔，以“ms”结尾代表毫秒，以“s”结尾代表秒，默认为毫秒
<count> 统计次数次数
```

# 线上常见问题的定位与思路 {#problems}
## 频繁GC或内存溢出 {#out-of-memory}
1. 使用`jstat -gc`跟踪gc中各堆内存的增长情况
2. 使用`jstat -gccause`跟踪gc的原因
3. 使用`jmap -dump:live,format=b,file=heap.bin <pid>`生成快照文件
4. 使用jhat或者可视化工具（Eclipse Memory Analyzer 、IBM HeapAnalyzer）分析堆情况。
5. 结合代码定位问题

## 线程死锁问题 {#dead-lock}
1. 使用jstack跟踪同步锁信息
2. 结合代码确定死锁的原因