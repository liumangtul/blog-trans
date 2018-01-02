### 常用指令
| 指令                                | 说明                 |
| -------------------- |:-------------:|
| ls | 显示文件或目录|
|-l |列出文件详细信息l(list)|
| -a|列出当前目录下所有文件及目录，包括隐藏的a(all)|
|mkdir|创建目录|
|p|创建目录，若无父目录，则创建p(parent)|
|cd|切换目录|
|touch|创建空文件|
| echo|创建带有内容的文件。|
|cat |查看文件内容|
|cp|拷贝|
|mv|移动或重命名|
|rm|删除文件|
| -r|递归删除，可删除子目录及文件|
| -f|强制删除|
|find|在文件系统中搜索某文件|
|wc|统计文本中行数、字数、字符数|
|grep|在文本文件中查找某个字符串|
|rmdir|删除空目录|
|tree|树形结构显示目录，需要安装tree包
|pwd|显示当前目录
|ln|创建链接文件
|more、less|分页显示文本文件内容
|head、tail|显示文件头、尾内容
|ctrl+alt+F1|命令行全屏模式

### 系统管理命令
| 指令                                | 说明                 |
| -------------------- |:-------------:|
|stat|显示指定文件的详细信息，比ls更详细
|who|显示在线登陆用户
|whoami|显示当前操作用户
|hostname|显示主机名
|uname|显示系统信息
|top|动态显示当前耗费资源最多进程信息
|ps|显示瞬间进程状态
|ps -aux du|查看目录大小
|du -h |/home带有单位显示目录信息
|df|查看磁盘大小
|df -h|带有单位显示磁盘信息
|ifconfig|查看网络情况
|ping|测试网络连通
|netstat|显示网络状态信息
|man|命令不会用了，找男人 如：man ls
|clear|清屏
|alias|对命令重命名 如：alias showmeit="ps -aux" ，另外解除使用unaliax showmeit kill 杀死进程，可以先用ps 或 top命令查看进程的id，然后再用kill命令杀死进程。

### 打包压缩相关命令

	gzip：

    	bzip2：

    		tar:                打包压缩

         		-c              归档文件

         		-x              压缩文件

         		-z              gzip压缩文件

         		-j              bzip2压缩文件

         		-v              显示压缩或解压缩过程 v(view)

        		 -f              使用档名

例：

    tar -cvf /home/abc.tar /home/abc
	//只打包，不压缩

    tar -zcvf /home/abc.tar.gz /home/abc
	//打包，并用gzip压缩

    tar -jcvf /home/abc.tar.bz2 /home/abc
	//打包，并用bzip2压缩

###### 当然，如果想解压缩，就直接替换上面的命令
	tar -cvf  / tar -zcvf  / tar -jcvf 中的“c” 换成“x”

### 关机/重启机器
    shutdown

        -r             //关机重启

        -h             //关机不重启

  		now          //立刻关机

    	halt               //关机

    	reboot          //重启
### Linux管道
将一个命令的标准输出作为另一个命令的标准输入。也就是把几个命令组合起来使用，后一个命令除以前一个命令的结果。

   例：
   ```
   grep -r "close" /home/* | more
   //在home目录下所有文件中查找，包括close的文件，并分页输出。
   ```
   ### Linux软件包管理
   dpkg (Debian Package)管理工具，软件包名以.deb后缀。这种方法适合系统不能联网的情况下。
   比如安装tree命令的安装包，先将tree.deb传到Linux系统中。再使用如下命令安装。

    sudo dpkg -i tree_1.5.3-1_i386.deb
	//安装软件

    sudo dpkg -r tree
	//卸载软件
注：将tree.deb传到Linux系统中，有多种方式。VMwareTool，使用挂载方式；使用winSCP工具等；
APT（Advanced Packaging Tool）高级软件工具。这种方法适合系统能够连接互联网的情况。
依然以tree为例

    sudo apt-get install tree
	//安装tree

    sudo apt-get remove tree
	//卸载tree

    sudo apt-get update
	//更新软件

    sudo apt-get upgrade
	//将.rpm文件转为.deb文件

	//.rpm为RedHat使用的软件格式。在Ubuntu下不能直接使用，所以需要转换一下。
    sudo alien abc.rpm

