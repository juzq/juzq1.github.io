---
layout: post
title: 不安装Oracle客户端使用PLSQL
date: 2014-03-20 16:10:46 +0800
categories: 总结归纳
tag: oracle
---

* content
{:toc}


PLSQL是很方便的Oracle数据库开发工具，但其依赖于Oracle客户端，因此使用起来很麻烦。今天我参考了网络上前人的一些攻略，并经过自己的整理和实践，成功地不安装Oracle客户端使用PLSQL，现总结如下：

# 安装PLSQL
我比较喜欢的版本是V7.15，可能是因为从刚开始接触oracle就一直使用的这个版本吧。下载地址可以自己百度或者在我的百度云网盘下载：<http://pan.baidu.com/s/12BssA>

# 搜集PLSQL需要使用到的Oracle数据库中的文件
包括classes12.jar、oci.dll、ocijdbc10.dll、ociw32.dll、ojdbc14.jar、orannzsbb10.dll、oraocci10.dll、oraociei10.dll、tnsnames.ora等9个文件，WIN7 64位系统还需要msvcr71.dll。可以使用如下几种方法搜集：

* 在已安装好Oracle客户端或服务器端的机器中将这些文件拷贝出来

* 在Oracle官网下载instantclient-basic-win32-11.2.0.1.0文件包

* 使用我搜集的文件包，百度云网盘地址：<http://pan.baidu.com/s/1eQDs6Pk>

在任意地方新建任意名字的文件夹，如oraclient，我的路径是C:\MyData\Development\PLSQL\oraclient，就是在PLSQL的安装目录，便于以后的拷贝。

# 新建环境变量ORACLE_HOME
变量值为步骤2所建文件夹目录，我的为`C:\MyData\Development\PLSQL\oraclient`

# 新建环境变量TNS_ADMIN
变量值同上。

# 新建环境变量NLS_LANG
变量值为`SIMPLIFIED CHINESE_CHINA.ZHS16GBK`

# 修改网络配置文件
也就是步骤2中搜集的tnsnames.ora，文件内容为
```
# tnsnames.ora Network Configuration File: C:\oracle\product\10.2.0\db_1\network\admin\tnsnames.ora
# Generated by Oracle configuration tools.

XCKY =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.66.8)(PORT =1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = xcky)
    )
  )

EXTPROC_CONNECTION_DATA =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1))
    )
    (CONNECT_DATA =
      (SID = PLSExtProc)
      (PRESENTATION = RO)
    )
  )
```
红色为需要修改的内容，XCKY为Oracle目标数据库的SID名，192.168.66.8为目标数据库IP地址，1521为目标数据库端口号，根据实际情况修改。

> 原文发表在我的csdn博客：<http://blog.csdn.net/emberd/article/details/21625249>，于2017/07/21迁入