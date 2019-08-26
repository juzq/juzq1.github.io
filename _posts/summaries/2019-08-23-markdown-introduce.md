
# 写作方式的演变

## 原始写作
![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/0.1.jpg)  
&emsp;&emsp;曾经，在上世纪90年代与21世纪初，PC（个人电脑）还没有普及，那时候写文章还用的纸和笔，字体与排版全靠我们自己的书法功力与审美能力。

## 单机写作
![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/0.2.png)  
&emsp;&emsp;21世纪以来，受益于计算机技术的发展与半导体行业的突飞猛进，计算机的价格逐年降低，被越来越多的人所接受，个人电脑逐渐进入了千家万户。在电脑上写文章逐渐成为一种潮流，因为它高效、便捷、易修改与低成本，因此Microsoft Word/WPS成为了此时最常用的文字处理软件。

## 在线写作

&emsp;&emsp;21世纪发展至今，互联网高速发展，从最初的64Kbps电话线网络（峰值下载速度8KB/S）发展至后来的1 - 4M ADSL宽带（峰值128KB/S - 512KB/S），再到今天的100-200M光纤宽带（峰值12.5MB/S - 25MB/S）。人们已经不局限于在单机上面写文章，而希望文章被更多的人看到。于是人们选择了在线写作平台：微信公众号、知乎、简书、WordPress、cnBlog、CSDN...等各种博客平台。

# 多平台发布的不便

&emsp;&emsp;为了获得更多的阅读量，让自己的文章能够被更多的读者看到，很可能会选择将同一篇文章在多个平台上，那么此时就会遇到一个很棘手的问题：排版。

&emsp;&emsp;我们知道，各大写作平台都有自己的文本编辑器。当你在编辑器中将文本编辑好之后，需要利用编辑器中的排版功能来调整文字大小、格式、颜色等等。如果文章中有插图，还需要将图片先上传，然后在文章中插入。上述操作虽然不复杂，但如果只操作一次还好，如果想要在多个平台同时发布，那么需要在多个平台的编辑器上进行重复的操作（每个平台的编辑器都不一样，操作可能还有差异），那么作者一定会崩溃。

# 其他的一些问题

*   Word到在线平台的文章迁移

&emsp;&emsp;可能有些人因为习惯使用Word来写作，或者不喜欢某个在线平台的编辑器，他希望先在Word中将文章写好，然后再复制进在线平台。此时就会遇到问题：

1.  文中的图片全没了（目前还没有看到有在线平台可以自动识别并上传粘贴进去的图片）
2.  排版格式可能会错乱（这个跟平台编辑器有关，如果编辑器对word支持得比较好那排版就基本还在）

*   草稿未自动保存

&emsp;&emsp;可能你也被坑过，当在某个在线平台中编写了数个小时，可能因为电脑网页未响应或断电等意外的原因，导致之前编写的内容未保存而只能重头写过，此时的内心是有多么绝望。

*   图片被禁止外链

&emsp;&emsp;上文中有提到，如果想在多个平台发布文章，需要将文中的图片分别上传，那么有没有办法只上传一次呢？你可能想到，将图片上传到平台A那么就可以获得该图片在平台A上的URL地址，那么在平台B上发布文章的时候，直接插入平台A的图片地址不就可以了吗？

&emsp;&emsp;非常不幸的是，现在很多在线平台对图片做了外链限制，在平台B中是打不开平台A中的图片链接的。

*   图片被自动加水印

&emsp;&emsp;同样是多平台发布，例如你将文章中的图片发布到了CSDN博客，CSDN会自动加上其水印。如果某天你想将该文章发布到某某平台，而又丢失了图片原文件的话，就只能先将CSDN的文中图片保存下来再上传，但因为图片上有水印，就会显得文章非常不专业。

# 推荐的写作方式：MarkDown
![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/0.3.png)
## MarkDown介绍

&emsp;&emsp;那么什么是MarkDown呢？使用过Git仓库的同学应该不会陌生，创建好Git仓库后，会推荐你写一个Readme.md来介绍你的仓库，而这个文件就是MarkDown格式（从后缀就能看出来）。

