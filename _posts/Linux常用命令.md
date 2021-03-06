---
title: Linux常用命令
date: 2016-10-06 22:30:00
categories:
- Linux
---

## Linux命令格式
格式：命令	[选项]	[参数]

例如：
```
#ls 	-list 	显示目录下内容
```
> 命令操作的对象（文件、目录、用户、进程）
> Linux中以”.”开头的文件是隐藏文件

提示符：[root@localhost src]#
> [当前登录用户@主机名 当前所在目录]#
> #	超级用户
> $	普通用户

当前所在目录：	用户家目录
>	管理员		/root
>	普通用户		/home/用户名

**tips：**
> ctrl +  c		强制终止
> ctrl+l			清屏
> ctrl+u		光标删除到行首
> ctrl+a		光标移动到行首
> ctrl+e		光标移动到行尾

<!-- more -->

## 目录操作命令
**切换所在目录**

`cd`

例：
```
cd  /usr/local/src
cd   ~	或cd	  	进入当前用户的家目录
cd  -		进入上次目录
cd  ..		进入上一级目录
cd  .		进入当前目录
```
**显示当前所在目录**

`pwd`

**建立目录**

`mkdir`

例：
```
mkdir  	-p  11/22/33/44		递归建立目录
```
**删除目录**
```
rmdir	目录		只能删除空目录

rm	文件名		删除文件
rm	-rf	目录		删除文件和目录
		-f	强制
		-r	递归、删除目录
```
**显示指定目录下的所有内容的目录树**

tree	目录名

例：
```
tree	/var/
```

## 文件操作命令
**创建空文件或修改文件时间**
```
touch  文件名
```
**删除**
```
rm  –rf  文件名
		-f	强制
		-r	删除目录
```
**查看文件内容**
```
cat	文件名		（从头到尾）
		-n	列出行号
more	文件名		（分屏显示文件内容）
空格向下翻		b  向上翻		q  退出
head	文件名		（显示文件头几行）
	head  –n  行数	文件名	指定显示文件头几行
	head  -20  文件名
```
**链接文件**
```
ln  -s  源文件   目标文件		（文件名都必须写绝对路径）
链接文件相当于windows中的快捷方式，链接文件和源文件修改一个两个都变，删除原文件，软链接打不开。
```
## 文件和目录都能操作的命令

**删除文件或目录**
```
rm
```

**复制**
```
cp  源文件   目标文件
		-r	复制目录
		-p	带文件属性复制
		-d	若源文件为链接文件，则复制链接属性
		-a	相当于  -pdr
例：
cp   aa  /tmp/			原名复制
cp  aa  /tmp/bb			改名复制
```

**剪切或改名**
```
mv	源文件   目标位置
剪切目录不需要加“-r”选项（特殊）
例：
mv  /root/aa  /tmp/	剪切
mv  aa  bb			改名
```
## 权限管理

**权限位**
```
-rw-r--r--   1   root root     0 08-11 01:45 aa
```
权限位是十位
第一位：代表文件类型
> \-	普通文件
>	d	目录文件
>	f	链接文件

常见的文件类型就有以上3种，Linux和Unix系统中共有7种文件类型，分别有
	-	Block		块设备文件，如某个磁盘分区，软驱和光驱等。
	-	Char		字符设备，如键盘、打印机等。
	-	Dir		目录类型，目录也是文件的一种。
	-	Fifo		命名管道，常用于将信息从一个进程传递到另一个进程。
	-	File		普通文件。
	-	Link		符号链接文件，即指向文件指针的指针。
	-	Unknown	未知类型

其他九位：属主权限u		属主权限g		其他人权限o
	-	r	读		4
	-	w	写		2
	-	x	执行		1

**修改权限**

chmod
> 格式：chmod  {u,g,o }{+,-,=}{w,r,x} 文件名或目录

例：
```
chmod  u+x  aa
chmod u-x  aa
chmod  g+w,o-r   aa
chmod  u=rwx  aa
chmod  755  aa
chmod  644  aa
```

**权限对于文件和目录的意义**

对于文件的意义
- r：读取文件的内容。cat、more、head、tail
- w：编辑、修改文件内容。vi、echo
- x：可执行

对于目录的意义
- r：可以查询目录下的文件名。ls
-	w：具有修改目录结构的权限。比如新建文件和目录，删除此目录下文件和目录，剪切等操作。touch、mv、cp
-	x：可以进入目录	。	cd

对于目录来说，r和x权限一般是一起给的，及对于目录的权限一般至少会给到6.

**更改文件或目录的属主和属组命令**

chown
> 格式：chown  用户名  文件名  

例
```
chown user1  aa
chown  user1：user1   aa	（更改属主的同时改变属主）
```

## 帮助命令
**查找命令的的帮助**
```
man  命令名
```

**查看常见的选项**
```
命令  --help
```
Tips：Linux中通配符和正则的使用原则：
- 在搜索目录或者文件名时Linux使用的是通配符，如find.		
> Find:	在系统当中搜索符合条件的文件名，如果需要匹配，使用通配符匹配
通配符是完全匹配

- 在搜索文件中的内容是，Linux使用的事正则表达式，如grep.		
> Grep：	在文件当中搜索符合条件的字符串，如果需要匹配，使用正则表达式匹配
正则表达是包含匹配

## 查找命令
**查找命令的命令，同时查看帮助文档的位置**

