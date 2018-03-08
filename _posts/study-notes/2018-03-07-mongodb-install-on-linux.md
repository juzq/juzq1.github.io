---
layout: post
title: mongodb笔记——Linux环境下安装与使用
date: 2018-03-07 0:10:22 +0800
categories: 学习笔记
tag: git
---

* content
{:toc}


# 安装 {#install}
因为yum仓库默认不包含mongodb包信息，为了添加mongodb包信息，需要配置mongodb仓库
## 配置yum仓库 {#configue-yum}
1. 创建仓库文件
```
sudo vi /etc/yum.repos.d/mongodb-org-3.6.repo
```

2. 添加仓库信息
```
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

## 使用yum安装 {#yum-install}
```
sudo yum install -y mongodb-org
```

## 设置selinux {#selinux}
设置selinux为disabled或permissive

# 启动 {#start}
```
sudo service mongod start
```