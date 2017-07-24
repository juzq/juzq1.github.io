---
layout: post
title: git笔记
date: 2017-07-24 12:10:22 +0800
categories: 学习笔记
tag: git
---

* content
{:toc}


# github建立仓库
## 建立新项目
1. github上新建仓库
	1. 右上角`+` -> `New repository`
	2. 填写`Repository name`
	3. **勾选**`Initialize this repository with a README`

2. 将项目clone到本地<br>
`git clone https://github.com/lijixue/test.git`

## 建立已存在的项目（稍后导入）
1. github上新建仓库
	1. 右上角`+` -> `New repository`
	2. 填写`Repository name`

2. 初始化本地项目
`git init`

3. 将本地所有文件添加到git
`git add --all`

4. 提交
`git commit -m "comment"`

5. 添加远程git仓库
`git remote add origin https://github.com/lijixue/test.git`

6. 推送到远程仓库
`git push origin master`

# 合并其他分支
1. 查看是否配置upstream源：`git remote -v`<br>
若结果没有upstream，则添加：
```git remote add upstream  git remote add upstream https://github.com/xxx/xxx.git```

2. 将upstream获取到本地：`git fetch upstream`
3. 合并：`git merge upstream/master`