---
layout: post
title: Centos7最小化安装后配置备忘
date: 2017-05-06 00:17:43 +0800
categories: 总结归纳
tag: linux,centos
---

* content
{:toc}


1. 安装wget（步骤2备用）<br>
`yum install wget -y`

2. 切换yum源为阿里云

	1. 备份旧源:<br>
	`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`
	
	2. 下载新的源文件:<br>
	`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`（可将7换成6、5等等，根据系统版本而定）

	3. 生成缓存：<br>
	`yum makecache`

3. 设置静态ip和DNS<br>
`vi /etc/sysconfig/network-scripts/ifcfg-e*`，添加如下内容：
```
BOOTPROTO="static"
IPADDR=192.168.188.6
NETMASK=255.255.255.0
GATEWAY=192.168.188.2
DNS1=61.139.2.69
DNS2=218.6.200.139
```

4. 安装jdk
	1. 官网下载tar.gc版：<http://www.oracle.com/technetwork/java/javase/downloads/index.html>
	2. 解压到/usr/local/lib/java8
	3. 创建软连接到环境变量目录


> 原文发表在我的cnblog博客：<http://www.cnblogs.com/emberd/p/6815454.html>，于2017/07/20迁入

|Item|Value|Qty|
|:---|----:|:-:|
|Com|fea|fwfa|