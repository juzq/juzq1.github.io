---
layout: post
title: Centos7最小化安装后配置备忘
date: 2017-05-06 00:17:43 +0800
categories: 总结归纳
tag: linux
---

* content
{:toc}


# 安装wget {#wget}
`yum install wget -y`

# 切换yum源为阿里云 {#yum}
1. 备份旧源:

	`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`
	
2. 下载新的源文件:
	
	`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`

	（可将7换成6、5等等，根据系统版本而定）

3. 生成缓存：
	
	`yum makecache`

# 设置静态ip和DNS {#ip-dns}
`vi /etc/sysconfig/network-scripts/ifcfg-eth0`，添加如下内容：
```
BOOTPROTO="static"
IPADDR=192.168.188.6
NETMASK=255.255.255.0
GATEWAY=192.168.188.2
DNS1=61.139.2.69
DNS2=218.6.200.139
```

# 修改ulimit {#ulimit}
针对Centos6.5
1. 最大文件打开数：`sudo vi /etc/security/limits.conf`，在末尾添加以下内容：
```
* soft nofile 65535 
* hard nofile 65535
```
2. 最大进程打开数：`sudo vi /etc/security/limits.d/90-nproc.conf`
将
```
* soft nproc 1024
root soft nproc unlimited
```
修改为：
```
* soft nproc 65535
* hard nproc 65535
```

# 安装jdk {#jdk}
1. 官网下载tar.gc版：<http://www.oracle.com/technetwork/java/javase/downloads/index.html>
2. 解压到/usr/local/lib/java8
3. 创建符号链接到环境变量目录


> 原文发表在我的cnblog博客：<http://www.cnblogs.com/emberd/p/6815454.html>，于2017/07/20迁入