### vim使用
vim三种模式：命令模式、插入模式、编辑模式。使用ESC或i或：来切换模式。
命令模式下：

    :q
	//退出

    :q!
	//强制退出

    :wq
	//保存并退出

    :set number
	//显示行号

    :set nonumber
	//隐藏行号

    /apache
	//在文档中查找apache 按n跳到下一个，shift+n上一个

    yyp
	//复制光标所在行，并粘贴

    h(左移一个字符←)、j(下一行↓)、k(上一行↑)、l(右移一个字符→)

### 用户及用户组管理

    /etc/passwd    存储用户账号

    /etc/group       存储组账号

    /etc/shadow    存储用户账号的密码

    /etc/gshadow  存储用户组账号的密码

    useradd 用户名

    userdel 用户名

    adduser 用户名

    groupadd 组名

    groupdel 组名

    passwd root     给root设置密码

    su root

    su - root

    /etc/profile     系统环境变量

    bash_profile     用户环境变量

    .bashrc              用户环境变量

    su user              切换用户，加载配置文件.bashrc

    su - user            切换用户，加载配置文件/etc/profile ，加载bash_profile

### 更改文件的用户及用户组

    sudo chown [-R] owner[:group] {File|Directory}

 例如：还以jdk-7u21-linux-i586.tar.gz为例。属于用户hadoop，组hadoop
要想切换此文件所属的用户及组。可以使用命令。

    sudo chown root:root jdk-7u21-linux-i586.tar.gz

### 文件权限管理

    三种基本权限

        R           读         数值表示为4

        W          写         数值表示为2

        X           可执行  数值表示为1

例如：
	 	jdk-7u21-linux-i586.tar.gz文件的权限为-rw-rw-r--
		-rw-rw-r--一共十个字符，分成四段。
		第一个字符“-”表示普通文件；这个位置还可能会出现“l”链接；“d”表示目录
		第二三四个字符“rw-”表示当前所属用户的权限。   所以用数值表示为4+2=6
		第五六七个字符“rw-”表示当前所属组的权限。      所以用数值表示为4+2=6
		第八九十个字符“r--”表示其他用户权限。              所以用数值表示为2
		所以操作此文件的权限用数值表示为662

### 更改权限

    sudo chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]   目录名

   例如：
   有一个文件filename，权限为“-rw-r----x” ,将权限值改为"-rwxrw-r-x"，用数值表示为765

    sudo chmod u+x g+w o+r  filename
    //上面的例子可以用数值表示
    sudo chmod 765 filename

----------
#### 1.目录操作
| 命令名  | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
| mkdir  | 创建一个目录  | mkdir dirname  |
| rmdir  | 删除一个目录  | rmdir dirname  |
| mvdir  |移动或重命名一个目录   |  mvdir dir1 dir2 |
| cd  | 改变当前目录  | cd dirname  |
| pwd  | 显示当前目录的路径名  | pwd  |
| ls  | 显示当前目录的内容  | ls -la  |
|dircmp|比较两个目录的内容|dircmp dir1 dir2|

#### 2.文件操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
| cat  | 显示或连接文件  | cat filename  |
|pg|分页格式化显示文件内容|pg filename|
|more	|分屏显示文件内容|	more filename|
|od|	显示非文本文件的内容	|od -c filename|
|cp	|复制文件或目录|	cp file1 file2|
|rm|	删除文件或目录|	rm filename|
|mv	|改变文件名或所在目录|	mv file1 file2|
|ln|	联接文件	|ln -s file1 file2|
|find|	使用匹配表达式查找文件|	find . -name "*.c" -print|
|file|	显示文件类型|	file filename|
|open|	使用默认的程序打开文件|	open filename|


