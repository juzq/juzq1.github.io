---
layout: post
title: "属于自己的博客（Jekyll搭建教程）"
date: 2017-07-19 14:31:49 +0800
categories: 其他
tag: jekyll
---

* content
{:toc}

# 为什么要使用Jekyll {#why-jekyll}
目前博客网站已经有了很多，比较知名的有cnblog、csdn博客等等，但这些网站基本都充满了广告，给读者的体验非常不好。并且页面比较固定，虽然有很多模板可以选，但是无法比较彻底的DIY。Wordpress也是一种可选方案，但是搭建Wordpress需要购买服务器，对于博主来讲，又是一项开支，此外还要花费心思去维护服务器，费时又费心。而[ Jekyll ](https://www.jekyll.com.cn/)就是另一种替代方案，他可以完全的DIY，可以直接部署在github，稳定性高（虽然国内的访问速度比较慢~），可以完全做到0成本，并且方便可靠。

# Ruby环境安装 {#ruby-install}
&emsp;&emsp;Jekyll是用Ruby语言实现的，要使用Jekyll我们首先就需要先安装Ruby。

## Linux环境下安装Ruby

由于jekyll依赖ruby开发环境，并且要求ruby版本>=2.1，因此无法使用centos yum安装。根据[RVM官网](https://rvm.io/)的指引，使用RVM安装Ruby环境的步骤如下:

1. 安装RVM
* 下载签名
`gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB`
* 下载文件<br>
`\curl -sSL https://get.rvm.io | bash -s stable`
* 执行文件<br>
`source /home/ljx/.rvm/scripts/rvm`

2. 查看可安装的ruby版本<br>
`rvm list known|grep ruby`

3. 安装指定的ruby版本<br>
`rvm install 2.5`

4. 测试<br>
`ruby -v`

5. 更新ruby源<br>
`gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/`

6. 查看当前ruby源<br>
`gem sources -l`

## Windows环境下安装Ruby

&emsp;&emsp;Windows环境下安装Ruby就相对简单很多，因为有人已经将繁琐的步骤制作成了一体化安装包[RubyInstaller](https://rubyinstaller.org/)，我选择的版本是附带Devkit的[Ruby+Devkit 2.5.5](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.5.5-1/rubyinstaller-devkit-2.5.5-1-x64.exe)。安装过程直接按照默认的设置，直接点下一步即可，这里不再赘述。

# jekyll安装 {#jekyll-install}

`gem install jekyll boundler`

```
C:\Users\Administrator>jekyll -v
jekyll 3.8.6
```

若打印出了jekyll的版本，则代表jekyll已经安装成功了。

这样我们的jekyll就安装完毕，就可以选择一个模板开始写博客了，最后再上传到github就搞定了。
