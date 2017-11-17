---
layout: post
title: wxPython开发总结及踩过的坑
date: 2017-11-17 00:17:43 +0800
categories: 总结归纳
tag: python
---

* content
{:toc}



# wxPython介绍 {#description}
    wxPython是针对Python编程语言的一套跨平台的图形用户界面工具集。它允许Python开发者使用一些健壮的、功能强大的图形用户接口来简单并且容易地开发GUI程序。它实现了一套封装了闻名的跨平台的使用C++编写的GUI组件——wxWidgets的Python扩展模块。
    
    像Python和wxWidgets一样，wxPython是开源的，这就意味着任何人可以免费地使用它并且查看和修改它的源代码，同样也可以贡献代码来修复bug或者完善该组件。
> 以上引用自wxPython官方网站的介绍<https://wxpython.org/pages/overview/>

# 界面构建 {#gui}
如上篇博客[苍龙服务器发布工具的优化思路](http://192.168.2.118/wordpress/2017/11/03/%e8%8b%8d%e9%be%99%e6%9c%8d%e5%8a%a1%e5%99%a8%e5%8f%91%e5%b8%83%e5%b7%a5%e5%85%b7%e7%9a%84%e4%bc%98%e5%8c%96%e6%80%9d%e8%b7%af/)中提到，使用wxPython组件构建GUI的原因便是可以方便地使用[wxFormBuilder](https://github.com/wxFormBuilder/wxFormBuilder)构建界面，简单地拖控件即可，所见即所得，很快就能拼出想要的界面效果。

# 常用控件介绍 {#widget}
* 静态文本：StaticText
StaticText用来显示普通的文字，主要方法有
```
 SetLabel(self, label)
    设置文字
    参数:	label (string)
```

* 下拉菜单：Choice
Choice用来让用户选择指定的条目，不可自定义编辑，主要方法有
````
 GetSelection(self)
    获取所选项的下标，如果没有选择返回NOT_FOUND
    返回类型: int
    返回值: 当前选项的下标（从0开始）

 GetString(self, n)
    返回下标对应的文本
    参数:	n (int) – 从0开始
    返回类型:	string
    返回值: 下标对应的文本，如果下标无效，返回空字符串
````
* 按钮：Button
按钮很常用，点击之后响应点击事件。可以在wxFormBuilder设置好按钮绑定的事件方法，然后在子类中实现该方法即可。

* 文本框：TextCtrl
文本框用来显示数量较多的文本，常用方法：
```
 AppendText(self, text)
    添加文字到文本框的末尾
    参数:	text (string) – 要添加的文字
 Clear(self)
    清空文本框
```

# 踩过的坑 {#holes}
* 在wxPython中执行高耗时任务时，会导致界面暂时卡死
解决方法：创建一个新线程来执行高耗时任务，并采用回调的方式在任务线程中修改控件数据
```
订阅消息（主线程）：
pub.subscribe(self.update_text_status, "update_text_status")

发送回调（任务线程）：
wx.CallAfter(pub.sendMessage, 'update_text_status', msg='正在发布')

定义回调方法（主线程）：
def update_text_status(self, msg):
        self.text_status.SetLabel(msg)
```
