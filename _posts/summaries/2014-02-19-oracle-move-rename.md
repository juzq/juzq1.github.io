---
layout: post
title: Oracle数据文件迁移/更名
date: 2014-02-19 11:21:24 +0800
categories: 总结归纳
tag: oracle
---

* content
{:toc}


在实际的工程应用中，Oracle数据库的数据文件经常因为磁盘问题、命名问题而需要进行迁移/更名

查看当前是否启用归档模式：
`archive lg list`

# 归档模式
1. 数据文件a脱机：
`alter database datafile '路径1' offline；`

2. 把文件A拷贝到目的地（正确的目录下）:
复制拷贝数据文件a到路径2（可改名）

3. 在数据库中更改数据文件a路径：
`alter database rename file'路径1' to '路径2'；`

4. 恢复介质文件a：
`recover database datafile'路径2'；`

5. 数据文件a联机：
`alter database datafile'路径2(数据文件A的最新路径)' online；`

# 非归档模式
1. 关闭数据库：
`shutdown immedate`

2. 拷贝数据文件从旧目录到新目录：
`cp ... source distination`

3. 启动数据库到mount状态：
`startup mount`

4. 修改文件位置：
`alter database rename file '...' to '...';`

5. 打开数据库：
`alter database open;`

6. 查看数据文件：
`select name from v$datafile;`

> 原文发表在我的csdn博客：<http://blog.csdn.net/emberd/article/details/19480157>，于2017/07/21迁入