---
layout: post
title: "spring-boot"
date: 2015-06-08 12:00:38 +0800
comments: true
categories: 
---
Spring Boot用来创建独立的Java应用程序，并接可以用java -jar命令运行，也可创建传统的WAR文件。由于Spring使用了大量的XML配置文件以及复杂的依赖管理，这一点饱受非议。而Spring Boot充分利用了JavaConfig的配置模式以及“约定优于配置”的理念，能够极大的简化基于Spring MVC的Web应用和REST服务的开发。 Spring Boot 简化了部署的过程，其内嵌了Tomcat和Jety。 使用gradle和maven构建 使用时只需将org.springframework.boot设置为父项目 *如果想生成War包，并部署到外部容器，
参考http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#build-tool-plugins-maven-packaging