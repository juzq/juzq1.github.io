---
layout: post
title: 各编程语言入口特征
date: 2018-12-04 0:13:22 +0800
categories: 总结归纳
tag: 逆向
---

* content
{:toc}

# VC++ {#vc}
## VC++6.0 {#vc6}
```
push ebp
mov ebp, esp
push xxx
push yyy
push zzz
```
区段：.text, .data, .rdata, .rsrc

## VS2008/2013/2015 {#vs2008-2013}
```
call xxx
jmp yyy
cmp ...
```
其中，VS2008的xxx为：
```
mov edi, edi
push ebp
mov ebp, esp
sub esp, n
mov ...
```
而VS2013/2015的xxx为：
```
push ebp
mov ebp, esp
sub esp, n
and ...
```
区段(2013)：.text, .data, .rdata, .rsrc, .reloc
区段(2015)：.text, .data, .rdata, .rsrc, .reloc, .textbss, .idata, .gfids, .00cfg


# 易语言 {#e}
## 独立编译 {#compile-alone}
特征与区段均与VC++6.0一致，因为易语言独立编译使用了VC++6.0的链接器。

## 非独立编译 {#compile-depend}
```
call xxx
push eax
call yyy
ascii ...
ascii ...
ascii ...
```
并可以看到krnln.fnr字样，并且程序运行起来后可以看到模块信息中有krnln.fnr

# delphi {#delphi}
```
push ebp
mov ebp, esp
add esp, n
mov ...
```
区段：.idata, .rdata, .rsrc, .reloc, .tls, CODE, DATA, BSS

# .net {#net}
因为OD无法载入，暂不做分析

# 其他
其他编程语言还有VB、 QT、BC++、ASM, PB、AutoIt等等，因为现在用得较少，也不做分析了。