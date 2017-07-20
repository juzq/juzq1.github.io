---
layout: post
title: Sublime笔记
date: 2016-04-14 14:24:08 +0800
categories: 学习笔记
tag: sublime
---

* content
{:toc}


## 安装PackageControl
官网：<https://packagecontrol.io><br>
安装步骤<br>
1. 打开控制台：```View->Show Console（默认快捷键：Ctrl+`）```
2. 输入代码（仅针对v3版本）
```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
等待完成安装即可

## 安装Soda主题
1. 打开命令窗口：```Ctrl+Shift+P```
2. 打开包管理器：```ip```
3. 选择```Theme - Soda```，并等待安装完成
4. 在配置文件中启用：
```
Preference->Settings
"theme": "Soda Light 3.sublime-theme"
```
保存即可

## 添加到右键菜单
1. 进入到SublimeText3安装目录
2. 创建文本文件然后重命名为：```sublime_addright.inf```
3. 填入以下代码

```
[Version]
Signature="$Windows NT$"

[DefaultInstall]
AddReg=SublimeText3

[SublimeText3]
hkcr,"*\\shell\\SublimeText3",,,"用 SublimeText3 打开"
hkcr,"*\\shell\\SublimeText3\\command",,,"""%1%\sublime_text.exe"" ""%%1"" %%*"
hkcr,"Directory\shell\SublimeText3",,,"用 SublimeText3 打开"
hkcr,"*\\shell\\SublimeText3","Icon",0x20000,"%1%\sublime_text.exe, 0"
hkcr,"Directory\shell\SublimeText3\command",,,"""%1%\sublime_text.exe"" ""%%1"""
```

## 常用配置
```
{
	# 显示右侧迷你视图
	"always_show_minimap_viewport": true,
	# 迷你视图边框
	"draw_minimap_border": true,
	# 默认字体
	"font_face": "Consolas",
	# 字体选项
	"font_options":
	[
		"directwrite"
	],
	# 字体大小
	"font_size": 11,
	# 退出时关闭所有打开的文件，并不提示
	"hot_exit": false,
	# 忽略的包
	"ignored_packages":
	[
		"Vintage"
	],
	# 不记住上次打开的文件
	"remember_open_files": false,
	# 右下角显示文件编码
	"show_encoding": true,
	# 右下角显示文件换行符
	"show_line_endings": true,
	# 主题
	"theme": "Soda Light 3.sublime-theme",
	# 自动换行
	"word_wrap": true
}
```