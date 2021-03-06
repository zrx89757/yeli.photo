---
author: yeyeli
type: image
featimg: http://upload-images.jianshu.io/upload_images/1271438-15e7581ca1a8bce3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
title: 初学Linux
layout: post-full
---


<a href="http://linux.vbird.org/new_linux.php" target="_blank">参考资料:鸟哥的Linux私房菜</a>
##### 常用命令

查看内存: `free -h`

查看档案内容:
* cat : 将档案内容全部列出
* head : 只列出档案前10行
* tail : 列出档案最后10行

ssh登录保持长连接 : `ssh -o serveraliveinterval=60 root@23.126.105.39 -p 27573
`

查看8000端口是否被占用: `lsof -i:8000`

***
##### 有关拷贝
递归拷贝命令(将文件夹及目录下的所有文件拷贝) : `cp -r upload_assets /root/`

本地拷贝文件到远程:`scp MP_verify_oixkR51uu2uTOAMq.txt root@139.176.83.122:/var/WWW/HTML`

远程拷贝文件到本地:`scp root@139.176.83.122:/root/backup/201612201009bak.sql  /Users/yetongxue/desktop`

使用端口号登陆的拷贝:
远程拷本地:`scp -P 29481 root@23.126.105.39:/root/my_key/ca.cert.pem ./`

本地拷远程:`scp -P 29481  yeli.key root@23.126.105.39:/root/my_website_server`

***
##### 系统权限
查看文件目录权限: ll -d dirname(查看目录下文件权限: ll dirname)

单纯的档案权限查看，可以使用 ls -l 或 ll查阅,输出:`drwxr-xr-x 4 root root  4096 Dec 31 01:40 my_website`

输出内容说明如下:
* -: 代表後面的檔名為一般檔案
* d: 代表後面的檔名為目錄檔
* l: 代表後面的檔名為連結檔 (有點類似 windows 的捷徑概念)
* b: 代表後面的檔名為一個裝置檔，該裝置主要為區塊裝置，亦即儲存媒體的裝置較多
* c: 代表後面的檔名為一個週邊裝置檔，例如滑鼠、鍵盤等

然后之后的九个符号平分成3组：分别代表user/group/其他 这三类用户所对应的权限：
* r: read，可讀的意思
* w: write，可寫入/編輯/修改的意思
* x: eXecutable，可以執行的意思

修改拥有者：`chown user my_website`

![](http://upload-images.jianshu.io/upload_images/1271438-6e02f46a6da75fe4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***
##### 有关vim

![](http://upload-images.jianshu.io/upload_images/1271438-bdb15b2c9303aea2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

字符移动(支持字符前缀: 10j ,向下移动10行):
`左:h     下:j     上:k     右:l`


单词移动:
* w:移动到下一个单词词首
* b:移动到上一个单词词首
* e:移动到下一个单词词尾
* ge:移动到上一个单词词首
* ^:到行首, $:到行尾

***
##### BASH

查看是否是bash 內建命令：`type -a ls`

Linux语言设置：`/etc/locale.conf`

login shell
**bash 环境变量的设置:**

1. /etc/profile：这是系统整体的设定，你最好不要修改这个档案；
2. ~/.bash_profile 或~/.bash_login 或~/.profile：属于使用者个人设定，你要改自己的资料，就写入这里！(备注：系统读取个人设定的环境变量时，依据这三个文件的优先次序读取，一旦文件存在则终止后面文件的读取)

**BASH变量中单双引号是有区别的,双引号才能让变量保持原来的内容,而单引号就只是变量名而已了**

查看系统以及自定义变量: set

取消变量: unset 变量名称

将自定义变量转为环境变量: `exprot 变量名称`

声明变量:`declare / typeset [-aixr] varibale ` ,其中:
* -a:数组类型
* -i:整型
* -x:用法与export一样,将varibale变成环境变量
* -r: 设定varibale只读,不可修改,也不可unset


由于/etc/profile 与~/.bash_profile 都是在取得login shell 的时候才会读取的设定档，所以， 如果你将自己的偏好设定写入上述的档案后，通常都是得登出再登入后，该设定才会生效。那么，能不能直接读取设定档而不登出登入呢？可以的！那就得要利用source 这个指令了:`source ~/.bash_profile`

管线命令(pipe):`ls -al /etc | less`,说明:管线前面的命令结果作为管线和面命令的输入,前后命令先后执行

***
##### shell script

例子：

```
#!/bin/bash  #宣告脚本使用的shell: bash
# Program:
# This program shows "Hello World!" in your screen.
# History:
# 2015/07/16 VBird First release      #脚本基本信息:创建者/功能等等
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin     #设定环境变量
export PATH      #设置成全局变量
echo -e "Hello World! \a \n”    #输出 hello world
exit 0    #设定回传值 (备注:回传值得查看: 执行完毕之后在终端执行echo $?)
```

最好在自己主目录下面创建一个bin ,将编写好的脚本统一放在里面管理；

第一行#!/bin/bash在宣告这个script使用的shell名称：
因为我们使用的是bash ，所以，必须要以『#!/bin/bash』来宣告这个档案内的语法使用bash的语法！
* script 的功能；
* script 的版本资讯；
* script 的作者与联络方式；
* script 的版权宣告方式；
* script 的History (历史纪录)；
* script 内较特殊的指令，使用『绝对路径』的方式来下达；
* script 运作时需要的环境变数预先宣告与设定。

***
##### Linux 的目录


/bin：bin是Binary的缩写, 这个目录存放着最经常使用的命令。

/boot：这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。

/dev ：dev是Device(设备)的缩写, 该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。

/etc：这个目录用来存放所有的系统管理所需要的配置文件和子目录。

/home：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

/lib：这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。

/lost+found：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

/media linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

/mnt：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

/opt： 这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

/proc：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：

echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all

/root：该目录为系统管理员，也称作超级权限者的用户主目录。

/sbin：s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。

/selinux： 这个目录是Redhat/CentOS所特有的目录，Selinux是一个安全机制，类似于windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

/srv： 该目录存放一些服务启动之后需要提取的数据。

/sys：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。

sysfs文件系统集成了下面3种文件系统的信息：针对进程信息的proc文件系统、针对设备的devfs文件系统以及针对伪终端的devpts文件系统。
该文件系统是内核设备树的一个直观反映。

当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

/tmp：这个目录是用来存放一些临时文件的。

/usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与windows下的program files目录。

/usr/bin：系统用户使用的应用程序。

/usr/sbin：超级用户使用的比较高级的管理程序和系统守护程序。

/usr/src：内核源代码默认的放置目录。

/var：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。


**在linux系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。**


/etc： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。

/bin, /sbin, /usr/bin, /usr/sbin: 这是系统预设的执行文件的放置目录，比如 ls 就是在/bin/ls 目录下的。

值得提出的是，/bin, /usr/bin 是给系统用户使用的指令（除root外的通用户），而/sbin, /usr/sbin 则是给root使用的指令。

/var： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在/var/log 目录下，另外mail的预设放置也是在这里。


