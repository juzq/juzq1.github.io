---
layout: post
title: Eclipse汉化方法
date: 2013-05-06 16:10:56 +0800
categories: 总结归纳
tag: eclipse
---

* content
{:toc}


从Eclipse官方网站下载的Eclipse默认为英文版本，如需要将其转化为中文版本，需要安装多国语言插件，官方下载地址为：<http://www.eclipse.org/babel/downloads.php>。我搜集了一些资料，有多种安装方法。现总结如下：
# 方法一：Links方法（推荐）
步骤：

1. 创建一个专门存放Eclipse插件的目录如D:/eclipse/eclipse_plugins，并在此总插件目录下创建一个用于存放语言包插件的目录language，即D:/eclipse/eclipse_plugins/language

2. 将语言包(NLpack1-eclipse-SDK-3.2.1-win32.zip)解压到language目录下，language目录下必须有一个eclipse子目录，然后才是features、plugins目录。

3. 在D:/eclipse下新建一个links目录，在links目录中创建一个文本文件language.link(可任意取名)，文件的内容仅一行文字：path=D:\\eclipse\\eclipse_plugins\\language。

4. 重启Eclipse，启动时会查找links目录中的所有文本文件，并将其中path指向的插件注册到Eclipse中，选择主菜单“帮助→动态帮助”命令可以看到帮助已经是中文了。

# 方法二：直接覆盖法（不推荐）
步骤：

1. 解压下载后压缩包，将eclipse文件夹里的内容覆盖到eclipse安装目录，如D:/eclipse。
2. 重启Eclipse。

> 百度经验：<http://jingyan.baidu.com/article/e75057f28401a8ebc91a899e.html>

> 原文发表在我的csdn博客：<http://blog.csdn.net/emberd/article/details/8890950>，于2017/07/21迁入