### 3.选择操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|head|	显示文件的最初几行|	head -20 filename|
|tail|	显示文件的最后几行|	tail -15 filename|
|cut	|显示文件每行中的某些域|	cut -f1,7 -d: /etc/passwd|
|colrm|	从标准输入中删除若干列|	colrm 8 20 file2|
|paste|	横向连接文件|	paste file1 file2|
|diff|	比较并显示两个文件的差异|	diff file1 file2|
|sed|	非交互方式流编辑器|	sed "s/red/green/g" filename|
|grep|	在文件中按模式查找|	grep "^[a-zA-Z]" filename|
|awk|	在文件中查找并处理模式|	awk '{print $1 $1}' filename|
|sort	|排序或归并文件|	sort -d -f -u file1|
|uniq|	去掉文件中的重复行|	uniq file1 file2|
|comm|	显示两有序文件的公共和非公共行|	comm file1 file2|
|wc|	统计文件的字符数、词数和行数|	wc filename|
|nl	|给文件加上行号	|nl file1 >file2|
### 4.安全操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|passwd	|修改用户密码|	passwd|
|chmod	|改变文件或目录的权限|	chmod ug+x filename|
|umask	|定义创建文件的权限掩码	|umask 027|
|chown|	改变文件或目录的属主|	chown newowner filename|
|chgrp	|改变文件或目录的所属组|	chgrp staff filename|
|xlock	|给终端上锁	|xlock -remote|
### 5.编程操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|make|	维护可执行程序的最新版本	|make|
|touch|	更新文件的访问和修改时间|	touch -m 05202400 filename|
|dbx	|命令行界面调试工具|	dbx a.out|
|xde|	图形用户界面调试工具	|xde a.out|
### 6.进程操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|ps|	显示进程当前状态|	ps u|
|kill	|终止进程|	kill -9 30142|
|nice	|改变待执行命令的优先级|	nice cc -c *.c|
|renice|	改变已运行进程的优先级	|renice +20 32768|
### 7.时间操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|date|	显示系统的当前日期和时间	|date|
|cal	|显示日历|	cal 8 1996|
|time|	统计程序的执行时间|	time a.out|
### 8.网络与通信操作
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|telnet	|远程登录|	telnet hpc.sp.net.edu.cn|
|rlogin|	远程登录|	rlogin hostname -l username|
|rsh	|在远程主机执行指定命令|	rsh f01n03 date|
|ftp|	在本地主机与远程主机之间传输文件|	ftp ftp.sp.net.edu.cn|
|rcp|	在本地主机与远程主机 之间复制文件	|rcp file1 host1:file2|
|ping	|给一个网络主机发送 回应请求|	ping hpc.sp.net.edu.cn|
|mail|	阅读和发送电子邮件	|mail|
|write	|给另一用户发送报文|	write username pts/1|
|mesg|	允许或拒绝接收报文|	mesg n|
### 9.Korn Shell 命令
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
|history	|列出最近执行过的 几条命令及编号|	history|
|r	|重复执行最近执行过的 某条命令|	r -2|
|alias|	给某个命令定义别名	|alias del=rm -i|
|unalias|	取消对某个别名的定义	|unalias del|
### 10.其它命令
|  命令名 | 功能描述  | 使用举例  |
| :------------: | :------------: | :------------: |
 |uname	 |显示操作系统的有关信息 |	uname -a |
 |clear |	清除屏幕或窗口内容 |	clear |
 |env |	显示当前所有设置过的环境变量 |	env |
 |who	 |列出当前登录的所有用户 |	who |
 |whoami	 |显示当前正进行操作的用户名 |	whoami |
 |tty |	显示终端或伪终端的名称 |	tty |
 |stty |	显示或重置控制键定义 |	stty -a |
 |du |	查询磁盘使用情况 |	du -k subdir |
 |df |	显示文件系统的总空间和可用空间 |	df /tmp |
 |w	 |显示当前系统活动的总信息 |	w |