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
1.View->Show Console（默认快捷键：Ctrl+`）<br>
2.输入代码（仅针对v3版本）<br>

``import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)``