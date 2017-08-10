---
layout: post
title: MarkDown笔记
date: 2017-07-20 18:33:11 +0800
categories: 学习笔记
tag: markdown
---

* content
{:toc}


# 简介（是什么） {#introduce}
官网：<https://daringfireball.net/projects/markdown><br>
Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。Markdown具有一系列衍生版本，用于扩展Markdown的功能（如表格、脚注、内嵌HTML等等），它们能让Markdown转换成更多的格式，例如LaTeX，Docbook。

# 用途（能干什么） {#use}
Markdown的语法简洁明了、学习容易，而且功能比纯文本更强，因此有很多人用它写博客。世界上最流行的博客平台WordPress和大型CMS如Joomla、Drupal都能很好的支持Markdown。完全采用Markdown编辑器的博客平台有Ghost和Typecho。

# 编辑器 {#editor}

## Windows
* VSCode
* Atom
* MarkdownPad
* Miu

## OSX
* VSCode
* Atom
* Byword
* Mou

## IOS
* Byword

## 推荐 {#recommend}
* MarkdownPad（Windows）<br>
官网：<https://markdownpad.com><br>
注册码（**仅用于功能测试，请在24小时内删除，若喜欢该软件，请在官网购买正版**）<br>
邮箱：```Soar360@live.com```<br>
授权秘钥：
```
GBPduHjWfJU1mZqcPM3BikjYKF6xKhlKIys3i1MU2eJHqWGImDHzWdD6xhMNLGVpbP2M5SN6bnxn2kSE8qHqNY5QaaRxmO3YSMHxlv2EYpjdwLcPwfeTG7kUdnhKE0vVy4RidP6Y2wZ0q74f47fzsZo45JE2hfQBFi2O9Jldjp1mW8HUpTtLA2a5/sQytXJUQl/QKO0jUQY4pa5CCx20sV1ClOTZtAGngSOJtIOFXK599sBr5aIEFyH0K7H4BoNMiiDMnxt1rD8Vb/ikJdhGMMQr0R4B+L3nWU97eaVPTRKfWGDE8/eAgKzpGwrQQoDh+nzX1xoVQ8NAuH+s4UcSeQ==
```


# 语法 {#syntax}
说明：每种语法后都必须添加**空格**才能生效

## 标题 {#title}
Markdown 支持两种标题的语法，类 Setext 和类 atx 形式。
1. 类 Setext 形式是用底线的形式，利用 = （最高阶标题）和 - （第二阶标题），例如<br>
```
This is an H1
=============
This is an H2
-------------
```
任何数量的 = 和 - 都可以有效果。
2. 行首插入1-6个\#，每增加一个\#表示更深入层次的内容，对应到标题的深度由 1-6 阶，其中Header1为首层，字体最大。
```
# 这是 H1 #
## 这是 H2 ##
### 这是 H3 ###
```

## 文本样式 {#text}
* 加粗：``**Text**`` （快捷键：Ctrl+B）<br>
**Text**
* 斜体：``*Text*`` （快捷键：Ctrl+I）<br>
*Text*
* 删除线：``~~Text~~`` （快捷键：Ctrl+ALT+U）<br>
~~Text~~
* 换行符 : 一行结束时输入两个空格，也可以使用`<br>`

## 列表 {#list}
Markdown支持有序列表和无序列表。
1. 有序列表:使用数字接着一个英文句点（并有个空格）：
```
1. one is...
2. two is...
```
效果可参考本段内容xD
2. 无序列表:使用星号、加号或是减号作为列表标记（任意一个即可）：
```
* i love you
+ i love you very much
- i love you more
```

* i love you
+ i love you very much
- i love you more

## 链接 {#url}
Markdown 支持两种形式的链接语法：行内式和参考式两种形式。
1. 行内式<br>
* 要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：
```
[My Blog1](http://icoding.pw/ "This link is my blog.")
[My Blog2](http://icoding.pw/)
```
[My Blog1](http://icoding.pw/ "This link is my blog.") |
[My Blog2](http://icoding.pw/) 
鼠标放在链接上，你就会发现不同。xD<br>
* 如果你是要链接到同样主机的资源，你可以使用相对路径：<br>
`[这里](/2017/07/20/markdown-study-note/)也是这篇文章的地址`<br>
[这里](/2017/07/20/markdown-study-note/)也是这篇文章的地址

2. 参考式<br>
参考式的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：<br>
```
这是[My Blog][1]的链接地址
[1]: http://icoding.pw/  "This link is my blog."
```
这是[My Blog][1]的链接地址


3. 直接链接
`<http://icoding.pw>是我的博客地址`
	<http://icoding.pw>是我的博客地址

[1]: http://icoding.pw/  "This link is my blog."

## 图片 {#pics}
跟行内式链接类似，只需要在最前面添加`!`符号即可，例如：
```
![pic](\pics\git笔记\git1.png)
```

## 分隔线 {#separate}
你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。
```
* * *
***
- - -
---------------------------------------
```
--------
## 区块引用 {#block}
Markdown 标记区块引用是使用类似 email 中用 > 的引用方式，Markdown 也允许你偷懒只在整个段落的第一行最前面加上 > 。
```
> No one can call back yesterday, yesterday will not be called again.No one can call back yesterday, yesterday will not be called again.
```
> No one can call back yesterday, yesterday will not be called again.No one can call back yesterday, yesterday will not be called again.

## 注释 {#comment}
注释分为单行注释与多行注释两种
1. 单行注释：\`单行注释\`<br>
`单行注释`
2. 多行注释：<br>
\`\`\`<br>
多行注释<br>
\`\`\`
```
多行注释
```