&emsp;&emsp;这里引用一段百度百科的介绍：

>   Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。

&emsp;&emsp;通俗来讲，就是可以让你在“记事本”这种普通文本处理软件中，写出具有文字格式、段落编排、数学公式、图片、视频的文件。听起来是不是很有趣？更好的消息是，目前主流的在线平台都已经支持MarkDown。

&emsp;&emsp;这就意味着，你可以在“记事本”中写出像在Word里面一样漂亮的文章，还可以通过Ctrl-C + Ctrl-V将该文章拷贝到诸如微信公众号、知乎、简书、WordPress、cnBlog、CSDN等在线平台中（包括公司的Confluence平台），而无需再次编辑格式、排版等问题，因为他们都有相同的格式，并且已经在MarkDown中定义好了。

## MarkDown基本语法

&emsp;&emsp;MarkDown的语法介绍的文章非常多，随便一搜就会有很多。

&emsp;&emsp;这里一篇比较完整的教程：

>   <https://www.runoob.com/markdown/md-tutorial.html>

&emsp;&emsp;我曾经也整理过一篇：

>   <https://juzii.gitee.io/2017/07/20/markdown-study-note/>

&emsp;&emsp;如果你想看最官方的MarkDown语法介绍，可以在[这里](https://daringfireball.net/projects/markdown/basics)找到。

## 编辑器推荐

&emsp;&emsp;上文中提到过，只要是能写普通文本的软件如“记事本”都可以拿来写MarkDown。但是如果有一款软件能让你“所见即所得”，能够实时将你写的MarkDown语法转换为排版（渲染）后的效果，那岂不是更好。

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/1.png)

&emsp;&emsp;如上图所示，我推荐用的编辑器是[Typora](https://typora.io/)，这是我在Windows上是我见过体验最好、界面最美的，而且是免费的一款所见即所得的MarkDown编辑器。它同时支持Windows、OS X、Linux平台。如果你是Mac用户，那选择就挺多，听说MWeb、Ulysses、MacDown都挺好用。~~为什么是听说？因为我没有Mac设备。~~

# 图片的处理

&emsp;&emsp;如果你已经看过了MarkDown的语法，那你应该已经知道，要在MarkDown中添加图片，可以用如下的语法：

`![图片名称](图片链接)`&emsp;例如：`![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png)`

&emsp;&emsp;那么我如何才能获得我的图片链接呢？你就需要将你的图片上传至某个在线存储服务提供商才行？业内将该存储图片的地方称为“图床”，那么有什么图床可以选择呢？

## 云服务对象存储

### 七牛云

&emsp;&emsp;七牛云的对象存储服务的基础计费项包括：存储空间容量，流量，请求次数，这里有具体的[价格详情](https://www.qiniu.com/prices?source=kodo)。简单来说，就是每月有10G的免费容量，10万次免费请求，但是流量费用有0.29 元/GB。

### 腾讯云

&emsp;&emsp;腾讯云的对象存储 COS 的计费项包括：存储容量费用、请求费用、数据取回费用 和 流量费用，具体的定价在[这里](https://cloud.tencent.com/document/product/436/6239)。简单来说，就是前6个月有50G的免费容量，超期后是0.099元/G/月，请求费用是0.01元/万次，流量费用是0.5元/GB。(数据取回费用可忽略，一般用不上)

### 阿里云

&emsp;&emsp;阿里云的对象存储计费方式也是包括：存储容量，流量，请求次数，具体的定价在[这里](https://www.aliyun.com/price/product?spm=5176.7933691.1309819.6.155f2a66NOxFUg#/oss/detail)。简单来说，就是存储费用0.12/G/月，请求费用同样是0.01元/万次，流量费用闲时是0.25元/GB，忙时是0.5元/GB。

### 自有（云）服务器

&emsp;&emsp;如果你有自己的（云）服务器，那么还可以在服务器上自行搭建一个Web服务器来用作图片下载使用。但是不推荐这么做，因为图片下载特别占用服务器带宽，可能会影响服务器上的其他应用。

### 对比

&emsp;&emsp;从价格来看，费用大致是七牛云<腾讯云<阿里云，因此如果要选择云服务商，七牛云是比较好的选择，并且根据用户反馈，下载效果是不错的。价格估算：如果1篇文章有10个图片，每个图片是100K，那么该篇文章大致占用1M的存储空间，如果该篇文章有1万的阅读量，那么会消耗10G的流量，大致会产生2.9元的费用。

## 免费的方案：Git仓库

&emsp;&emsp;上面的方案都是收费的，有免费的方案吗？如果有这样一个网站，你可以将自己的文件上传上去，其他所有用户可以自由的点击访问，这不就实现了我们存储图片的需求了吗？那么现实中有这样的网站吗？

&emsp;&emsp;答案是有的！如果你是程序员，你应该听说过Github，是的，就是全球最著名的代码托管仓库。但是国内的访问速度较慢，国内有同类的网站吗？有的！它就是**码云**~~（不是马云）~~，是中国最有名的代码托管仓库，其服务器目前部署在阿里云，国内的访问速度很快。使用步骤如下：

### 注册码云帐号

<https://gitee.com/signup>

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/2.png)

<hr>

### 新建仓库

点击右上角“加号”-“新建仓库”

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/3.png)
<hr>

填写仓库名称，选择“公开”，勾选“使用Readme文件初始化这个仓库”，点击创建

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/4.png)
<hr>

### 创建文件夹

最好创建一个文件夹来区分不同的图片，例如article1

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/5.png)
<hr>