```
whereis
格式：whereis   命令
```
**搜索命令**
```
find
格式：find   查找位置   -name   文件名
例
find /   -name   aabbcc    
		-user   用户名		按属主用户查找文件
		-group   组名		按属组组名查找文件
		-nouser			查找没有属主的文件
按照文件属性查找：
		-name			按照文件名
		-size 				文件大小。+50k：大于50k，-50k：小雨50k，50k：等于50k	（可用的单位有k，M）
		-type				文件类型。（f:普通文件，l：链接文件，d目录文件）
		-perm	644		按文件的权限值查找。
		-iname			文件名查找，不区分大小写
		-inum				按照文件i节点查找
在查找出的结果中，直接进行命令操作。
find  /var/log/   -mtime   +10  -exec   rm  -rf  {}  \;	查找出十天前修改的文件名删除
find   /root  -inum   112033   -exec  ls   -l   {}  \;	用文件i节点查找到文件并且显示文件的长格式。
```
**查找文件中的字符串**
```
grep
原理：先把命令操作的结果存在文件中，然后后面的命令去操作文件
格式：grep  “字符串”   文件名
例：
grep  -i  “root” /etc/passwd
		-i	忽略大小写
		-v	反向选择
比较：
find：在系统当中搜索符合条件的文件名，如果需要匹配，使用通配符匹配。通配符是完全匹配。
grep：在文件当中搜索符合条件的字符串，如果需要匹配，使用正则表达式匹配，正则表达式是包含匹配
```
**管道符**
```
命令1  |  命令2  |   命令3		命令1的执行结果，作为命令2执行的执行条件，以此类推。
例：
netstat   -an  |  grep  ESTABLISHED  |  wc  -l	统计正在连接的网络连接的个数
cat  文件名  |  grep  “字符串”  	提取含有字符串的行
ls  -l  /etc  |  more 		分屏显示ls内容
ll  /etc、  |  grep  my
```

## 压缩和解压缩
Linux中常见的压缩包格式
> .gz		bz2		Linux可以识别的常见压缩格式
>.tar.gz	.tar.bz2	常见的压缩和打包命令

**压缩打包命令**
- 压缩同时打包
```
tar  -zcvf   压缩文件名	源文件
tar  -zcvf  aa.tar.gz   aa
	-z:识别.gz格式
	-c:压缩
	-v:显示压缩过程
	-f:指定压缩包名
```
- 解压缩同时解打包
```
tar   -zxvf  压缩文件名
	-x: 解压缩
```
- 压缩同时打包
```
tar  -jcvf  压缩文件名	源文件
	-j：识别.tar.gz2文件格式
```
- 解打包同时解压缩
```
tar  -jxvf  aa.tar.bz2  
```
- 查看不解包
```
tar   -ztvf   aa.tar.gz
tar  -jtvf   aa.tar.bz2
	-t:  只查看，不解压
```
- 解压到指定压缩位置
```
tar  -jxvf  root.tar.bz2   -C  /tmp/  	（-C一定要写在后边）
```

## 关闭和重启命令

```
shutdown		没有特殊情况建议使用此命令来关机
shutdown  -h  now	立刻关机
	-h	关机
	-r 	重启

reboot 重启命令
```
## 挂载命令
Linux下所有的存储设备都必须挂载使用，包括硬盘
- 挂载
```
mount
设备文件：
	/dev/sda1		第一个SCSI硬盘的第一个分区
	/dev/cdrom		光盘（链接）
	/dev/hdc		光盘		CentOS 5.5
	/dev/sr0		光盘		CentOS6.x
mount  -t  文件系统  设备描述文件   挂载点（已经存在的空目录）
mount  -t iso9660  /dev/cdrom  /mnt/cdrom(-t iso9660也可省略)
mount  /dev/cdrom  /mnt/cdrom

fdisk  -l		列出指定的外围设备的分区表状况
mount  -t  vfat  /dev/sdb1  /mnt/usb
```
- 卸载
```
umount
umount    /dev/cdrom  
umount   /mnt/cdrom
强调：退出挂载目录，才能卸载
```
## 网络命令
ping测试网络连通性
```
ping   -c  次数  ip 	（测试网络的通畅）
```
ifconfig		查询本机网络信息
>	-a	列出所有的网卡信息（包括没启动的）

## 输出重定向和多命令顺序执行
输出重定向 把应该输出到屏幕的输出，重定向到文件。
```
>	覆盖
>>	追加
ls  >  aa		覆盖到aa
ls  >>  aa		追加到aa
ls  gdlslga  2>>aa		错误信息输出到aa		
强调：错误输出，不能有空格
```
错误信息重定向
```
ls  >>  aa  2>&1		错误和正确都输入到aa，可以追加
		2>&1		把标准错误重定向到标准正确输出

ls  >>  aa  2>>/tmp/bb		正确信息输入aa，错误信息输入bb
```

## 补充命令
date  查看系统时间
```
	date 		
	date	-s   20140701		设定日期
	date	-s	09:30:00		设定时间
```

du统计目录大小
```
		-sh	目录名		统计目录大小
		-s		和
		-h		习惯单位
```
netstat  
```
netstat  		查看网络状态的命令
	-t	查看TCP端口
	-u	查看UDP端口
	-l	监听
	-n	以IP地址和端口号显示，不用域名和服务名显示
	-a	查询所有连接
例
netstat  -tlun		
```
