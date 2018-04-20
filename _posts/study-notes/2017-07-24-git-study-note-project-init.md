---
layout: post
title: git笔记——常用操作
date: 2017-07-24 12:10:22 +0800
categories: 学习笔记
tag: git
---

* content
{:toc}

# Git仓库Clone的两种方式
* HTTPS：在clone时无需任何验证；在push的时候需要验证用户名和密码
* SSH：需要在clone前添加SSH Key，并有clone权限；在push的时候，直接验证私钥，无需验证用户名和密码（私钥有密码仍需验证）
<br>（注：有人提到SSH方式在clone时只能clone自己拥有或管理的项目，但我在github与gitee测试均可以clone别人的项目，只要配置好SSH私钥即可，配置方法见<a href="#ssh-key">这里</a>）


# 建立仓库 {#new-repo}
## 新项目 {#new-proj}
1. github上新建仓库
	1. 右上角`+` -> `New repository`
	2. 填写`Repository name`
	3. **勾选**`Initialize this repository with a README`

2. 将项目clone到本地<br>
`git clone git@github.com:lijixue/test.git`

## 已存在的项目 {#old-proj}
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
`git remote add origin git@github.com:lijixue/test.git`

6. 推送到远程仓库
`git push origin master`

# 添加远程仓库 {#add-remote-repo}
有时，我们需要将代码push到多个远程仓库保存，这时候就需要添加一个额外的远程仓库，步骤如下：
1. 查看远程仓库：`git remote -v`
<br>一般来说这时候只有一个默认的远程仓库，名叫origin
2. 添加新的远程仓库：`git remote add digisky git@git.ppgame.com:lijixue/batch_download.git`
<br>`digisky`就是新的远程仓库名称，后面紧跟远程仓库地址
3. 再次查看远程仓库：`git remote -v`
<br>这时候就能看到新的远程仓库：digisky
4. push到新的远程仓库：`git push digisky`

# 从其他分支合并 {#merge}
如果我们的项目B是从其他项目A fork过来的，那么就需要合并项目A的一些更新到我们的项目B，步骤如下：
1. 查看是否配置upstream源：`git remote -v`
<br>若结果没有upstream，则添加项目A的地址：
<br>```git remote add upstream  git remote add upstream https://github.com/xxx/xxx.git```
2. 将upstream获取到本地：`git fetch upstream`
3. 合并：`git merge upstream/master`

# 使用SSH密钥进行身份验证 {#ssh-key}
push代码到远程仓库时，如果是HTTPS方式Clone的项目，每次push都需要验证用户名和密码，过程非常繁琐，因此可以使用ssh的方式连接远程仓库。
1. 生成ssh密钥
两种方式任选一种即可：
* 命令方式：`ssh-keygen -t rsa`
* xshell生成方式：`工具`->`用户密钥管理者`->`生成`，按照提示进行操作即可<br>
生成好后默认在`C:\Users\用户名\.ssh`目录下(Windows)

2. 添加公钥到github
登陆github，进入`Settings`->`Personal settings`->`SSH and GPG keys`->`New SSH key`，填写title和key即可，key是第一步生成的.pub结尾的公钥。

3. 修改远程仓库地址为SSH方式：`git remote set-url origin git@github.com:USERNAME/REPO_NAME.git`
<br>其中，USERNAME为github用户名，REPO_NAME为仓库名

4. 修改私钥名称
为了使私钥能被git识别，必须将私钥放在`C:\Users\用户名\.ssh`目录下，并将私钥命名为`id_rsa`或`id_dsa`。

5. 测试
`ssh -T git@github.com`<br>
提示`Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.`<br>
则表示设置成功。<br>
提示`Permission denied (publickey).`<br>
则表示失败。

# 使用多套SSH密钥 {#multi-ssh-key}
我们可能同时在使用多个远程仓库，例如github、gitee、gitlab，并且可能在不同的远程仓库使用不同的密钥。从上一节中我们知道，如果将私钥命名为`id_rsa`或`id_dsa`即可被自动识别，但如果使用多个密钥怎么办呢？
在`C:\Users\用户名\.ssh`目录下，新建文件并命名为`config`，添加如下内容：

```
Host git.ppgame.com  
   IdentityFile ~/.ssh/id_rsa_ppgame

Host github.com
   IdentityFile ~/.ssh/id_dsa_github
```
其中，`Host`对应仓库的域名，`IdentityFile`对应要使用的私钥即可