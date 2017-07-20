---
layout: post
title: MarkDown笔记
date: 2013-07-02 18:33:11 +0800
categories: 学习笔记
tag: markdown
---

* content
{:toc}


简介（是什么）
===============
官网：<https://daringfireball.net/projects/markdown><br>
Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。Markdown具有一系列衍生版本，用于扩展Markdown的功能（如表格、脚注、内嵌HTML等等），它们能让Markdown转换成更多的格式，例如LaTeX，Docbook。

用途（能干什么）
===============
Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客。世界上最流行的博客平台WordPress和大型CMS如Joomla、Drupal都能很好的支持Markdown。完全采用Markdown编辑器的博客平台有Ghost和Typecho。

编辑器
===============

Windows
---------------
* VSCode
* Atom
* MarkdownPad
* Miu

OSX
---------------
* VSCode
* Atom
* Byword
* Mou

IOS
---------------
* Byword

推荐
---------------
* MarkdownPad<br>
官网：https://markdownpad.com/<br>
注册码（**仅用于功能测试，请在24小时内删除，若喜欢该软件，请在官网购买正版**）<br>
邮箱：```Soar360@live.com```<br>
授权秘钥：
```
GBPduHjWfJU1mZqcPM3BikjYKF6xKhlKIys3i1MU2eJHqWGImDHzWdD6xhMNLGVpbP2M5SN6bnxn2kSE8qHqNY5QaaRxmO3YSMHxlv2EYpjdwLcPwfeTG7kUdnhKE0vVy4RidP6Y2wZ0q74f47fzsZo45JE2hfQBFi2O9Jldjp1mW8HUpTtLA2a5/sQytXJUQl/QKO0jUQY4pa5CCx20sV1ClOTZtAGngSOJtIOFXK599sBr5aIEFyH0K7H4BoNMiiDMnxt1rD8Vb/ikJdhGMMQr0R4B+L3nWU97eaVPTRKfWGDE8/eAgKzpGwrQQoDh+nzX1xoVQ8NAuH+s4UcSeQ==
```

语法
===============
说明：每种语法后都必须添加**空格**才能生效
标题
---------------
标题能显示出文章的结构。行首插入1-6个\#，每增加一个\#表示更深入层次的内容，对应到标题的深度由 1-6 阶，其中Header1为首层，字体最大。
```
# Header 1
## Header 2
### Header 3
#### Header 4
##### Header 5
###### Header 6
```

文本样式
---------------
* 加粗：``**Text**`` （快捷键：Ctrl+B）<br>
**Text**
* 斜体：``*Text*`` （快捷键：Ctrl+I）<br>
*Text*
* 删除线：``~~Text~~`` （快捷键：Ctrl+ALT+U）<br>
~~Text~~
* 链接：``[Title](URL)`` <br>
[Title](#)
* 段落 : 段落之间空一行
* 换行符 : 一行结束时输入两个空格
* 列表 :* 添加星号成为一个新的列表项
* 引用 :> 引用内容
* 内嵌代码 : `alert('Hello World');`
* 画水平线 (HR) :--------

图片
---------------
使用Markdown将图像插入文章，你需要在Markdown编辑器输入 ![]() 。 这时在预览面板中会自动创建一个图像上传框。你可以从电脑桌面拖放图片(.png, .gif, .jpg)到上传框, 或者点击图片上传框使用标准的图像上传方式。 如果你想通过链接插入网络上已经存在的图片，只要单击图片上传框的左下角的“链接”图标，这时就会呈现图像URL的输入框。想给图片添加一个标题, 你需要做的是将标题文本插图中的方括号，e.g;![This is a title]().

注释
---------------
* 单行注释：\`注释\`
* 多行注释：<br>
\`\`\`<br>
注释<br>
\`\`\`