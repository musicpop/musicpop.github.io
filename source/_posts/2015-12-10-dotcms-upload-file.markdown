---
layout: post
title: "DotCMS使用中的问题"
date: 2015-12-10 09:55:11 +0800
comments: true
categories: 
---

##DotCMS如何上传资源
dotcms 是一个Java开发的功能强大的内容管理系统(CMS)。开源且跨平台，速度和嵌入式脚本语言使dotCMS易于扩充和建构。因为dotCMS使用hibernate来抽象所有的数据访问，数据库移植很方便。目前支持dotCMS数据库,包括:Oracle，PostgreSQL，Microsoft SQL Server，MySQL。dotCMS是一个可扩展的网页内容管理平台。

在使用的过程中，需要上传资源到后台，根据其文档，我使用了cyberduck工具，[https://cyberduck.io/](https://cyberduck.io/)

新建链接时，端口设置、路径设置都需要正确才可以，见下图：


![小黄鸭](images/dotcms1.png)

##在一次修改代码后，突然出现了下图的错误

![小黄鸭](images/dotcms-wenti.png)

原来是将velocity模板的‘#‘号符，写在了html的注释里面，本想将其注释掉，但是velocity会先进行解析，解析'#'号符时，自然也就出错了。

