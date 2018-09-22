---
layout: post
title: Maven Web项目发布到Tomcat服务器
date: 2018-09-22 0:13:22 +0800
categories: 总结归纳
tag: maven
---

* content
{:toc}

# 前言 {#foreword}
Maven是java开发中常用的项目管理工具，可以很好的处理java组件的依赖关系。在java web项目中，maven也提供了“tomcat7-maven-plugin”这样的maven插件来使我们方便地将项目部署到服务器中的tomcat，下面就是我对配置步骤的总结。

# Maven中的配置 {#maven-config}
## pom文件配置 {#pom-config}
```
<plugins>
	<plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
            <url>http://192.168.63.56:50031/manager/text</url>
            <server>tomcat</server>
        </configuration>
    </plugin>
</plugins>
```
注："server"标签要与C:\Users\\{User}\\.m2\settings.xml中配置的用户名和密码的id一致，例如：
```
<server>
    <id>tomcat</id>
    <username>test</username>
    <password>123456</password>
</server>
```
## maven命令的配置 {#maven-cmd}
需要使用tomcat7:redeploy，因为如果tomcat中已经发布好了该项目，使用deploy就无法再次发布。




# Tomcat中的配置 {#tomcat-config}
## 用户配置 {#user-config}
在conf/tomcat-users.xml中添加：
```
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="test" password="123456" roles="manager-gui, manager-script"/>
```
注：user标签中的用户名和密码要与C:\Users\\{User}\\.m2\settings.xml中配置的用户名和密码一致。

## 权限配置 {#privilege-config}
修改webapps/manager/META-INF/context.xml，将Value标签中的allow元素修改为
```
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="192.168.*.*|::1|0:0:0:0:0:0:0:1" />
```
或者直接注释掉该标签也可以。

## war包大小限制 {#size-limie}
Tomcat默认的war包限制为50M，若超过大小，则还需要修改webapps/manager/WEB-INF/web.xml中的
```
<multipart-config>
      <!-- 50MB max -->
    <max-file-size>52428800</max-file-size>
    <max-request-size>52428800</max-request-size>
    <file-size-threshold>0</file-size-threshold>
</multipart-config>
```

通过以上的设置，就可以实现Maven中直接将项目部署到Tomcat。