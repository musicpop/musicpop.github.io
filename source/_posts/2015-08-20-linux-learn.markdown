---
layout: post
title: "linux-learn"
date: 2015-08-20 16:45:44 +0800
comments: true
categories: 
---

bash使用中创建变量，变量是区分大小写的


##grep 命令
grep搜索包含特定模式的文本行，可以在一个或多个文本中搜索。
也可以在输入流中搜索。
###基本的语法是：
	grep pattern filename(或者grep "pattern" filename)
	this is the line containing pattern
grep后可以加一些常用的选项，包括:

*  -c：只输出匹配行的计数。
*  -I：不区分大小写(只适用于单字符)。
*  -h：查询多文件时不显示文件名。
*  -l：查询多文件时只输出包含匹配字符的文件名。
*  -n：显示匹配行及行号。
*  -s：不显示不存在或无匹配文本的错误信息。
*  -v：显示不包含匹配文本的所有行。

###grep 与grep -E（也就是egrep）的区别：
例1，grep命令也可以使用正则表达式来做字符串的匹配，
但需要加转义字符。
为了查找多个字符，可以使用命令：

		 [root@devops ~]# grep 'user1\|user2\|user3' /etc/passwd
		user1:x:501:501::/home/user1:/bin/bash
		user1add:x:503:503::/home/user1add:/bin/bash
		user2:x:504:504::/home/user2:/bin/bash
		user3:x:505:505::/home/user3:/bin/bash

例2，grep命令的扩展版本egrep，带有更复杂的正则表达式元字符，对于以上例子，如果使用egrep，那么可以省略转义字符，如下：

	[root@devops ~]# egrep 'user1|user2|user3' /etc/passwd
	user1:x:501:501::/home/user1:/bin/bash
	user2:x:504:504::/home/user2:/bin/bash
	user3:x:505:505::/home/user3:/bin/bash
###grep还可以在多级目录中对文本进行递归搜索
`grep "text" . -R -n`
###grep 搜索中指定(include)或排除(exclude)文件
目录中递归搜索所有的.c和.cpp文件

`grep "main()" . -r --include *.{c,cpp}`

在搜索中排除所有的README文件

`grep "main()" . -r --exclude "README"`

如果要排除目录，使用--exclude-dir选项
###有时候，我们不打算查看匹配的字符串，而只是想知道是否能够成功匹配。

可以使用grep的静默选项（-q）来实现，
如果命令运行成功会返回0，如果失败则返回非0值

##find命令
工作原理：沿着文件层次结构向下遍历，匹配符合条件的文件。
执行相应的操作。例如，列出base_path目录下的所有文件和文件夹：

`find base_path`

再例如：

`find . -print`

-print指明打印匹配文件的文件名时，'\n'对输出的文件名进行分隔。
-print0指明使用'\0'作为匹配的文件名之间的分隔符，也就是不换行。

###根据文件名进行搜索

使用-name选项，后跟文件名或者包含通配符的文件名

###根据路径名进行搜索

使用-path选项，将文件路径作为一个整体进行匹配

###基于目录深度的搜索

大多数情况下，我们只需要在当前目录下搜索，无需再继续向下搜索。
这时使用深度选项来限制。深度设置为1表示在当前目录下查找，以此类推。
使用-maxdepth指定最大深度，使用-mindepth指定最小深度。

例如：

`find . -maxdepth 1 -name "f*"　-print`

###根据文件类型进行搜索

`find . -type d -pring`

其中：普通文件的类型为f
      符合链接的类型为l
      目录的类型为    d
      字符设备的类型为c
      块设备的类型为  b
      套接字的类型为  s
      FIFO的类型为    p
###根据文件时间进行搜索
Linux文件系统中的文件都有三种时间戳：

* 1 访问时间（）：用户最近一次访问文件的时间
* 2 修改时间（）：文件内容最后一次被修改的时间
* 3 变化时间（）：文件元数据（）最后一次改变的时间
* 
例如：打印出访问时间超所7天的所有文件

`find . -type f -atime +7 -print`

其中指定-atime选项时，单位是天，参数值带有-表示小于，+表示大于，不带有符合表示当天。

###根据文件大小的搜索
使用-size选项：

`find . -type f -size +2k`

###删除匹配的文件

-delete可以用来删除查找到的文件

`find . -type f -name "*.swap" -delete`

###基于文件权限和所有权进行匹配

-perm 匹配特定的权限值

`find -type f -name "*.java" ! -perm 644`

上面的例子，找出没有设置好权限的文件-user 选项找出特定用户所拥有的文件

`find . -type f -user peng -print`
###find与-exec结合

我们查找出来的内容，不只是看一下而已，还会有进一步的操作，这时候-exec的作用就体现出来了。-exec 后面只能接受单个命令，{}表示查找出的文件名。以`;`结束，为了考虑各个系统`;`会有不同的意义,所以在前面加反斜杠。例如：

`find . -type f -name "*.txt" -exec printf "Text file: %s\n" {} \;`

如果后面要接多个参数，可以把多个命令写到一个shell脚本中。例如：

`-exec ./command.sh {} \;`

###find排除目录及文件

-prune 选项，就表示，不寻找字符串作为寻找文件或目录的范本样式。
例如：

	ls -l
	
	total 4
	
	drwxrwxr-x. 2 peng peng 4096 Oct 12 17:10 fold1
	
	`-rw-rw-r--. 1 peng peng    0 Oct 12 16:34 t1.txt
	
	`-rw-rw-r--. 1 peng peng    0 Oct 12 16:35 t2.txt
	
	`-rw-rw-r--. 1 peng peng    0 Oct 12 17:10 test.git

	[peng@localhost learn]$ find . -path "./fold1"
	/fold1

	[`peng@localhost learn]$ find . -path "./fold1" -prune`
	./fold1`
 
find . -path "./fold1" 是基本的查询命令。-prone就像一个判断语句,当发现-prune前面的表达式match时，返回真。

`[peng@localhost learn]$ find . -path "./fold1" -prune -o -print`
`.`
`./t1.txt`
`./t2.txt`
`./test.git`

当使用-o选项，表示逻辑或。如果其前返回真，则不执行后面的-print

如果其前返回假，则会执行-print并输出不包含./fold1的目录。

注意：-a 应该是and的缩写，意思是逻辑运算符‘或’(&&); -o应该是or的缩写,意思是逻辑运算符‘与’(||), -not 表示非.
比较下面的代码：

默默地

`[peng@localhost learn]$ find . -path "./fold1" -prune -a -print`

`./fold1`
 
##xargs命令

为什么要使用xargs命令呢？我们可以使用管道将一个命令的stdout
重定向到另一个命令。例如

`cat foo.txt | grep "test"`

但是有些命令只能使用命令的参数接受数据，而无法通过stdin接收数据流。
那么，我们就设法用管道提供那些只有命令行参数需要的数据。
xargs命令将标准输入数据转换成命令行参数。
基本的语法是：

`command | xargs`

###第一步把stdin接收的数据重新格式化
###第二步将其作为参数提供给其他命令