### 上传图片

进入刚新建的文件夹，点击上传文件

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/6.png)

<hr>

拖拽文件或点击上传，填写提交信息（可随意填写），点击提交  


![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/7.png)

<hr>

### 获取图片地址

点击刚刚上传的图片，点击“原始数据”即可获得图片的地址，就可以在MarkDown中使用了。

例如：`https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/2.png`

![](https://gitee.com/juzii/res/raw/master/pic/2019/08/markdown/8.png)

如果你使用的是Typora这样的实时渲染的编辑器，那么就可以看到该图片了。

# MarkDown的其他应用

&emsp;&emsp;MarkDown的可扩展性非常强，除了其自身定义的标签外，MarkDown还兼容绝大多数html元素标签，这就意味着你可以利用html元素标签实现MarkDown本身未实现的功能，例如视频、字体颜色、删除线、分割线...等等，做web前端的同学在这里就可以自由发挥了。

&emsp;&emsp;**但是，该项扩展跟平台MarkDown的具体实现有关，并不是所有平台都支持。  
&emsp;&emsp;例如公司的Confluence就不支持，如果你想了解该部分内容，请乘坐[专机](https://juzii.gitee.io/2019/08/23/markdown-introduce/#other)抵达支持的平台，否则下方内容无法正常显示。**

## 视频

&emsp;&emsp;一般各大视频网站（爱奇艺、腾讯视频、优酷、Bilibili等）在视频的下方都有分享的按钮，在分享中一般会有“通用代码”或“嵌入代码”的内容，我们仅需要将代码拷贝进入我们的Markdown文中即可。

例如，如下的视频代码为：

```
<iframe src="//player.bilibili.com/player.html?aid=64860674&cid=112590017&page=1" scrolling="no" 
border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
```

为了更好的适应页面与视频内容，我们最好给视频加上高度与宽度：`width="600" height="580"`

<iframe src="//player.bilibili.com/player.html?aid=64860674&cid=112590017&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="800" height="700"> </iframe>

## 文字颜色与字体

&emsp;&emsp;文字颜色与字体使用`<font>`标签即可，分别设置color属性与face属性

格式：`<font color="red">红色</font>`

效果：<font color="red">红色</font>

<hr>

格式：`<font face="STCAIYUN">我是华文彩云</font>`

效果：<font face="STCAIYUN">我是华文彩云</font>

  

以上。