---
layout: post
title: "根据端口号查找进程"
date: 2014-09-17 21:00:18 +0800
comments: true
categories: 
---
 lsof -Pnl +M -i4 | grep 20880 
