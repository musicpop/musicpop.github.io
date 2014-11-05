---
layout: post
title: "GCC and Make"
date: 2014-09-17 16:10:27 +0800
comments: true
categories: 
---
&emsp;&emsp;GCC 开始是Gun C Complier，后来由于加入各种语言支持，能够编译java,c++等。慢慢演变为Gun Complier Collection。这节的内容包括Gcc介绍，Makefile介绍，GDB应用,Eclipse集成环境。

##Gcc编译过程

*预处理：gcc -E main.c -o main.i

*编译  ：gcc -S main.i -o main.s

*汇编  ：gcc -c main.s -o main.o

*链接  ：gcc main.o -o main

##调试

*编译文件并加入调试信息 gcc -O crash.c -o crash 

*使用gdb调试 gdb crash 

*加入断点 b 3
 
*开始调试 run

next 执行一行源码但不进入函数内部

step 执行一行源码且进入函数内部

continue 继续执行

quit 终止


##GCC编译过程中需要的库依赖
-头文件
    搜索的路径包括：C_INCLUDE_PATH  CPLUS_INCLUDE_PATH OBJ_INCLUDE_PATH

    通过-l选项添加搜索目录

-静态库

    通过-L选项添加搜索目录

    使用环境变量LD_LIBRARY_PATH

-动态库

    a.把库拷贝到/usr/lib，/lib，/usr/local/lib目录下
 
    b.添加库路径到LD_LIBRARY_PATH

    c.修改/etc/ld.so.conf文件，把库文件所在的目录放在文件末尾，并执行ldconfig刷新

##如何生成动态库

*gcc -c main.c xiangjia.c xiangjian.c   生成.o文件

*此时执行gcc main.o -o main会出错，提示没有定义xiangjia 和 xiangjian

*ar cr libmath56.a xiangjia.o xiangjian.o 此时生成.a文件

*gcc main.o -o main libmath56.a 这个时候运行成功，生成main文件



