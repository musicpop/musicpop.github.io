---
layout: post
title: "dotcms数据-迁移服务器"
date: 2015-12-11 18:11:07 +0800
comments: true
categories: 
---
##导出本地数据库

![导出](images/dotcms-deploy.png)

##将数据导入部署机器 数据库
引用要导入的目标数据库 
	(use temp_test_db) 
	
数据库temp_test_db数据库必须存在。
导入命令 
source 数据文件目录(source E:\data.sql)
需要一定时间，和导入文件大小相关

##将代码部署到服务器Tomcat目录ROOT目录下

##修改 ROOT/Meta-info/context.xml 中的连接数据库的用户名及密码

##如果启动的时候提示分配的内存不足，可以创建setenv.bat文件，输入
	set JAVA_OPTS=-Dfile.encoding=UTF-8 -Xms128m -Xmx1024m
将这个文件保存在Tomcat bin目录下。这里没有设置 -XX:MaxPermSize，因为在新版本的java中已经移除了此设置。


