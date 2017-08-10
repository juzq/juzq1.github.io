---
layout: post
title: git笔记
date: 2017-07-24 12:10:22 +0800
categories: 学习笔记
tag: git
---

* content
{:toc}


# github建立仓库 {#new-repo}
## 建立新项目 {#new-proj}
1. github上新建仓库
	1. 右上角`+` -> `New repository`
	2. 填写`Repository name`
	3. **勾选**`Initialize this repository with a README`

2. 将项目clone到本地<br>
`git clone https://github.com/lijixue/test.git`

## 建立已存在的项目（稍后导入） {#old-proj}
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

# 合并其他分支 {#merge}
1. 查看是否配置upstream源：`git remote -v`<br>
若结果没有upstream，则添加：
```git remote add upstream  git remote add upstream https://github.com/xxx/xxx.git```

2. 将upstream获取到本地：`git fetch upstream`
3. 合并：`git merge upstream/master`

# 使用ssh-key进行身份验证 {#ssh-key}
提交代码到远程仓库时，默认是使用用户名+密码进行身份验证，频繁提交代码时非常繁琐，因此可以使用ssh密钥的方式进行身份验证
1. 生成ssh密钥
两种方式任选一种即可：
* 命令方式：`ssh-keygen -t rsa`
* xshell生成方式：`工具`->`用户密钥管理者`->`生成`，按照提示进行操作即可<br>
生成好后默认在`C:\Users\用户名\.ssh`目录下(Windows)

2. 添加公钥到github
登陆github，进入`Settings`->`Personal settings`->`SSH and GPG keys`->`New SSH key`，填写title和key即可，key是第一步生成的.pub结尾的公钥。

3. 修改远程仓库地址
`git remote set-url origin git@github.com:USERNAME/REPO_NAME.git`<br>
其中，USERNAME为github用户名，REPO_NAME为仓库名

4. 修改私钥名称
为了使私钥能被git识别，必须将私钥放在`C:\Users\用户名\.ssh`目录下，并将私钥命名为`id_rsa`或`id_dsa`。

5. 测试
`ssh -T git@github.com`<br>
提示`Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.`<br>
则表示设置成功。<br>
提示`Permission denied (publickey).`<br>
则表示失败。