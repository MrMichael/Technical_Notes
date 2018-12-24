# 目录
[TOC]
# 第一章 Linux 系统简介
Linux 是全球最受欢迎的开源操作系统。它是一个由 C 语言编写的，符合 POSIX 标准的类 UNIX 系统。

Linux 内核源码的网址为： http://www.kernel.org。

## Linux 内核
### Linux 内核的重要特点
1. Linux 是一个一体化内核；
2. 可移植性强。
3. 是一个可裁剪的操作系统内核。
4. 模块化。 
5. 网络支持完善。
6. 稳定性强。
7. 安全性好。
8. 支持的设备广泛。

### Linux 操作系统的特点
1. 开放性
2. 多用户
3. 多任务
4. 良好的用户界面
5. 设备独立性
6. 完善的网络功能
7. 可靠的系统安全
8. 模块化
9. 良好的可移植性

### Linux 内核组成部分
1. 内存管理
2. 进程管理
3. 进程间通信
4. 虚拟文件系统
5. 网络
  ![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCEdfe69e3b3f1aca3a1a1f23160dda7752/7245)

#### 进程管理
进程管理负责控制进程对 CPU 的访问，如任务的创建、调度和终止等。任务调度是进
程管理最核心的工作， 由 Linux 内核调度器来完成。

##### 进程状态
1. 运行态
2. 就绪态
3. 可中断睡眠状态
4. 不可中断睡眠状态
5. 暂停状态
6. 僵死态
  ![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCEe83d8e801ad09049f2690c36468a0d95/7266)

#### 内存管理
内存管理的主要作用是控制和管理多个进程，使它们能够安全地共享主内存区域。当
CPU 提供内存管理单元（MMU）时，内存管理为各进程实现虚拟地址到内存物理地址的转
换。

#### 文件系统
Linux 内核支持众多的逻辑文件系统，如 Ext2、 Ext3、 Ext4、 btrfs、 NFS、 VFAT 等。

#### 网络接口
网络接口提供了对各种网络标准的存取和各种网络硬件的
支持，接口可分为网络协议和网络驱动程序。

#### 进程间通信
##### 支持进程间各种通信机制
- 管道
    - 用于具有亲缘关系的父子进程或者兄弟进程间通信，是半双工的，数据只
      能往一个方向流动，先入先出.如果需要双方互通，必须建立
      两个管道。   
- 命名管道
    - 突破了进程间的亲缘关系限制，即非父子、兄弟进程之间也可相互通信。
- 信号
    - 软件中断，用于在多个进程之间传递异步信号。 
- 消息队列
    - 信号能传递的信息有限，而消息队列则正好弥补了这点
- 内存共享
    - 用于不同进程间进行大量的数据传递。
- 信号量
    - 用于进程同步。 只有获得了信号量的进程才可以运行，没有获得信号量的进
      程就只能等待。
- 套接字等
    - 用于多个进程间通信，可以基于文件，也可基于网络。 

## Linux 发行版
1. RedHat
2. Fedora
3. Madriva
4. Debian
5. Ubuntu
6. SuSe
7. Gentoo
8. Slackware
9. 红旗 Linux

## 嵌入式 Linux
### 嵌入式 Linux 的特点
1. 小型化
2. 实时化

---
# 第二章 安装 Linux 操作系统

## 发行版选择和 ISO 下载
纯净的 Ubuntu 系统：
[ Ubuntu 发行版](https://note.youdao.com/web/#/file/recent/markdown/WEB6fabced67f2118513a7f8321da7d7b88/)

已经构建好嵌入式Linux 开发环境的 Ubuntu 系统：
[zlg Ubuntu 14.04 64bit ISO光盘镜像](http://www.zlg.cn/linux)

## VMware Player 软件
[VMware官方非商用的 VMware Player](https://my.vmware.com/en/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/14_0)

### 虚拟网卡有 3 种模式
- 桥接模式
    - VMWare 虚拟出来的操作系统就像是局域网中的一台独立的主机，它可
      以访问网内任何一台机器

    - 在进行嵌入式 Linux 开发，要目标板通过 NFS 挂载虚拟机的 NFS 共享目录的话，
      必须将虚拟网卡配置为桥接模式。
- NAT 模式
    - 让虚拟系统借助 NAT（网络地址转换）功能，通过宿主机器所在
      的网络来访问公网
- 仅主机模式
    - 所有的虚拟系统是可以相互通信的，但虚
      拟系统和真实的网络是被隔离开的。

---

# 第三章 使用 Linux

## Linux Shell
Linux Shell 一个命令解释器，是 Linux 下最重要的交互界面， 它从标准输入接收用户命
令，将命令进行解析并传递给内核，内核则根据命令，作出相应的动作，如果有反馈信息，
则输出到标准输出上。

Shell 也是一种解释型的程序设计语言，并且支持绝大多数高级语言的程序元素，如变
量、数组、函数以及程序流程控制等。 

### Shell 的种类和特点
- Bourne Shell （sh）
- C Shell（csh）

特性
1. 具有内置命令可供用户直接使用；
2. 支持复合命令：把已有命令组合成新的命令；
3. 支持通配符（*、 ?、 []）；
4. 支持 TAB 键补齐；
5. 支持历史记录；
6. 支持环境变量；
7. 支持后台执行命令或者程序；
8. 支持 Shell 脚本程序；
9. 具有模块化编程能力，如顺序流控制、条件控制和循环控制等；
10. trl+C 能终止进程

### Linux 常见Shell命令
#### 导航命令
```
$ ls [选项]
```
```
$cd 目标路径
```
```
$ pwd
```

#### 目录操作命令
创建目录
```
$mkdir [选项] [参数] 目录
```
用 rmdir 删除空目录

```
$rmdir dir1 dir2
```
用 rm 命令删除
```
//rm 命令既可以删除文件，也可以删除目录而不管目录是否非空。
$rm [选项] 文件/目录
$ rm -fr dir3 
```

#### 文件操作命令
创建空文件
```
$ touch a
```
创建一个有内容的文件
- 使用文本编辑器
- 使用 echo 命令加上重定向可以创建一个带内容的非空文件.
    ```
    $echo 内容 或者 “内容” > 文件
    ```
    查看文件类型
```
$ file 文件
```
查看文件内容
```
$more/less 文件
```
```
$head/tail [选项][参数] 文件
```
```
$ cat 文件
```
文件合并

```
//cat 命令可以将一个或者多个文件输出到标准输出，如果将标准输出重定位到某个文件，则将多个文件合并一个文件。
$ cat [选项] 文件 1 文件 2 … [>文件 3]
```
文件压缩/解压
```
$tar [选项] 文件

//解压 b.tar.gz 文件，并指定解压到/home/chenxibing/目录
$ tar -xzvf b.tar.gz -C /home/chenxibing

//将 drivers 目录的文件打包，创建一个.tar.bz2 压缩文件
$ tar -cjvf drivers.tar.bz2 drivers
```
删除文件
```
$ rm 文件
```
文件改名和移动
```
$mv 源文件/目录 目的文件/目录

//严格来说， Linux 下的文件名是由“路径+文件名”组成的，不同目录的两个同名文件实际
//上不是一个文件，如/home/lpc3250/apps/hello.c 与/home/lpc3250/drivers/hello.c 是两个不同
//文件。所以， Linux 下文件的改名和移动实际上是一回事。
```
文件复制
```
$cp [选项] 源文件/目录 目的文件/目录
```
创建链接
```
$ ln 选项 源文件/目录 目标文件

//硬链接   修改其中任何一个，另外一个都会进行同样的改动。
ln hello.c main.c

//软链接   类似于 Windows 下的快捷方式。
ln -s dir1 dir2
```
改变文件和目录权限
```
$chmod [参数] 文件/目录
$ chmod 777 hello
```

#### 网络操作命令

网络配置

```
$ifconfig 网络接口 [选项] 地址/参数
```

| 选项/参数     | 说明                   | 示例                                       |
| --------- | -------------------- | ---------------------------------------- |
| -a        | 查看系统拥有的全部网络接口        | ifconfig -a                              |
| 网络接口      | 指定操作某个网口             | ifconfig eth0 192.168.1.136              |
| broadcast | 设置网口的广播地址            | ifconfig eth0 broadcast 192.168.1.255    |
| netmask   | 设置网口的子网掩码            | ifconfig eth0 netmask 255.255.255.0      |
| hw ether  | 设置网卡物理地址（如果驱动不支持则无效） | ifconfig eth0 hw ether 00:11:00:00:11:22 |
| up        | 激活指定网卡               | ifconfig eth0 up                         |
| down      | 关闭指定的网卡              | ifconfig eth0 down                       |

ping 命令
```
$ping IP地址
```

#### 安装和卸载文件系统

文件系统挂载
```
# mount [-参数] [设备名称] [挂载点]

//FAT 格式的 U 盘
#mount –t vfat /dev/sda1 /mnt

// NFS 挂载 
#mount -t nfs 192.168.1.138:/home/chenxibing/lpc3250 /mnt -o nolock
//nolock 表示禁用文件锁，当连接到一个旧版本的 NFS 服务器时常加该选项。

//挂载yaffs2 分区
#mount -t yaffs2 /dev/mtdblock2 /mnt

//挂载 ubifs 分区
#mount -t ubifs ubi0:rootfs /mnt
```

文件系统卸载
```
#umount 挂载点
```

#### 使用内核模块和驱动

加载（插入） 模块
```
//能够动态加载和卸载模块是 Linux 引以为豪的特性之一
//Linux 中最常见的模块是内核驱动
# insmod [选项] 模块 [符号名称=值]

# insmod beepdrv.ko
# insmod pcm-8032a.ko irq=3 addr=0x300
```

查看系统已经加载的模块
```
$lsmod
```

卸载驱动模块
```
#rmmod [选项] 模块
#rmmod beepdrv.ko
```

自动处理可加载模块
```
//modprobe 命令集加载/卸载功能于一身
//并且可以自动解决模块间的依赖关系，将某模块所依赖的其它模块全部加载。
# modprobe [选项] 模块 [符号=值]
```

创建设备节点
```
//加载驱动后，则需要为驱动建立对应的设备节点，否则无法通过驱动来操作设备。
#mknod 设备名 设备类型 主设备号 次设备号
#mknod /dev/led c 231 0
```

#### 其它命令

临时获取 root 权限
```
$ sudo 命令
```

文件搜索
```
$find 路径 –选项 其它
$find arch/arm/ -name mux*.c
```

字符串搜索
```
$grep 选项 表达式 [文件]
$grep ―pcf8563‖ -R arch/arm
```

## Shell 文件
Shell 文件是以某种方式将一些命令放在一起得到的文件，常称为 Shell 脚本。 
Shell 文件通常以“#!/bin/sh”开始， #!后面指定解释器.

```
//shell 文件
#!/bin/sh
echo "hello, I am a shell script"
```
增加可执行权限后，在 Shell 中即可运行
```
$chmod +x a.sh

$./a.sh
或$source a.sh
```

## Linux 环境变量
Linux 是一个多用户操作系统，每个用户都有自己专有的运行环境。
用户所使用的环境由一系列变量所定义，这些变量被称为环境变量。系统环境变量通常都是大写的。

### Linux 常见环境变量
| 变量        | 说明                                       |
| --------- | ---------------------------------------- |
| PATH      | 决定了 Shell 将到哪些目录中寻找命令或程序，这个变量是在日常使用中经常需要修改的变量。 |
| TERM      | 指定系统终端                                   |
| SHELL     | 当前用户 Shell 类型                            |
| HOME      | 当前用户主目录                                  |
| LOGNAME   | 当前用户的登录名                                 |
| USER      | 当前用户名                                    |
| HISTSIZE  | 历史命令记录数                                  |
| HOSTNAME  | 指主机的名称                                   |
| LANGUGE   | 语言相关的环境变量，多语言可以修改此环境变量                   |
| MAIL      | 当前用户的邮件存放目录                              |
| PS1       | 基本提示符，对于 root 用户是#，对于普通用户是$              |
| PS2       | 附属提示符，默认是“>”                             |
| LS_COLORS | ls 命令结果颜色显示                              |

在 Shell 下通过美元符号（$）来引用环境变量，
使用 echo 命令可以查看某个具体环境变量的值。
```
$echo $TERM
```

### 修改环境变量
- 通过 Shell 命令设置环境变量
    ```
    $ 变量=$变量:新增加变量值
    $ export 变量=$变量:新增加变量值

    PATH=$PATH:/opt/usr/bin
    ```
- 修改系统配置文件
    - 修改/etc/profile 文件会影响使用本机的全部用户；
    - 修改~/.bashrc 则仅仅影响当前用户；

    运行文件可用“source 文件”的方式操作。

---

# 第四章 Linux 文件系统

## Linux 目录结构
![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCE6938b947987064b97b251f6d8e1ad510/7676)


| 目录    | 说明                          |
| ----- | --------------------------- |
| bin   | 基本命令的程序文件，里面不能再包含目录         |
| boot  | Boot Loader 静态文件            |
| dev   | 设备文件                        |
| etc   | 系统配置文件，配置文件必须是静态文件，不能是二进制文件 |
| home  | 存放各用户的个人数据                  |
| lib   | 基本的共享库和内核模块                 |
| media | 可移动介质的挂载点                   |
| mnt   | 临时的文件系统挂载点                  |
| opt   | 附加的应用程序软件包                  |
| root  | root 用户目录                   |
| sbin  | 基本系统命令的二进制文件                |
| srv   | 系统服务的一些数据                   |
| tmp   | 临时文件                        |
| usr   | /usr/bin 众多的应用程序            |
|       |                             |
|       |                             |
|       |                             |
|       |                             |
|       |                             |
|       |                             |
|       |                             |
| var   | 可变数据                        |

## Linux 的文件
### Linux 文件结构
Linux 下一切都是文件，Linux 下所有文件的描述结构都是相同的，包含索引节点和数据。
![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCE6d3d268b7576971344a417ddbed04814/7699)

- 索引节点
> Linux 文件系统用来记录文件信息的一种数据结构，信息包
> 括文件名、文件长度、文件权限、存放位置、所属关系、创建和修改时间等。
> 每个文件都有一个索引号与之对应，而一个索引节点号，可以
> 对应多个文件。
- 数据
> 文件的实际内容。

```
chenxibing@gitserver-zhiyuan:~$ ls -li examples.desktop
785326 -rw-r--r-- 1 chenxibing root 179 2010-10-15 09:07 examples.desktop
```

| 信息               | 说明                                       |
| ---------------- | ---------------------------------------- |
| examples.desktop | 文件名                                      |
| 785326           | 索引节点号                                    |
| -（第 1 个）         | -表示是普通文件，可能出现的有：                         |
|                  |                                          |
|                  |                                          |
|                  |                                          |
|                  |                                          |
|                  |                                          |
|                  |                                          |
| rw-r--r--        | 文件访问权限： 644                              |
| 1                | 1 表示文件没有硬链接；如果文件有 1 个硬链接则为 2；对于目录，则表示该目录包含的目录文件数（包含隐藏的(.)和(..)） |
| chenxibing       | 文件拥有者                                    |
| root             | 文件所在群组                                   |
| 179              | 文件大小（以字节为单位）                             |
| 2010-10-15 09:07 | 文件修改时间                                   |

文件名以点号(.)开始的是隐藏文件，用 ls 命令不加参数-a 将看不到这类
文件。

## Linux 文件系统
Ext2 属于非日志型文件系统，而 Ext3/4 文件系统是日志型文件系统。
还有proc、Sysfs等


# 第五章 Vi 编辑器

## Vi的模式
- 命令模式
    - 从键盘上输入的任何字符都被作为编辑命令来解释
- 输入模式
    - 从键盘上输入的所有字符都被插入到正在编辑的缓冲区中，被当作正文。

启动 Vi 后处于命令模式，在命令模式下，输入编辑命令（i,a,o），将进入输入模式；
在输入模式下，按 ESC 键将进入命令模式.

## Vi操作

#### 退出 Vi
```
:q  退出未被编辑过的文件
:q! 强行退出 vi，丢弃所做改动
:x  存盘退出 vi
:wq 存盘退出 vi
ZZ  等同于:wq
```

#### 光标移动
在命令模式下光标移动的方法：
- 上： k、 Ctrl+P、 <up_arrow>
- 下： j、 Ctrl+N、 <down_arrow>
- 左： h、 Backspace、 <left_arrow>
- 右： l、 Space、 <right_arrow>

无论在输入模式下还是命令模式下，都支持 Page Up 和 Page Down 翻页。

命令模式下快捷命令
| 命令   | 说明          |
| ---- | ----------- |
| G    | 将光标定位到最后一行  |
| nG   | 将光标定位到第 n 行 |
| gg   | 将光标定位到第 1 行 |
| ngg  | 将光标定位到第 n 行 |
| :n   | 将光标定位到第 n 行 |

#### 文本输入
在命令模式下输入编辑命令（i/I、 a/A、 o/O），就可以进入输入模式，它们之间的差异如表。

| 命令   | 说明             |
| ---- | -------------- |
| a    | 在当前光标位置后面开始插入  |
| A    | 在当前行行末开始插入     |
| i    | 在当前光标前开始插入     |
| I    | 在当前光标行行首开始插入   |
| o    | 从当前光标开始下一行开始插入 |
| O    | 从当前光标开始前一行开始插入 |

#### 文本处理
可在
命令模式下进行快速的文本复制、粘贴、删除、剪切、 查找、替换、撤销和恢复等操作。

- 文本块选定
> 将光标移到将要选定的文本块开始出，按 ESC 进入命令模式，再按 v，
> 进入可视状态，然后移动光标至文本块结尾，被选定的文本块高亮显示，
> 连按两次 ESC 可以取消所选定文本块。

- 复制和粘贴
> 如果已经选定文本块，按 y，即可将所选定文本复制到缓冲区，
> 将光标移到将要粘贴的地方，按 p，就可完成文本粘贴。

> 在命令模式下，连按 yy，即可复制光标所在行的内容，连按 yny 即可复制从光标所在行开始的 n 行。

- 剪切和删除

| 命令     | 说明                     |
| ------ | ---------------------- |
| x 或 nx | 剪切从光标所在的位置开始的一个或 n 个字符 |
| X 或 nX | 剪切光标前的一个或 n 个字符        |
| dd     | 删除光标所在的行               |
| D      | 删除从光标位置开始至行尾           |
| dw     | 删除从光标位置至该词末尾的所有字符      |
| d0     | 删除从光标位置开始至行首           |
| dnd    | 删除光标所在行开始的 n 行         |
| dnG    | 将光标所在行至第 n 行删除         |

- 文本查找
>局部匹配搜索： 用“/字符串”或者“?字符串”方式搜索，
>例如搜索字符串 abc，字符串 abcd 也将会被显示在搜索结果中。

> 全局匹配搜索：先将光标移动到字符串 abc，然后按“SHIFT+*”，完成搜索。

- 文本替换
```
//能够将文本内全部的字符串 old 替换为 new。
:%s /old/new/g
```

- 撤销和恢复
> 在命令模式下输入 u，可撤销所做的更改，恢复编辑前的状态。
> 可以用 Ctrl+R 来恢复。

## 配置 vi

| 命令              | 说明                  |
| --------------- | ------------------- |
| :set number     | 显示行号                |
| :set ignorecase | 不区分大小写              |
| :set tabstop=n  | 按下 tab 键则实际输入 n 个空格 |
| :set hlsearch   | 搜索时高亮显示             |
| :syntax on      | 开启语法高亮              |

Vi 有自己的配置文件，可以是“/etc/vim/vimrc”或者“~/.vimrc”。
两者的区别是前者全局的，影响登录本机的全部用户，后者仅仅对当前用户有效。

```
" 在窗口标题栏显示文件名称
set title
" 编辑的时候将所有的 tab 设置为空格
set tabstop=4
" 设置自动对齐空格数
set shiftwidth=4
" 显示行号
set number
" 搜索时高亮显示
set hlsearch
" 不区分大小写
set ignorecase
" 语法高亮
syntax on

//以"开始的是注释
```

## 文件对比

```
$ vimdiff file1 file2 file3
```

---

# 第六章 嵌入式 Linux 开发环境构建

## 安装交叉编译器

如果以 deb 包形式发布
```
host$ dpkg -i package.deb
```
如果以 bin 方式打包发布
```
host$ chmod +x package.bin
host$ ./package.bin
```
以.tar.bz2 压缩包方式发布
```
host$ tar xjvf package.tar.bz2
```
以 deb 或者 bin 方式发布的工具包，安装后通常会自动设置环境变量；
而以.tar.bz2 方式的发布包，在完成解压后， 如果不设置环境变量，如果不指定交叉编译器的完整路径，系统
是无法调用交叉编译器的。

### 设置环境变量

- 临时设置
```
host$ export PATH=$PATH:/交叉编译器路径
$ echo $PATH

//添加工具链路径
$ export PATH=$PATH:/home/ctools/arm-2011.03/bin/
$ echo $PATH
```
- 修改全局配置文件
```
sudo vi /etc/profile
//在文件末尾添加：
    export PATH=$PATH:/home/ctools/arm-2011.03/bin/
    
. /etc/profile
```

- 修改用户配置文件
```
vi ~/.bashrc或者vi ~/.bash_profile
//在文件末尾添加：
    export PATH=$PATH:/home/ctools/arm-2011.03/bin/
    
. .bashrc或者. .bash_profile   
```

- 测试工具链

- 安装 32 位的兼容库
```
//如果测试不成功
vmuser@Linux-host ~$sudo apt-get install ia32-libs
```

## SSH 服务器
SSH 是 Secure Shell的缩写，是建立在应用层和传输层基础上的安全协议，
能够有效防止远程管理过程中的信息泄露问题。

SSH 实际上是一个 Shell，可以通过网络登录远程系统。

### 安装 SSH 服务器
```
vmuser@Linux-host:~$ sudo apt-get install openssh-server
```

### 测试 SSH 服务
在虚拟机里， VMware 虚拟网卡设置为 NAT 模式的话， Linux 系统网卡设置为动态 IP即可；
如果虚拟网卡设置为桥接模式，则需要为 Linux 设置一个与 Windows 系统同一个网段的静态 IP 地址。

主机端安装winscp连接虚拟机测试
[windows下 使用winscp 连接VM中ubuntu实现共享](http://www.jianshu.com/p/be2e085d55ed)

## NFS 服务器
通过 NFS 服务，主机将用户指定的目录通过
网络共享给目标机（和 windows的文件网络共享类似）。
目标机可以直接运行存放于 Linux主机共享目录下的程序。

### 安装 NFS 软件包
```
vmuser@Linux-host ~$ sudo apt-get install nfs-kernel-server #安装 NFS 服务器端
vmuser@Linux-host ~$ sudo apt-get install nfs-common #安装 NFS 客户端
```
### 添加 NFS 共享目录
```
sudo vi /etc/exports
#在该文件末尾添加下面的一行
    /nfsroot *(rw,sync,no_root_squash)  #“*”表示允许任何网段 IP 的系统访问该 NFS 目录。
    
#为共享目录设置最宽松的权限：
sudo chmod -R 777 /nfsroot
```

### 启动 NFS 服务
```
vmuser@Linux-host ~$ sudo /etc/init.d/nfs-kernel-server start
```
在 NFS 服务已经启动的情况下，如果修改了“/etc/exports”文件，需要重启 NFS 服务，以刷新 NFS 的共享目录。
```
vmuser@Linux-host ~$ sudo /etc/init.d/nfs-kernel-server restart
```

### 测试 NFS 服务器
可以在 Linux 主机上进行自测。 测试的基本方法为：将已经设定好的
NFS 共享目录 mount（挂载） 到另外一个目录下，看能否成功。
```
vmuser@Linux-host~$ sudo mount -t nfs 192.168.12.123:/nfsroot /mnt -o nolock
```

## TFTP 服务器
TFTP（Trivial File Transfer Protocol，简单文件传输协议），
是 TCP/IP 协议族中用来在客户机和服务器之间进行简单文件传输的协议，开销很小。

FTP 通常用于内核调试，调试内核通常是与 Bootloader 配合使用，
只需在 Bootloader 中实现了网卡驱动和TFTP 客户端，就可以使用 TFTP 进行传输内核。

### 安装配置 tftp 软件
```
vmuser@Linux-host ~$ sudo apt-get install tftpd-hpa tftp-hpa
```

### 配置 tftp 服务器
tftp 软件安装后，默认是关闭 tftp 服务的。
```
vmuser@Linux-host ~$ sudo vi /etc/default/tftpd-hpa
```

```
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/tftpboot"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="-l -c -s"
```

```
vmuser@Linux-host ~$ sudo chmod –R 777 /tftpboot
```

### 启动 tftp 服务
```
vmuser@Linux-host:~$ sudo service tftpd-hpa start
```

### 测试 tftp 服务器
在 tftp 服务器目录/tftpboot 下创建一个测试文件 tftpTestFile。

打开终端，输入以下测试命令
```
vmuser@Linux-host ~$ tftp 192.168.12.123
tftp> get tftpTestFile
tftp> q
```
tftp不需要挂载。

---

# 第七章 Linux C 编程环境

## GCC
GNU Compiler Collection， GNU 编译器套件。

### gcc 工具软件

| 名称        | 功能描述                       |
| --------- | -------------------------- |
| cpp       | C 预处理器                     |
| gcc       | C 编译器                      |
| g++       | C++编译器                     |
| gccbug    | 创建 BUG 报告的 Shell 脚本        |
| gcov      | 覆盖测试工具，用于分析在程序的哪个位置做优化效果最佳 |
| libgcc    | GCC 的运行库                   |
| libstdc++ | 标准 C++库                    |
| libsupc++ | 提供支持 C++语言的函数库             |

| 扩展名           | 文件内容                             |
| ------------- | -------------------------------- |
| .a            | 静态库，由目标文件构成的文件库                  |
| .c            | C 语言源码，必须经过预处理                   |
| .C， .cc 或.cxx | C++源代码文件，必须经过预处理                 |
| .h            | C/C++语言源代码的头文件                   |
| .i            | .c 文件经过预处理后得到的 C 语言源代码           |
| .ii           | .C， .cc 或.cxx 源码经过预处理得到的 C++源码文件 |
| .o            | 目标文件，是编译过程得到的中间文件                |
| .s            | 汇编语言文件，是.i 文件编译后得到的中间文件          |
| .so           | 共享对象库，也称动态库                      |

### gcc 基本使用
```
$ gcc [选项] [文件名]
vmuser@Linux-host:hello$ gcc hello.c -o hello
```
gcc 编译过程
- 预处理， C 编译器对各种预处理命令进行处理，包括头文件包含、宏定义的扩展、条件编译的选择等；
    ```
    $ gcc -E hello.c –o hello.i
    ```

- 编译，将预处理得到的源代码文件，进行“翻译转换”，产生出机器语言的目标程序，得到机器语言的汇编文件；
    ```
    gcc -S hello.i
    ```

- 汇编，将汇编代码翻译成了机器码，但是还不可以运行；
    ```
    $ gcc -c hello.s
    ```

- 链接，处理可重定位文件，把各种符号引用和符号定义转换成为可执行文件中的合适信息，通常是虚拟地址。
    ```
    gcc hello.o
    ```
    链接可分为动态链接和静态链接：
    - 动态链接使用动态链接库进行链接，生成的程序在执行的时候需要加载所需的动态库才能运行。
    - 静态链接使用静态库进行链接，生成的程序包含程序运行所需要的全部库，可以直接运行
        ```
        $ gcc hello.o -static -o hello_static
        ```

### gcc编译控制选项

| 名称        | 功能描述                                     |
| --------- | ---------------------------------------- |
| -c        | 只编译不链接。编译器只是将输入的.c 等源代码文件生成.o 为后缀的目标文件，通常用于编译不包含主程序的子程序文件 |
| -S        | 只对文件进行编译，不汇编和链接                          |
| -E        | 只对文件进行预处理，不编译汇编和链接                       |
| -o        | output_filename确定输出文件的名称为 output_filename，这个名称不能和源文件同名。如果不给出这个选项， gcc 就给出预设的可执行文件 a.out |
| -g        | 产生符号调试工具(GNU 的 gdb)所必要的符号信息，要想对源代码进行调试，就必须加入这个选项。 g 也分等级，默认是-g2， -g1 是最基本的， -g3 包含宏信息 |
| -DFOO=BAR | 在命令行定义预处理宏 FOO，值为 BAR                    |
| -O        | 对程序进行优化编译、链接。采用这个选项，整个源代码会在编译、链接过程中进行优化处理，这样产生的可执行文件的执行效率可以提高 |
| Idirname  | 将 dirname 目录加入到程序头文件搜索目录列表中，是在预编译过程中使用的参数 |
| -L        | dirname 将 dirname 目录加入到库文件的搜索目录列表中       |
| -l        | FOO 链接名为 libFOO 的函数库                     |
| -static   | 链接静态库                                    |
| -v        | 显示 gcc 执行时执行的详细过程， 以及 gcc 和相关程序的版本号      |

#### 头文件包含
```
$ gcc –v hello.c -I /home/vmuser/hello
```

#### 库文件链接
编译器提供了很多底层库文件，应用程序可以直接使用，例如标准输入输出库，只需要
包含 stdio.h 头文件，编译器在编译的时候会使用系统默认路径链接这个库。

库文件用法有两种
- 在编译列表中写出库文件全名（可带路径）
```
$gcc hello.c libFOO.a 或者 gcc hello.c libFOO.so
```
- 用“-L”指定库文件路径，并用“-l”参数加上 FOO 名称即可，无需
  库文件全名
```
$gcc hello.c -L /home/vmuser/hello -lFOO
```

### 创建静态库和共享库
#### 创建静态库
静态库是.o 文件的集合。

```
//创建一个 libhelloa目录，并在其中创建 hello1.c 和 hello2.c 两个文件
/* hello1.c */
#include <stdio.h>
int hello1 (void)
{
    printf("hello 1!\n");
    return 0;
}

/* hello2.c */
#include <stdio.h>
int hello2 (void)
{
    printf("hello 2!\n");
    return 0;
}
```

```
$ gcc -c hello1.c hello2.c

$ ar -r libhello.a hello1.o hello2.o
```

#### 创建共享库
共享库也是目标文件的集合，但这些文件是由编译器按照特殊方式生成的。对象模块的
每个地址（函数调用和变量引用）都是相对地址，允许在运行时被动态加载和运行。
```
$ gcc -fpic -shared hello1.c hello2.c -o libhello.so
//-fpic 参数，表示生成的对象模块是可重定位的
```

## GNU make
在终端输入 make 命令就会调用 make 工具， make 会在当前目录按照文件名顺序寻找
Makefile 文件，依次按照： GNUmakefile、 makefile、 Makefile。如果找到其中的任何一个，
就读取并按照其中的规则执行，否则报错。

### Makefile 规则

#### 目标
Makefile的基本语法格式
```
target: prerequisites
(TAB)command
```
- target 编译目标：
    - 目标文件
    - 可执行文件
    - 标签，如 all。

    在编译的时候输入“make target” 就可以执行 target 的规则。

- prerequisites 依赖关系文件

    - 生产 target 所需要的文件或者目标。

- command 生成 target 所需执行的命令

#### 伪目标
伪目标是一个标签，这个目标只执行命令，不创建目标，还能避免目标与工作目录下的实际文件名冲突。

伪目标的写法
```
PHONY:标签

如：PHONY:all
//将 all 设置为伪目标后，尽管在当前目录下有同名的 all 文件，但是在终端输入 make 命令， all 的命令会被正确执行，
```

#### 自定义变量
合理使用变量，能增强 Makefile 文件的通用性，
并简化 Makefile 文件编写。

变量的定义和赋值
```
VAR=value
//在用到变量的地方，通过美元符号（$）和括号()一起来完成变量引用，如$(VAR)。
```
变量如果有多个值，可直接在等号（=）后面列出
```
RC = hello.c hello1.c hello2.c
或
SRC = hello.c
SRC += hello1.c
SRC += hello2.c
```

```
EXE = hello
SRC = hello.c
all:
    gcc -o $(EXE) $(SRC)
.PHONY:clean
    rm -v $(EXE)
```
注释行以井号（#）开始并顶格。

#### makefile 变量
特殊变量包括
- 环境变量
    - Makefile 中基本上可以直接引用几乎所有的系统环境变
      量，比如代表当前登录用户的(USER)，系统外部命令搜索路径(PATH)等，这些变量都可以直接以$(VAR)的方式引用。
- 自动变量

    | 自动变量  | 含义                             |
    | ----- | ------------------------------ |
    | $@    | 规则的目标文件名                       |
    | $<    | 规则的目标的第一个依赖文件名                 |
    | $^    | 规则的目标所对应的所有依赖文件的列表，以空格分隔       |
    | $?    | 规则的目标所对应的依赖文件新于目标文件的文件列表，以空格分隔 |
    | $(@D) | 规则的目标文件的目录部分（如果目标在子目录中）        |
    | $(@F) | 规则的目标文件的文件名部分（如果目标在子目录中）       |

    ```
    EXE = main
    OBJ = hello.o hello1.o
    SRC = hello.c hello1.c

    EXE:$(OBJ)
        gcc -o $(EXE) $^
    .PHONY:clean
    clean:
        -rm -vfr *.o $(EXE)
    ```


- 预定义变量
    - make 的预定义变量用于定义程序名称以及传递给这些程序的参数和标志位等。

    | 预定义变量    | 含义                  |
    | -------- | ------------------- |
    | AR       | 归档维护程序，默认值为 ar      |
    | AS       | 汇编程序，默认值为 as        |
    | CC       | C 语言编译程序，默认值为 cc    |
    | CPP      | C 语言预处理程序，默认值为 cpp  |
    | RM       | 文件删除程序，默认值为 rm -f   |
    | ARFLAGS  | 传递给 AR 程序的标志，默认为 rv |
    | ASFLAGS  | 传递给 AS 程序的标志，默认值无   |
    | CFLAGS   | 传递给 CC 程序的标志，默认值无   |
    | CPPFLAGS | 传递给 CPP 程序的标志，默认值无  |
    | LDFLAGS  | 传递给链接程序的标志，默认值无     |

    ```
    EXE = main
    OBJ = hello.o hello1.o
    SRC = hello.c hello1.c

    CC = gcc
    CFLAGS = -g #可gdb调试
    LDFLAGS = -L . -lF00   #依赖库

    EXE:$(OBJ)
        $(CC) $(LDFLAGS) -o $(EXE) $^
    .PHONY:clean
    clean:
        -$(RM) $(OBJ) $(EXE)
    ```

#### 隐式规则和显式规则
EXE:$(OBJ)， EXE 依赖于 OBJ，但是整个Makefile 只定义了 EXE 的生成规则，并没有给出 OBJ 的生成规则，这是因为makefile有隐形规则。

- 显式规则 用户自定义的规则
    ```
    OBJ:$(SRC)
        $(CC) -o $(OBJ) -c $^
    ```

- 隐式规则 make既定的目标生成规则

    - 如对于一个 file.o 文件， make 会优先寻找同名的 file.c 文件，并按照 gcc -c file.c -o file.o 的编译规则生成 file.o文件。

#### 完整Makefile框架
```
# Makefile for hello
EXE = main
OBJ = hello.o hello1.o
SRC = hello.c hello1.c

CC = gcc
CFLAGS = -g
LDFLAGS = -L . –lFOO

EXE:$(OBJ)
    $(CC) $(LDFLAGS) -o $(EXE) $^
    
OBJ:$(SRC)
    $(CC) $(CFLAGS) -o $(OBJ) -c $^
    
#%.o:%.c
#   $(CC) -c $(CFLAGS) $< -o $@
    
.PHONY:clean
clean:
    -$(RM) $(OBJ) $(EXE)
```

### make 命令
```
make [选项] [宏定义] [目标]
```

| 选项   | 说明                                       |
| ---- | ---------------------------------------- |
| C    | dir 指定 make 开始运行之后的工作目录为 dir， 默认为当前目录    |
| -d   | 打印除一般处理信息之外的调试信息，例如进行比较的文件的时间，尝试的规则等等    |
| -e   | 不允许在 makefile 中对环境变量赋新值，即丢弃与环境变量同名的自定义变量 |
| -f   | file 使用指定文件为 makefile                    |
| -i   | 忽略 makefile 运行时命令产生的错误，不退出 make          |
| -I   | dir 指定 makefile 运行时的包含目录，多个包含目录用空格分隔     |
| -S   | 执行 makefile 时遇到错误即退出。这是 make 的默认工作方式，无需指定 |
| -v   | 打印 make 版本号                              |


## GDB
GDB（the GNU Project Debugger）是 GNU 发布的一个功能强大的 UNIX 程序调试工具。

### GDB 基本命令
运行 GDB调试程序
```
$ gdb <程序名称>
```
使用 GDB 调试时会经常用到的一些命令

| 命令        | 描述                                |
| --------- | --------------------------------- |
| break     | 设置断点： break + 要设置断点的行号            |
| clear     | 清除断点： clear + 要清除断点的行号            |
| delete    | 用于清除断点和自动显示的表达式的命令                |
| disable   | 让所设断点暂时失效。如果要让多个编号处的断点失效可将编号用空格隔开 |
| enable    | 与 disable 相对                      |
| run       | 运行调试程序                            |
| countinue | 继续执行正在调试的程序                       |
| file      | 装入想要调试的可执行文件                      |
| kill      | 终止正在调试的程序                         |
| list      | 列出产生执行文件的源代码的一部分                  |
| next      | 执行一行源代码但不进入函数内部                   |
| step      | 执行一行源代码而且进入函数内部                   |
| run       | 执行当前被调试的程序                        |
| quit      | 终止 gdb                            |
| watch     | 监视一个变量的值而不管它何时被改变                 |
| make      | 在 GDB 中重新产生可执行文件                  |
| shell     | 在 gdb 中执行 UNIX shell 命令           |

#### GDB 本地调试步骤

1. 编写 Makefile，在编译参数中加上-g 参数，使生成 bugging 文件。
2. 先启动 GDB 并装载 bugging 文件
    ```
    gdb bugging
    ```
3. 输入 run 命令， 执行已经装载的 bugging 文件
    ```
    (gdb) run
    ```
4. 输入 where 命令， 查看程序可能出错的地方
    ```
    (gdb) where
    ```
5. 输入 list 命令，查错误附近的代码：
    ```
    (gdb) list
    ```
6. 用 break 命令， 在第 11 行处设置断点
    ```
    (gdb) break 11
    ```
7. 输入 run 命令重新运行， 程序将会在第 11 行处停止，这时程序执行正常
    ```
    (gdb) run
    ```
8. 输入 next 命令，单步执行
    ```
    (gdb) next
    ```
9. 输入 print 命令查看 string 的值
    ```
    (gdb) print string
    ```
10. 修改程序
11. 再次编译，重新调试

### GDB 远程调试
GDB 远程调试需要两个程序，一个是目标机的GDBServer，另一个是运行于本地机器
的与之对应的 GDB，对于 ARM 嵌入式 Linux 而言，通常是 arm-linux-gdb。远程系统和本地系统之间通过网线连接。

#### 步骤
1. 修改 Makefile 的 CC 变量为 CC=arm-none-linux-gnueabi-gcc
    ```
    CC = arm-none-linux-gnueabi-gcc
    CFLAGS = -g
    ```
    输入 make 命令进行交叉编译

2. 开发板与宿主机进行 NFS 连接
    ```
    # mount -t nfs 192.168.1.168:/home/chenxibing /mnt/
    ```
3. 开发板执行 gdbserver 命令， 启动 gdbserver，设置端口号（假定为 2000），并装载 bugging程序，输入后系统创建一个调试进程，监听设定的端口
    ```
    # gdbserver :2000 bugging
    Process bugging created; pid = 1062
    Listening on port 2000
    ```
4. 主机启动 arm-linux-gdb 程序，并装载 bugging 程序
    ```
    $ arm-none-linux-gnueabi-gdb bugging
    ```
5. GDB 成功启动， 在(gdb)提示符下通过 remote 命令与开发板进行远程连接
    ```
    (gdb) target remote 192.168.1.136:2000
    //192.168.1.136 为开发板 IP 地址，2000 为目标系统开启的端口号。
    ```
6. 连接成功后，在主机(gdb)命令提示符下， 可输入各种命令进行远程调试
7. 调试完毕，在主机(gdb)提示符下输入 q，可终止 GDB 调试和连接

---
# 第八章 Linux 文件 I/O
UNIX/Linux 的一个基本哲学是“一切皆文件”。

本章讲述 Linux 下文件 I/O 的基本操作，从文件打开、关闭到文件的基本的读写操作，
最后讲了文件的 lseek 和 ioctl 操作，并提供了相应的操作范例。

## Linux 的文件 I/O 概述
文件分类
- 普通文件，即一般意义上的文件、磁盘文件；
- 设备文件，代表的是系统中一个具体的设备；
- 管道文件、 FIFO 文件，一种特殊文件，常用于进程间通信；
- 套接字（socket)文件，主要用在网络通信方面。

## 文件描述符
文件描述符 fd（file descriptor）是进程中用来表示某个文件的整数，又称它为文件句柄。

当读、写一个文件时， 先调用 open()
或 creat()函数取得代表该文件的文件描述符 fd， 然后将 fd 作为参数传递给 read()或 write()
等文件操作函数。

| 文件描述符 | 含义           | 桌面/服务器 Linux | 嵌入式 Linux |
| ----- | ------------ | ------------ | --------- |
| 0     | 标准输入（stdin）  | 键盘           | 串口终端      |
| 1     | 标准输出（stdout） | 终端屏幕         | 串口终端      |
| 2     | 标准错误（stderr） | 终端屏幕         | 串口终端      |

## 常用文件 I/O 操作和函数
文件 I/O 常用头文件
```
#include<sys/types.h> /* 定义数据类型，如 ssize_t， off_t 等 */
#include <fcntl.h> /* 定义 open， creat 等函数原型，创建文件权限的符号常量 S_IRUSR 等 */
#include <unistd.h> /* 定义 read， write， close， lseek 等函数原型 */
#include <errno.h> /* 与全局变量 errno 相关的定义 */
#include <sys/ioctl.h> /* 定义 ioctl 函数原型 */
```

### open
```
int open(const char *pathname, int flags, ... /* mode_t mode */);
```
```c
char sz_filename[] = "hello.txt"; /* 要打开的文件 */
int fd = -1;
fd = open(sz_filename, O_WRONLY | O_CREAT, /* 以只写、创建标志打开文件 */
        S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH); /* 权限模式 mode=0x664 */
if(fd < 0){ /* 出错处理 */
    ......
```
open()的参数 flags，当设置了 O_CREAT标志时，可以创建一个新文件，
也可用另外一个函数 creat()创建新文件.

### creat
```
int creat(const char *pathname, mode_t mode);
```
```
char sz_filename[] = "hello.txt";
int fd ;
fd = creat(sz_filename, 0x664);
......
```

### close
```
int close(int fd);
```

### read
```
ssize_t read(int fd, void *buf, size_t count);
//操作成功，返回实际读取的字节数,如果已到达文件结尾，返回 0，否则返回-1 表示出错.
```
```
int main(int argc, char* argv[])
{
    char sz_str[] = "Hello, welcome to linux world!";
    char sz_filename[] = "hello.txt";
    int fd = -1;
    int res = 0;
    ......
    fd = open(sz_filename, O_RDONLY); /* 以只读方式打开文件 */
    ......
    res = read(fd, buf, sizeof(buf)); /* 读文件 */
    ......
}
```

### write
```
ssize_t write(int fd, const void *buf, size_t count);
//操作成功，返回实际写入的字节数，出错则返回-1。
```
write()函数一旦返回，表明所写的数据已提交到系统内部缓存.
```
int main(int argc, char* argv[])
{
    char sz_str[] = "Hello, welcome to linux world!";
    char sz_filename[] = "hello.txt";
    int fd = -1;
    int res = 0;
    char buf[128] = {0};
    ......
    fd = open(sz_filename, O_WRONLY); /* 以只写方式打开文件 */
    ......
    res = write(fd, sz_str, sizeof(sz_str); /* 写文件 */
    ......
}
```

### fsync
进行文件数据同步，强制把已修改过的文件数据存入持久存储设备中。
```
int fsync(int fd);
//fsync()针对打开的文件，参数 fd 是已打开文件的描述符， 
//fsync()调用在将该文件已修改数据全部写入磁盘后才会返回。操作成功返回 0，否则返回-1.
```

```c
int main(int argc, char* argv[])
{
    char sz_str[] = "Hello, welcome to linux world!";
    char sz_filename[] = "hello.txt";
    nt fd = -1;
    int res = 0;
    char buf[128] = {0};
    
    fd = open(sz_filename, O_WRONLY | O_CREAT, /* 以只写、创建打开文件 */
                S_IRUSR | S_IWUSR | S_IRGRP | S_IWGRP | S_IROTH);/* 权限模式 mode=0x664 */
    if(fd < 0){
        printf("open file \"%s\" failed, errno=%d.\n",
        sz_filename, errno);
        return -1;
    }
    
    res = write(fd, sz_str, sizeof(sz_str)); /* 写文件 */
    printf("write %d bytes to \"%s\".\n", res, sz_filename);
    fsync(fd); /* 同步文件 */
    close(fd); /* 关闭文件 *
}
```

### lseek
Linux 文件，按数据的读写方式，可分为顺序读写文件和随机读写文件。

- 能随机读写的文件可通过 lseek()函数改变文件读写位置；
    - 普通磁盘文件
- 顺序读写文件只能从头到尾，按顺序进行读写。
    - 管道（pipe）文件
    - 套接字（socket）文件
    - FIFO

```
off_t lseek(int fd, off_t offset, int whence);
//如果操作成功， lseek()返回新的读写位置；否则返回-1 表示操作不成功。

new_offset = lseek(fd, offset, SEEK_CUR);
```
whence 有效值
- SEEK_SET 设置新的读写位置为从文件开头算起，偏移 offset 字节；
- SEEK_CUR 设置新的读写位置为从当前所在的位置算起，偏移 offset 字节，正值表示往文件尾部偏移，负值表示往文件头部偏移；
- EEK_END 设置新的读写位置为从文件结尾算起，偏移 offset 字节，正值表示往文件尾部偏移，负值表示往文件头部偏移

获取当前读写位置 或 测试一个文件是否支持 lseek 操作
```
new_offset = lseek(fd, 0, SEEK_CUR);
if(new_offset == -1){ /* ←应检查返回值是不是-1 */
```
```c
int fd = -1;
int res, cur, new_cur, offset = 0;
char sz_filename[] = "hello.txt";
char buf[128] = {0};

fd = open(sz_filename, O_RDONLY); /* 以只读方式打开文件 */

cur = lseek(fd, 0, SEEK_CUR); /* 获取当前读写位置 */
offset = 18 - cur; /* 要移动到偏移 18 字节的位置读数据 */
new_cur = lseek(fd, offset, SEEK_CUR); /* 从当前位置 6，移到 18 *

res = read(fd, buf, sizeof(buf)); /* 请求字节数大于文件长度，将读到文件结尾 */
buf[res]='\0';
```

### ioctl
很多设备文件通过 ioctl()函数提供设备特有的操作，比如修改设备寄存器的值。
```c
int ioctl(int fd, int cmd, …);
//参数 cmd 是文件的操作命令，这个参数的取值还决定后面的参数含义，“„”表示从参数是可选的、类型不确定的。
//操作成功， 返回 0，失败返回-1.
```
ioctl()的 cmd 操作命令是文件专有的，不同的文件， cmd 往往是不同的，没有共用性，
比如嵌入式系统中的设备文件，蜂鸣器（BUZZER）和模数转换（ADC）.

I/O操作蜂鸣器
```
//file_ioctl.c
int main(int argc, char* argv[])
{
    int fd = -1; /* 文件描述符 */
    char sz_dev[] = "/dev/imx283_beep"; /* 蜂鸣器设备文件名 */
    int cmd = 0; /* 蜂鸣器操作命令码， 0-鸣叫， 1-静音 */
    
    fd = open(sz_dev, O_RDWR); /* 以可读写方式打开，忽略最后一个参数 mode */
    if(fd == -1) /* 打开文件失败时的错误处理 */
    {
        printf("open device file \"%s\" failed, err=%d\n", sz_dev, errno);
        return errno;
    }
    
    cmd = 0; /* 发鸣叫命令 */
    ioctl(fd, cmd);
    usleep(400*1000); /* 延时 400 毫秒 */
    
    for( cmd = 1; cmd < 8; cmd++){ /* 循环发送 7 次命令 */
        ioctl(fd, 0x01 & cmd);
        usleep(200*1000); /* 延时 200 毫秒 */
    }
    
    close(fd); /* 关闭设备 */
    return 0;
}

```
先用 insmod 命令在开发板加载 beep.ko 驱动，再运行 file_ioctl程序.

---

# 第九章 进程与进程间通信
## 程序与进程
### 进程状态
- D：不可中断的深度睡眠状态，处于这种状态的进程不能响应异步信号；
- R：进程处于运行态或就绪状态
- S：可中断的睡眠状态（阻塞）
- T： 暂停状态或跟踪状态
- X： 退出状态，进程即将被销毁；
- Z： 退出状态，进程成为僵尸进程

![image](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCEcc35c0aa5ff5f6391ec0696b8aaf6500/8651)

        使用“ps -aux”命令时可观察到进程的当前状态。


### main 函数
3 种原型定义
```
int main(); /* 原型 1 */
int main(int argc, char *argv[]); /* 原型 2 */
int main(int argc, char *argv[], char *env[]); /* 原型 3 */

//参数 argc 表明命令行参数的个数；
//参数 argv 是指向参数的各个指针所构成的数组。
//argv[0]为程序的名称，后续的数组元素组成参数列表， argv[argc]值为 NULL；
// env参数是指向环境变量字符串的数组
```

### 进程 ID
每个进程在创建时，内核都会为之分配一个进程 ID（Process ID，简称 PID）用来标识
当前的进程，进程 ID 是一个类型为 pid_t 的整数，并保持同一时刻是唯一值，它最大值为
pid_max 值（默认为 32768，可修改）.

获取当前进程的进程 ID
```
#include <unistd.h>
pid_t getpid(void);
```
使用ps 命令来查看系统各个进程的 PID
```
ps -ef
```

### 父进程与子进程
创建进程为新进程的父进程，新进程是创建进程的子进程.

子进程中使用 getppid()函数获取父进程的 PID
```
#include <unistd.h>
pid_t getppid(void);
```

### 环境变量
3 种方式来获取运行环境的环境变量
- 通过 main()函数的第 3 个参数 env 获取；
    ```
    #include <stdio.h>
    int main(int argc, char * argv[], char *env[]) {
        int i = 0;
        while (env[i])
            puts(env[i++]);
        return 0;
    }
    ```

- 通过 environ 全局变量获取；
    - 在加载进程的时候，系统会为每一个进程复制一份系统环境变量的副本，并保存在全局变量 environ中。
    ```
    #include <stdio.h>
    extern char ** environ;
    int main(int argc, char * argv[]) {
        int i = 0;
        while (environ[i])
            puts(environ[i++]);
        return 0;
    }
    ```

- 通过 getenv()函数获取
    ```
    #include <stdlib.h>
    char *getenv(const char *name);

    char* env;
    env = getenv("HOME");
    ```

## 进程基本操作
### 创建进程
fork()函数从运行着的进程中分裂出一个子进程，它通过拷贝父进程的方式创建子进程。
子进程与父进程有相同的代码空间、文件描述符等资源.
```
#include <unistd.h>
pid_t fork(void);
//成功则对父进程返回子进程的 PID，对子进程返回 0；
//失败则返回小于 0 的错误码。
```

```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main(int argc, char *argv[]) {
    pid_t pid;
    pid = fork(); /* 创建进程 */
    if (pid == 0) { /* 对子进程返回 0 */
        printf("Here is child, my pid = %d, parent's pid = %d\n", getpid(), getppid()); /* 打印父子进程 PID */
        exit(0);
    } 
    else if(pid > 0) { /*对父进程返回子进程 PID */
        printf("Here is parent, my pid = %d, child's pid = %d\n", getpid(), pid);
    } 
    else { /* fork 出错 *
        perror("fork error\n");
    }
    return 0;
}
```

### 终止进程
正常终止方式
- 从 main()函数 return 返回；
- 调用类 exit()函数。
    ```
    void exit(int status);
    ```
- 常见的异常终止方式有：
- 调用 abort()函数；
- 接收到一个信号终止。

在 Shell 中，可以使用$?变量来获取上次所运行程序的退出状态。

### exec 族函数
exec 族函数用来修改当前进程的代码空间,前后进程的 ID 并无改变.
```
#include <unistd.h>
extern char **environ;
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg,..., char * const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execvpe(const char *file, char *const argv[],char *const envp[]);
//如果成功，该函数无返回，否则返回-1。

/*
后缀 l： 表示使用 list 形式来传递新程序的参数
后缀 p：表示使用 filename 做参数，如果 filename 中包含“/” 则视其为路径名，否则将会在 PATH 环境变量所指定的各个目录中搜索该文件
后缀 e：表示可以传递一个指向环境字符串指针数组的指针，环境数组需要以 NULL 结束
后缀 v： 表示使用 vector 形式来传递新程序的参数，传给新程序的所有参数放入一个字符串数组中，数组以 NULL 结束以表示结尾
*/
```

```
//sample3 程序
extern char **environ; /* 全局环境变量 */
int main(int argc, char *argv[]) {
    int i;
    printf("argc = %d\n", argc); /* 打印参数个数 */
    printf("args :");
    for (i = 0 ; i < argc; i++)
        printf(" %s ", argv[i]); /* 打印参数表 */
    printf("\n");
    
    i = 0;
    while(environ[i])
        puts(environ[i++]); /* 打印环境变量表 */
    printf("\n");
    return 0;
}
```

```
char *env_init[] = {"USER=peng", "HOME=/home/peng/", NULL}; /* 为子进程定义环境变量 */

int main(int argc, char *argv[]) {
    pid_t pid;
    if ((pid = fork())< 0) { /* 创建进程失败判断 */
        perror("fork error");
    } else if (pid == 0) { /* fork 对子进程返回 0 */
        execle("/home/peng/sample3", "sample3", "hello", "world", (char *)0, env_init);/*子进程装载新程序*/
        perror("execle error"); /* execle 失败时才执行 */
        exit(-1);
    } else {
        exit(0); /* 父进程退出 *
    }
    return -1;
}
```

### wait()函数

- 僵尸进程：如果子进程先终止，但其父进程没有为它调用 wait()函数，那么该子进
  程就会变为僵尸进程。僵尸进程在它的父进程为它调用 wait()函数之前将一直占有
  系统的内存资源。
- 孤儿进程：如果父进程先终止，尚未终止的子进程将会变成孤儿进程。孤儿进程将
  直接被 init 进程收管，由 init 进程负责收集它们的退出状态。

调用 wait()函数的进程将挂起等待直到它的任一个子进程终止
```
#include <sys/types.h>
#include <sys/wait.h>
pid_t wait(int *status);
//参数 status 是一个用来保存子进程退出状态的指针
//执行成功，将返回获取到退出状态进程的 PID，否则返回-1。
```

### 守护进程

独立于控制终端并且==周期性地执行某种任务==或等待处理某些发生的事件，它不需要用户输入就能运行并提供某种服务。守护进程最重要的特性是==后台运行==。

守护进程的父进程是 init 进程，因为它真正的父进程在 fork 出该子进程后就先于该子进
程 exit 退出了，所以它是一个由 init 领养的孤儿进程。

大多数服务器就是通过守护进程实现的，且通常以字母 d 结尾来命名进程
名， 比如 sshd、 xinetd、 crond 等。

#### 创建守护进程
```
#include <unistd.h>
int daemon(int nochdir, int noclose);
//参数 nochdir：如果传入 0，则 daemon 函数将调用进程的工作目录设置为根目录
//参数 noclose：如果传入 0，则 daemon 函数会将标准输入、标准输出、标准错误重定向到/dev/null 文件中，否则不改变这些文件描述符。
//该函数如果成功则返回 0，否则返回-1。
```
变为守护进程后程序每60 秒打印当前的时间信息到/tmp/daemon.log 文件中
```
int main(void)
{
    int fd;
    time_t curtime;
    if(daemon(0,0) == -1) {
        perror("daemon error");
        exit(-1);
    }
    fd = open("/tmp/daemon.log", O_WRONLY | O_CREAT|O_APPEND, 0644);
    if (fd < 0 ) {
        perror("open error");
        exit(-1);
    }
    while(1) {
        curtime = time(0);
        char *timestr = asctime(localtime(&curtime));
        write(fd, timestr, strlen(timestr));
        sleep(60);
    }
    close(fd);
    return 0;
}
```
守护进程执行后将脱离控制台，可用 ps -axj来查看。 



## 信号
又称为软中断信号，用来通知进程发生了异步事件。

### 常用的信号

| 信号名称            | 信号描述                                 |
| --------------- | ------------------------------------ |
| SIGALRM         | 在用 alarm()函数设置的计数器超时时，产生此信号          |
| SIGINT          | 当用户按中断键（Ctrl+c）时，终端产生此信号并发送给前台进程组的进程 |
| SIGKILL         | 这是两个不能捕捉、忽略的信号之一，它提供了一种可以杀死任一进程的方法   |
| SIGSTOP         | 这是两个不能捕捉、忽略的信号之一，用于停止一个进程。           |
| SIGPIPE         | 如果在写管道时读进程已经终止，则产生该信号。               |
| SIGTERM         | 这是由 kill 命令发送的默认信号                   |
| SIGCHLD         | 在一个进程终止或者结束时，将 SIGCHLD 信号发送给它的父进程。   |
| SIGABRT         | 调用 abort()函数时产生此信号，进程异常终止            |
| SIGSEGV         | 该信号指示进程进行了一次无效的内存引用                  |
| SIGUSR1,SIGUSR2 | 这是用户定义的两个信号，可以用于应用程序间通信              |



### 产生信号

- 通过终端按键产生信号

  - 按下 Ctrl+c，可以产生SIGINT信号，终止进程。
  - 按下Ctrl+\，可以产生SIGQUIT信号，终止进程。

- 调用系统函数向进程发信号

  - `kill`函数可以给一个指定的进程发送指定的信号。

  - `raise`函数可以给当前进程发送指定的信号

  - ```c
    #include <signal.h>
    
    int kill(pid_t pid, int signo);
    int raise(int signo);
    ```

  - `abort`函数使当前进程接收到`SIGABRT`信号而异常终止，相当于exit(1)。

    - ```c
      #include <stdlib.h>
      
      void abort(void);
      ```

- 由软件条件产生信号

  - 如果在写管道时读进程已经终止，则产生SIGPIPE信号。

  - 使用alarm()函数设置的计数器超时时，产生SIGALRM信号，终止当前进程。

    - ```c
      #include <unistd.h>
      
      unsigned int alarm(unsigned int seconds);
      ```



### 重写信号处理函数

- sigaction 函数
```
#include <signal.h>
int sigaction(int signum, const struct sigaction *act,struct sigaction *oldact);

//参数 signum 指出需要改变处理方法的信号，如 SIGINT 信号
//参数 act 和 oldact 是一个 sigaction 结构体的指针，
//act 是要设置的对信号的新处理方式，而 oldact 则为原来对信号的处理方式。 

struct sigaction {
    void (*sa_handler)(int);  //信号发生时调用的处理函数
    void (*sa_sigaction)(int, siginfo_t *, void *); //另外一个信号处理函数
    sigset_t sa_mask;   //指定在信号处理函数执行期间需要被屏蔽的信号
    int sa_flags; // sa_flags 成员的值包含了 SA_SIGINFO 标志时，系统将使用sa_sigaction 函数作为信号的处理函数，否则将使用 sa_handler
    void (*sa_restorer)(void); //已经废弃
};
```

```
//实现当进程首次收到 SIGINT(Ctrl+c)信号时在终端打印 Ouch 信息，后续又恢复到默认的处理方法。

void ouch(int sig) { /* 信号处理函数 */
    printf("\nOuch! - I got signal %d\n", sig);
}

int main(int argc, char *argv[]) {
    struct sigaction act;
    act.sa_handler = ouch; /* 设置信号处理函数 */
    sigemptyset(&act.sa_mask); /* 清空屏蔽信号集 */
    act.sa_flags = SA_RESETHAND; /* 设置信号处理之后恢复默认的处理方式 */
    sigaction(SIGINT, &act, NULL); /* 设置 SIGINT 信号的处理方法 */
    while (1) { /* 进入循环等待信号发生 */
        printf("sleeping\n");
        sleep(1);
    }
    return 0;
}
```

- kill 函数

向指定的进程发送一个指定的信号
```
#include <sys/types.h>
#include <signal.h>
int kill(pid_t pid, int sig);

kill(pid, SIGINT);
```

## 进程间通信

### 管道
管道是一个进程连接数据流到另一个进程的通道,用符号“|”来表示管道

```
$ ps -ef | grep init
// ps 是一个独立的进程， grep 也是一个独立的进程，
//中间的管道把 ps 原本要输出到屏幕的数据输出到 grep 中，作为 grep 这个进程的输入
```
分类
- 匿名管道
    - 主要用于两个有父子关系的进程间通信
    - 创建匿名管道
        ```
        #include <unistd.h>
        int pipe(int pipefd[2]);
        //函数成功返回 0，否则返回-1
        //参数 pipefd 是一个文件描述符数组，对应着所打开管道的两端.
        //pipefd[0]为读端，pipefd[1]为写端，往写端写的数据会被内核缓存起来，直到读端将数据读完。
        ```
        匿名管道创建以后，不需要打开，可直接读写
        ```
        int main(int argc, char *argv[])
        {
            int pipefd[2];
            pid_t cpid;
            char buf;
            if (argc != 2) {
                fprintf(stderr, "Usage: %s <string>\n", argv[0]);
                exit(EXIT_FAILURE);
            }
            if (pipe(pipefd) == -1) { /* 创建匿名管道 */
                perror("pipe");
                exit(EXIT_FAILURE);
            }
            cpid = fork(); /* 创建子进程 */
            if (cpid == -1) {
                perror("fork");
                exit(EXIT_FAILURE);
            }
            if (cpid == 0) { /* 子进程读管道读端 */
                close(pipefd[1]); /* 关闭不需要的写端 */
                while (read(pipefd[0], &buf, 1) > 0)
                    write(STDOUT_FILENO, &buf, 1);
                write(STDOUT_FILENO, "\n", 1);
                close(pipefd[0]);
                _exit(EXIT_SUCCESS);
            } else { /* 父进程写 argv[1]到管道 */
                close(pipefd[0]); /* 关闭不需要的读端 */
                write(pipefd[1], argv[1], strlen(argv[1]));
                close(pipefd[1]); /* 关闭文件发送 EOF，子进程停止读*/
                wait(NULL); /* 等待子进程退出 */
                exit(EXIT_SUCCESS);
            }
        }
        ```
- 命名管道
    - 主要用于没有父子关系的进程间通信.
    - 创建一个命名管道
        ```
        #include <sys/types.h>
        #include <sys/stat.h>
        int mkfifo(const char *pathname, mode_t mode);
        //参数 pathname指定了文件名，参数 mode 则指定了文件的读写权限。
        //函数成功返回 0，否则返回-1
        ```
        命名管道创建以后，需要先打开文件，然后再进行读写。

        ```
        //通过命名管道发送文件的功能。程序先判断命名管道是否已存在，如果不存在则创建命名管道/tmp/fifo文件，
        //然后将参数所指出文件的数据循环读出并写进命名管道中.
        int main(int argc, char *argv[])
        {
            const char *fifoname = "/tmp/fifo"; /* 命名管道文件名 */
            int pipefd, datafd;
            int bytes, ret;
            char buffer[BUFSIZE];
            if (argc != 2) { /* 带文件名参数 */
                fprintf(stderr, "Usage: %s < filename >\n", argv[0]);
                exit(EXIT_FAILURE);
            }
            if(access(fifoname, F_OK) < 0) { /* 判断文件是否已存在 */
                ret = mkfifo(fifoname, 0777); /* 创建管道文件 */
                if(ret < 0) {
                    perror("mkfifo error");
                    exit(EXIT_FAILURE);
                }
            }
            pipefd = open(fifoname, O_WRONLY); /* 打开管道文件 */
            datafd = open(argv[1], O_RDONLY); /* 打开数据文件 */
            if((pipefd > 0) && (datafd > 0)) { /* 将数据文件读出并写到管道文件 */
                bytes = read(datafd, buffer, BUFSIZE);
                while(bytes > 0) {
                    ret = write(pipefd, buffer, bytes);
                    if(ret < 0) {
                        perror("write error");
                        exit(EXIT_FAILURE);
                    }
                    bytes = read(datafd, buffer, BUFSIZE);
                }
                close(pipefd);
                close(datafd);
            } else {
                exit(EXIT_FAILURE);
            }
            return 0;
        }
        ```

```
//程序打开命名管道文件/tmp/fifo，然后循环从命名管道中读取数据
//并将它们写入由参数给出的文件中

int main(int argc, char *argv[])
{
    const char *fifoname = "/tmp/fifo"; /* 命名管道文件名，需对应写进程 */
    int pipefd, datafd;
    int bytes, ret;
    char buffer[BUFSIZE];
    if (argc != 2) { /* 带文件名参数 */
        fprintf(stderr, "Usage: %s < filename >\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    pipefd = open(fifoname, O_RDONLY); /* 打开管道文件 */
    datafd = open(argv[1], O_WRONLY|O_CREAT, 0644); /* 打开目标文件 */
    if((pipefd > 0) && (datafd > 0)) { /* 将管道文件的数据读出并写入目标文件 */
        bytes = read(pipefd, buffer, BUFSIZE);
        while(bytes > 0) {
            ret = write(datafd, buffer, bytes);
            if(ret < 0) {
                perror("write error");
                exit(EXIT_FAILURE);
            }
            bytes = read(pipefd, buffer, BUFSIZE);
        }
        close(pipefd);
        close(datafd);
    } else {
        exit(EXIT_FAILURE);
    }
    return 0;
}
```

### 共享内存
共享内存允许两个不相关的进程访问同一个逻辑内存,
是在两个正在运行的进程之间共享和传递数据的一种非常有效的方式.

不同进程之间的共享内存通常安排为同一段物理内存。进程可以将同一段共享内存连接
到它们自己的地址空间中，所有进程都可以访问共享内存中的地址.

#### 打开或创建一个共享内存区
```
#include <sys/mman.h>
#include <sys/stat.h>
#include <fcntl.h>
int shm_open(const char *name, int oflag, mode_t mode);

//name 为指定创建的共享内存的名称
//oflag 为以下标志
    O_RDONLY：共享内存以只读方式打开；
    O_RDWR：共享内存以可读写方式打开；
    O_CREAT：共享内存不存在才创建；
    O_EXCL：如果指定了 O_CREAT，但共享内存已经存在时返回错误；
    O_TRUNC：如果共享内存已经存在则将其大小设置为 0；
//只有指定了 O_CREAT 后才有效， 用于设定共享内存的权限
//函数成功返回创建或打开的共享内存描述符,失败则返回-1。
```
注意：新创建或打开的共享内存大小默认为 0，需要设置大小才能使用。

#### 删除共享内存
```
int shm_unlink(const char *name);
//函数成功返回 0，失败返回-1。
```

#### 设置共享内存大小
```
int ftruncate(int fd, off_t length);
//函数成功返回 0，失败返回-1。
```

#### 映射共享内存
创建共享内存后，需要将这块内存区域映射到调用进程的地址空间中
```
void *mmap(void *addr, size_t length, int prot, int flags,int fd, off_t offset);
//函数成功返回映射后指向共享内存的虚拟地址，失败返回 MAP_FAILED 值。
```
- addr：指向映射存储区的起始地址， 通常将其设置为 NULL， 表示让系统来选择该映射区的起始地址；
- length：映射的字节数
- prot：对映射存储区的保护要求
    - PROT_READ： 映射区可读；
    - PROT_WRITE：映射区可写；
    - ROT_EXEC：映射区可执行；
    - PROT_NONE： 映射区不可访问
- flag：映射标志位
    - MAP_FIXED： 返回值必须等于 addr
    - MAP_SHARED： 多个进程对同一个文件的映射是共享的
    - MAP_PRIVATE： 多个进程对同一个文件的映射不是共享的
- fd：要被映射的文件描述符或者共享内存描述符
- offset： 要映射字节在文件中的起始偏移量

#### 取消共享内存映射
```
int munmap(void *addr, size_t length);
//addr 为 mmap()函数返回的地址， length 是映射的字节数。
//函数成功返回 0，否则返回-1；
```

#### 共享内存示例
共享内存写数据
```c
#define SHMSIZE 10 /* 共享内存大小， 10 字节 */
#define SHMNAME "shmtest" /* 共享内存名称 */
int main()
{
    int fd;
    char *ptr;
    /* 创建共享内存 */
    fd = shm_open(SHMNAME, O_CREAT | O_TRUNC | O_RDWR, S_IRUSR | S_IWUSR);
    if (fd<0) {
        perror("shm_open error");
        exit(-1);
    }
    /* 设置共享内存大小*/
    ftruncate(fd, SHMSIZE); /* 设置大小为 SHMSIZE */
    /*将共享内存映射到进程地址空间*/
    ptr = mmap(NULL, SHMSIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (ptr == MAP_FAILED) {
        perror("mmap error");
        exit(-1);
    }
    *ptr = 18; /* 往起始地址写入 18 */
    munmap(ptr, SHMSIZE); /* 取消映射 */
    shm_unlink(SHMNAME); /* 删除共享内存 */
    return 0;
}
```
共享内存读数据
```c
#define SHMSIZE 10 /* 共享内存大小， 10 字节 */
#define SHMNAME "shmtest" /* 共享内存名称 */
int main()
{
    int fd;
    char *ptr;
    fd = shm_open(SHMNAME, O_CREAT | O_RDWR, S_IRUSR | S_IWUSR); /*创建共享内存 */
    if (fd < 0) {
        perror("shm_open error");
        exit(-1);
    }
    ptr = mmap(NULL, SHMSIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);/*映射共享内存*/
    if (ptr == MAP_FAILED) {
        perror("mmap error");
        exit(-1);
    }
    ftruncate(fd, SHMSIZE); /* 设置共享内存大小 */
    while (*ptr != 18) { /* 读起始地址，判断值是否为 18 */
        sleep(1); /* 不是 18，继续读取 */
    }
    printf("ptr : %d\n", *ptr); /* 数据是 18，打印显示 */
    munmap(ptr, SHMSIZE); /* 取消内存映射 */
    shm_unlink(SHMNAME); /* 删除共享内存 */
    return 0;
}
```

注意：使用共享内存时，编译需要加上-lrt

```c
gcc shmtest.c -o shmtest -lrt
```



### 信号量

信号量是用来解决进程间同步与互斥问题的一种进程间通信机制

- 二值信号量
- 计数信号量

信号量只能进行两个原子操作
- P 操作：Take
- V 操作：Give

POSIX 提供两类信号量
- 有名信号量
    - 可以让不同的进程通过信号量的名字获取到信号量
- 基于内存的信号量（无名信号量）
    - 只能放置在进程间共享内存区域中

#### 创建或打开有名信号量
```
#include <fcntl.h>
#include <sys/stat.h>
#include <semaphore.h>
sem_t *sem_open(const char *name, int oflag);
sem_t *sem_open(const char *name, int oflag,mode_t mode, unsigned int value);
/*
name 为信号量的名字
oflag
    O_CREAT,如果name指定的信号量不存在则创建，此时必须给出 mode和value
    O_EXCL
mode 为信号量的权限位
value 为信号量的初始化值
*/
```

#### 关闭有名信号量
```
int sem_close(sem_t *sem);
//函数成功返回 0，失败返回-1
```

#### 删除有名信号量
```
int sem_unlink(const char *name);
//该函数成功返回 0，失败返回-1。
```

#### 初始化基于内存信号量
```c
int sem_init(sem_t *sem, int pshared, unsigned int value);
//函数成功返回 0，失败返回-1；

/*
参数 sem 为需要初始化的信号量的指针； 
pshared 值如果为 0 表示该信号量只能在线程内部使用，否则为进程间使用。
        //在进程间使用时，该信号量需要放在共享内存处； 
value 为信号量的初始化值，代表的资源数。
*/
```

#### 销毁基于内存的信号量
```
int sem_destroy(sem_t *sem);
//函数成功返回 0，否则返回-1。
```

#### P操作 Take
```
int sem_wait(sem_t *sem);
//如果信号量的值大于 0， sem_wait()函数将信号量值减 1 并立即返回
//如果没有可用的资源（信号量值=0），则进程被阻塞直到系统将资源分配给该进程
```

#### V操作 Give
```
int sem_post(sem_t *sem);
//把所指定的信号量的值加 1，然后唤醒正在等待该信号量的任意进程（线程）
```

#### 信号量范例

服务器端和客户端通过共享内存通信，服务端接收客户端传
递的数据， 用信号量实现两进程同步。

```c
//使用有名信号量同步共享内存示例服务端程序

#define MAPSIZE 100 /* 共享内存大小， 100 字节 */
int main(int argc, char **argv)
{
    int shmid;
    char *ptr;
    sem_t *semid;
    if (argc != 2) { /* 参数 argv[1]指定共享内存和信号量的名字 */
        printf("usage: %s <pathname>\n", argv[0]);
        return -1;
    }
    shmid = shm_open(argv[1], O_RDWR|O_CREAT, 0644); /* 创建共享内存对象 */
    if (shmid == -1) {
        printf( "open shared memory error\n");
        return -1;
    }
    ftruncate(shmid, MAPSIZE); /* 设置共享内存大小 */
    ptr = mmap(NULL, MAPSIZE, PROT_READ | PROT_WRITE, MAP_SHARED, shmid, 0);
    strcpy(ptr,"\0");
    semid = sem_open(argv[1], O_CREAT, 0644, 0); /* 创建信号量对象 */
    if (semid == SEM_FAILED) {
        printf("open semaphore error\n");
        return -1;
    }
    sem_wait(semid); /* 信号量等待操作，等待客户端修改共享内存 */
    printf("server recv:%s",ptr); /* 从共享内存中读取值 */
    strcpy(ptr,"\0");
    munmap(ptr, MAPSIZE); /* 取消对共享内存的映射 */
    close(shmid); /* 关闭共享内存 */
    sem_close(semid); /* 关闭信号量 */
    sem_unlink(argv[1]); /* 删除信号量对象 */
    shm_unlink(argv[1]); /* 删除共享内存对象 */
    return 0;
}
```

```c
//使用有名信号量同步共享内存示例客户端程序

#define MAPSIZE 100 /* 共享内存大小， 100 字节 */
int main(int argc, char **argv){    
  int shmid;   
  char *ptr;    
  sem_t *semid;    
  if (argc != 2) {        
    printf("usage: %s <pathname>\n", argv[0]); /* 参数 argv[1]指定共享内存和信号量的名字 */        	return -1;    
  }    
  shmid = shm_open(argv[1], O_RDWR, 0); /* 打开共享内存对象 */    
  if (shmid == -1) {        
    printf( "open shared memory error.\n");        
    return -1;    
  }    
  ftruncate(shmid, MAPSIZE); /* 设置共享内存大小 */    
  /* 将共享内存进行映射 */    
  ptr = mmap(NULL, MAPSIZE, PROT_READ | PROT_WRITE, MAP_SHARED, shmid, 0);    
  semid = sem_open(argv[1], 0); /* 打开信号量对象 */    
  if (semid == SEM_FAILED) {        
    printf("open semaphore error\n");        
    return -1;    
  }    
  printf("client input:");    
  fgets(ptr, MAPSIZE, stdin); /* 从标准输入读取需要写入共享内存的值 */    
  sem_post(semid); /* 通知服务器端 */    
  munmap(ptr, MAPSIZE); /* 取消对共享内存的映射 */    
  close(shmid);    
  sem_close(semid);    
  return 0;
}

```

# 第十章 Linux 多线程编程
线程（thread）是包含在进程内部的顺序执行流，是进程中的实际运作单位。


- 方便的通信和数据交换
    - 同一进程下的线程之间共享数据空间，所以一个线程的数据可以直接为其它线程所用
- 更高效的利用 CPU
    - 使用多线程可以加快应用程序的响应。

## POSIX Threads
POSIX Threads（通常简称为 Pthreads）定义了创建和操纵线程的一套 API 接口。

Pthreads 接口可以根据功能划分四个组：
- 线程管理
- 互斥量
- 条件变量
- 同步

编写 Pthreads 多线程的程序时， 源码只需要包含 pthread.h 头文件就可以使用 Pthreads
库中的所有类型及函数
```
#include <pthread.h>

```
在编译 Pthread 程序时在编译和链接过程中需要加上-pthread 参数：
```
LDFLAGS += -pthread
```

### 线程管理
线程管理包含了线程的创建、终止、等待、分离、设置属性等操作。

#### 创建线程
```
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
            void *(*start_routine) (void *), void *arg);
//如果 pthread_create()调用成功，函数返回 0，否则返回一个非 0 的错误码

/*
thread 用来指向新创建线程的 ID；
attr 用来表示一个封装了线程各种属性的属性对象
start_routine 是线程开始执行的时候调用的函数的名字, (void *), void *arg为其参数
*/
```
说明：线程被创建后将立即运行。

#### 终止线程
```
void pthread_exit(void *retval);
//retval 返回值

```

```
void *PrintHello(void *threadid){ /* 线程函数 */
    long tid;
    tid = (long)threadid;
    printf("Hello World! It's me, thread #%ld!\n", tid); /* 打印线程对应的参数 */
    pthread_exit(NULL);
}
int main (int argc, char *argv[])
{
    pthread_t threads[NUM_THREADS];
    int rc;
    long t;
    for(t=0; t<NUM_THREADS; t++){ /* 循环创建 5 个线程 */
    printf("In main: creating thread %ld\n", t);
        rc = pthread_create(&threads[t], NULL, PrintHello, (void *)t); /* 创建线程 */
        if (rc){
            printf("ERROR; return code from pthread_create() is %d\n", rc);
            exit(-1);
        }
    }
    printf("In main: exit!\n");
    pthread_exit(NULL); /* 主线程退出 */
    return 0;
}
```
操作系统线程调度随机

####  线程分离
将非分离线程设置为分离线程，分离线程是退出时会释放它的资源的线程；
```
int pthread_detach(pthread_t thread);
//成功返回 0；失败返回一个非 0 的错误码
pthread_detach(pthread_self());
```

#### 线程连接
非分离线程退出后不会立即释放资源，需要另一个线程为它调用 pthread_join 函数
或者进程退出时才会释放资源.

pthread_join()函数将调用线程挂起， 直到参数 thread 指定的目标线程终止运行为止
```
int pthread_join(pthread_t thread, void **retval);
```

```
#define NUM_THREADS 4

void *BusyWork(void *t) /* 线程函数 */
{
    int i;
    long tid;
    double result=0.0;
    tid = (long)t;
    printf("Thread %ld starting...\n",tid);
    for (i=0; i<1000000; i++) {
        result = result + sin(i) * tan(i); /* 进行数学运算 */
    }
    printf("Thread %ld done. Result = %e\n",tid, result);
    pthread_exit((void*) t); /* 带计算结果退出 */
}

int main (int argc, char *argv[])
{
    pthread_t thread[NUM_THREADS];
    int rc;
    long t;
    void *status;
    for(t=0; t<NUM_THREADS; t++) {
        printf("Main: creating thread %ld\n", t);
        rc = pthread_create(&thread[t], NULL, BusyWork, (void *)t); /* 创建线程 */
        if (rc) {
            printf("ERROR; return code from pthread_create() is %d\n", rc);
            exit(-1);
        }
    }
    for(t=0; t<NUM_THREADS; t++) {
        rc = pthread_join(thread[t], &status); /*等待线程终止，并获取返回值*/
        if (rc) {
            printf("ERROR; return code from pthread_join() is %d\n", rc);
            exit(-1);
        }
        printf("Main: completed join with thread %ld having a status of %ld\n",t,(long)status);
    }
    printf("Main: program completed. Exiting.\n");
    pthread_exit(NULL);
}
```

注意：使用math库的时候，需要在编译时加上-lm



#### 线程属性

线程基本属性包括： 栈大小、 调度策略和线程状态。

- 属性对象

    初始化属性对象
    ```
    int pthread_attr_init(pthread_attr_t *attr);
    //函数只有一个参数， 是一个指向 pthread_attr_t 的属性对象的指针。
    //成功返回 0， 否则返回一个非 0 的错误码。
    ```

    销毁属性对象
    ```
    int pthread_attr_destroy(pthread_attr_t *attr);
    //成功返回 0， 否则返回一个非 0 的错误码
    ```

- 线程状态

    获取线程状态
    ```
    int pthread_attr_getdetachstate(pthread_attr_t *attr, int *detachstate);
    /*参数 attr 是一个指向已初始化的属性对象的指针， detachstate 是所获取状态值的指针。
    成功返回 0，否则返回一个非 0 的错误码。*/
    ```

    设置线程状态
    ```
    int pthread_attr_setdetachstate(pthread_attr_t *attr, int detachstate)
    /*成功返回 0，否则返回一个非 0 的错误码。*/
    ```

- 线程栈

每个线程都有一个独立的调用栈，线程的栈大小在线程创建的时候就已经固定下来，
Linux 系统线程的默认栈大小为 8MB.

获取线程栈
```
int pthread_attr_getstacksize(pthread_attr_t *attr, size_t *stacksize);
/*参数 attr 是一个指向已初始化的属性对象的指针，stacksize 是保存所获取栈大小的指针。
成功返回 0，否则返回一个非 0 的错误码。*/
```

设置线程栈
```
int pthread_attr_setstacksize(pthread_attr_t *attr, size_t stacksize);
/*成功返回 0，否则返回一个非 0 的错误码。*/
```

- 示例
```c
/*主线程根据参数给出的线程栈大小来设置线程属性对象，
然后使用剩余参数分别创建线程来实现小写转大写的功能并打印出栈地址*/

struct thread_info {
pthread_t thread_id;
int thread_num;
char *argv_string;
};

static void *thread_start(void *arg){ /* 线程运行函数 */
    struct thread_info *tinfo = arg;
    char *uargv, *p;
    printf("Thread %d: top of stack near %p; argv_string=%s\n", /* 通过 p 的地址来计算栈的起始地址*/
    tinfo->thread_num, &p, tinfo->argv_string);
    uargv = strdup(tinfo->argv_string);
    if (uargv == NULL)
        handle_error("strdup");
    for (p = uargv; *p != '\0'; p++)
        *p = toupper(*p); /* 小写字符转换大写字符 */
    return uargv; /* 将转换结果返回 */
}

int main(int argc, char *argv[]){
    int s, tnum, opt, num_threads;
    struct thread_info *tinfo;
    pthread_attr_t attr;
    int stack_size;
    void *res;
    stack_size = -1;
    while ((opt = getopt(argc, argv, "s:")) != -1) { /* 处理参数-s 所指定的栈大小 */
        switch (opt) {
            case 's':
                stack_size = strtoul(optarg, NULL, 0);
                break;
            default:
                fprintf(stderr, "Usage: %s [-s stack-size] arg...\n",argv[0]);
                exit(EXIT_FAILURE);
        }
    }
    num_threads = argc - optind;//调用getopt的时，从optind存储的位置处重新开始检查选项。 
    s = pthread_attr_init(&attr); /* 初始化属性对象 */
    if (s != 0)
        handle_error_en(s, "pthread_attr_init");
        if (stack_size > 0) {
            s = pthread_attr_setstacksize(&attr, stack_size); /* 设置属性对象的栈大小 */
        if (s != 0)
            handle_error_en(s, "pthread_attr_setstacksize");
    }
    tinfo = calloc(num_threads, sizeof(struct thread_info));
    if (tinfo == NULL)
        handle_error("calloc");
    for (tnum = 0; tnum < num_threads; tnum++) {
        tinfo[tnum].thread_num = tnum + 1;
        tinfo[tnum].argv_string = argv[optind + tnum];
        s = pthread_create(&tinfo[tnum].thread_id, &attr, /* 根据属性创建线程 */
        &thread_start, &tinfo[tnum]);
        if (s != 0)
            handle_error_en(s, "pthread_create");
    }
    s = pthread_attr_destroy(&attr); /* 销毁属性对象 */
    if (s != 0)
        handle_error_en(s, "pthread_attr_destroy");
    for (tnum = 0; tnum < num_threads; tnum++) {
        s = pthread_join(tinfo[tnum].thread_id, &res); /* 等待线程终止，并获取返回值 */
        if (s != 0)
            handle_error_en(s, "pthread_join");
        printf("Joined with thread %d; returned value was %s\n",
        tinfo[tnum].thread_num, (char *) res);pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
        free(res);
    }
    free(tinfo);
    exit(EXIT_SUCCESS);
}

```

## 互斥量

### 临界区
在计算机系统中有许多共享资源不允许用户并行使用。

临界区是必须以互斥方式执行的代码段，也就是说在临界区的范围内只能有一个活动的执行线程。

### 互斥量含义
互斥量（Mutex）， 又称为互斥锁， 是一种用来保护临界区的特殊变量， 它可以处于锁
定（locked） 状态， 也可以处于解锁（unlocked） 状态。

当互斥锁处于解
锁状态时， 如果某个线程试图获取这个互斥锁， 那么这个线程就可以得到这个互斥锁而不会
阻塞；当互斥锁处于锁定状态时， 如果某个线程试图获取这个互斥锁， 那么这个线程将阻塞
在互斥锁的等待队列内。

### 创建互斥量
- 静态初始化
```
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

- 动态初始化
```
int pthread_mutex_init(pthread_mutex_t *restrict mutex,const pthread_mutexattr_t *restrict attr);
//参数 mutex 是一个指向要初始化的互斥量的指针；
//参数 attr 传递 NULL 来初始化一个带有默认属性的互斥量
//函数成功返回 0，否则返回一个非 0 的错误码
```

静态初始化程序通常比调用 pthread_mutex_init 更有效，而且在任何线程开始执行之
前，确保变量被初始化一次。

### 销毁互斥量
```
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```

### 加锁 Take

```
int pthread_mutex_lock(pthread_mutex_t *mutex);//函数会一直阻塞到互斥量可用为止
int pthread_mutex_trylock(pthread_mutex_t *mutex);//尝试加锁， 但通常会立即返回
//函数成功返回 0，否则返回一个非 0 的错误码
```

### 解锁 Give
```
int pthread_mutex_unlock(pthread_mutex_t *mutex);
//函数成功返回 0，否则返回一个非 0 的错误码。
```

### 死锁和避免
死锁是指两个或两个以上的执行序列在执行过程中， 因争夺资源而造成的一种互相等待
的现象。

- 死锁的避免
    - 如果能确保所有的线程都是按照相同的顺序获得锁，那么死锁就不会发生。 
      如规定程序内有三个互斥锁的加锁顺序为 mutexA->mutexB->mutexC

    | t1           | t2           | t3           |
    | ------------ | ------------ | ------------ |
    | lock(mutexA) | lock(mutexA) | lock(mutexB) |
    | lock(mutexB) | lock(mutexC) | lock(mutexC) |
    | lock(mutexC) |              |              |


### 示例：使用互斥量保护多线程同时输出
使用互斥量来保证多线程同时输出顺序的例子，互斥量能保证只有获
取资源的线程打印完， 别的线程才能打印， 从而避免了打印乱序的问题。
```
pthread_t tid[2];
pthread_mutex_t lock;

void* doPrint(void *arg)
{
    int id = (long)arg;
    int i = 0;
    pthread_mutex_lock(&lock); /* 使用互斥量保护临界区 */
    printf("Job %d started\n", id);
    for (i = 0; i < 5; i++) {
        printf("Job %d printing\n", id);
        usleep(10);
    }
    printf("Job %d finished\n", id);
    pthread_mutex_unlock(&lock);
    return NULL;
}
    
int main(void)
{
    long i = 0;
    int err;
    
    if (pthread_mutex_init(&lock, NULL) != 0) /* 动态初始化互斥量 */
    {
        printf("\n Mutex init failed\n");
        return 1;
    }
    while(i < 2)
    {
        err = pthread_create(&(tid[i]), NULL, &doPrint, (void*)i);
        if (err != 0)
        printf("Can't create thread :[%s]", strerror(err));
        i++;
    }
    pthread_join(tid[0], NULL);
    pthread_join(tid[1], NULL);
    pthread_mutex_destroy(&lock);
    
    return 0;
}
```

## 条件变量
假设有两个线程 t1 和 t2， 需要这个两个线程循环对一个共享变量 sum 进行自增操作，
那么 t1 和 t2 只需要使用互斥量即可保证操作正确完成。如果这时需要增加另一个线程 t3，需要 t3 在 count 大于 100 时将 count 值重新置 0 值。就会存在时间与效率的矛盾。条件变量就是解决这个问题的好方法。

### 创建条件变量
- 静态初始化
```
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
```
- 动态初始化
```
int pthread_cond_init(pthread_cond_t *restrict cond,const pthread_condattr_t *restrict attr);
/*参数 cond 是一个指向需要初始化 pthread_cond_t 变量的指针，
参数 attr 传递 NULL 值时， pthread_cond_init()将 cond 初始化为默认属性的条件变量。
函数成功将返回 0；否则返回一个非 0 的错误码。*/
```

- 销毁条件变量
```
int pthread_cond_destroy(pthread_cond_t *cond);
```

### 等待
条件变量是与条件测试一起使用的，通常线程会对一个条件进行测试，如果条件不满足
就会调用条件等待函数来等待条件满足。

作用：释放信号量，并使线程一直或一段时间内阻塞。

```
//在条件不满足时将一直等待
int pthread_cond_wait(pthread_cond_t *restrict cond, 
            pthread_mutex_t *restrict mutex);
//只等待一段时间
int pthread_cond_timedwait(pthread_cond_t *restrict cond,
            pthread_mutex_t *restrict mutex, 
            const struct timespec *restrict abstime);
/*            
参数 cond 是一个指向条件变量的指针，
参数 mutex 是一个指向互斥量的指针，线程在调用前应该拥有这个互斥量，
当线程要加入条件变量的等待队列时，等待操作会使线程释放这个互斥量。 
*/

//函数成功调用返回 0，否则返回非 0 的错误码
```

### 通知
唤醒阻塞的线程。
```
//唤醒所有在条件变量等待队列等待的线程
int pthread_cond_broadcast(pthread_cond_t *cond);
//唤醒一个在条件变量等待队列等待的线程
int pthread_cond_signal(pthread_cond_t *cond);
//参数 cond 是一个指向条件变量的指针。函数成功返回 0，否则返回一个非 0 的错误码。
```

### 条件变量示例
```
pthread_t tid[3];
int sum = 0;
pthread_mutex_t sumlock = PTHREAD_MUTEX_INITIALIZER; /* 静态初始化互斥量 */
pthread_cond_t cond_sum_ready = PTHREAD_COND_INITIALIZER; /* 静态初始化条件变量 */

void * t1t2(void *arg) {
    int i;
    long id = (long)arg;
    for (i = 0; i < 60; i++) {
        pthread_mutex_lock(&sumlock); /* 使用互斥量保护临界变量 */
        sum++;
        printf("t%ld: read sum value = %d\n", id + 1 , sum);
        pthread_mutex_unlock(&sumlock);
        if (sum >= 100)
            pthread_cond_signal(&cond_sum_ready); /* 发送条件通知，唤醒等待线程 */
    }
    return NULL;
}

void * t3(void *arg) {
    pthread_mutex_lock(&sumlock);
    while(sum < 100) /* 不满足条件将一直等待 */
        pthread_cond_wait(&cond_sum_ready, &sumlock); /* 等待条件满足 */
    sum = 0;
    printf("t3: clear sum value\n");
    pthread_mutex_unlock(&sumlock);
    return NULL;
}

int main(void) 
{
    int err;
    long i;
    for (i = 0; i < 2; i++) {
        err = pthread_create(&(tid[i]), NULL, &t1t2, (void *)i); /* 创建线程 1 线程 2 */
        if (err != 0) {
            printf("Can't create thread :[%s]", strerror(err));
        } 
    }
    err = pthread_create(&(tid[2]), NULL, &t3, NULL); /* 创建线程 3 */
    if (err != 0)
        printf("Can't create thread :[%s]", strerror(err));
    for (i = 0; i < 3; i++)
        pthread_join(tid[i], NULL);
    return 0;

```

# 第十一章 C 语言网络编程

## 网络基本概念

### OSI 模型
OSI 模型(Open System Interconnection model，开放系统互联模型)是一个由国际标准
化组织提出概念模型，试图提供一个使各种不同的计算机和网络在世界范围内实现互联的标
准框架。



7层模型

- 物理层

- 数据链路层 
- 网络层 
- 传输层 
- 会话层 
- 表示层 
- 应用层 

### TCP/IP 协议 

- 网络接口层 
- 网间层 
  - IP 协议、 RIP 协议（Routing
    Information Protocol，路由信息协议），负责数据的包装、寻址和路由。 
- 传输层 
  - TCP 、UDP 协议 
- 应用层 
  - Finger、Whois、 FTP（文件传输协议）、 Gopher、 HTTP（超文本传输协议）、 Telent（远程终端协议）、SMTP（简单邮件传送协议）、 IRC（因特网中继会话）、 NNTP（网络新闻传输协议）等 

### 客户机/服务器模型 

服务器端特征：

1. 被动通信；
2. 始终等待来自客户端的请求；
3. 自己参与通信的网络接口和端口必须确定；
4. 处理客户端的请求后将结果（响应）返回给客户端。 

客户端的特征 

- 主动通信；
- 需要发起请求；
- 自己参与通信的网络接口和端口可以不确定；
- 发起请求后需要等待服务器回应结果。 

## 编程接口 BSD Socket 

- 网络 Socket 
  - 支持很多种不同的协议 
- 本地 Socket 两种 
  - Unix Domain Socket，用于进程间通信 
  - Netlink ，用于用户空间和内核空间通讯 

### 基础数据结构和函数 

#### 地址表示数据结构 

```c
struct sockaddr_in
{
  __SOCKADDR_COMMON (sin_);
  in_port_t sin_port; /* Port number. */
  struct in_addr sin_addr; /* Internet address. */

  /* Pad to size of `struct sockaddr'. */
  unsigned char sin_zero[sizeof (struct sockaddr) -
                         __SOCKADDR_COMMON_SIZE -
                         sizeof (in_port_t)
                         sizeof (struct in_addr)];
};
```

``` c
typedef uint32_t in_addr_t;
struct in_addr
{
  in_addr_t s_addr;
};
```

#### 网络字节序和本地字节序之间的转换 

```c
uint32_t htonl(uint32_t hostlong); //32 位整数从主机字节序转换为网络字节序；
uint16_t htons(uint16_t hostshort);//16 位整数从主机字节序转换为网络字节序；
uint32_t ntohl(uint32_t netlong)://32 位整数从网络字节序转换为主机字节序；
uint16_t ntohs(uint16_t netshort);//16 位整数从网络字节序转换为主机字节序。
```

#### 主机名和地址转换函数 

IP 地址的点分十进制表示和二进制表示之间相互转化 

```c
in_addr_t inet_addr(const char *cp)//将一个点分十进制的 IP 地址字符串转换成 in_addr_t 类型
```

```c
char *inet_ntoa(struct in_addr in)//将结构 struct in_addr 中的二进制 IP 地址转换为一个点分十进制表示的字符串，返回这个字符串的首指针。
```

通过主机名获取 IP 地址 

```c
struct hostent *gethostbyname(const char *name);//根据主机名字符串返回一个 struct hostent 结构

struct hostent *gethostbyname2(consts char *name, in af);
```

struct hostent 在 Linux 下的原型 

```c
struct hostent
{
  char *h_name; /* Official name of host. */
  char **h_aliases; /* Alias list. */
  int h_addrtype; /* Host address type. */
  int h_length; /* Length of address. */
  char **h_addr_list; /* List of addresses from name server. */
  #if defined __USE_MISC || defined __USE_GNU
  # define h_addr h_addr_list[0] /* Address, for backward compatibility.*/
  #endif
};
```

### BSD Socket 常用操作 

#### 创建 Socket 

```c
int socket(int domain, int type, int protocol);
// domain 代表这个 Socket 所使用的地址类型，对于我们讨论的 IPv4 协议的IP 地址，使用 AF_INET，也可以使用 PF_INET。
//type 代表了这个 Socket 的类型: SOCK_STREAM(TCP) 和 SOCK_DGRAM(UDP)
//protocol 是协议类型，都取 0 即可
//成功返回一个有效的文件描述符，出错时返回-1
```

创建 TCP Socket：

```c 
sock_fd = socket(AF_INET, SOCK_STREAM, 0);
```

创建 UDP Socket：

```c
sock_fd = socket(AF_INET, SOCK_DGRAM, 0); 
```

#### 绑定地址和端口 

```c
int bind(int socket, const struct sockaddr *address, socklent address_len);
//socket 应该是一个指向 Socket 的有效文件描述符
//address 参数就是一个指向 struct sockaddr 结构的指针
//address_len sizeof(struct sockaddr_in)
//成功时返回 0，失败时返回-1
```

```c
struct sockaddr_in server_addr;

(void)memset(&server_addr, 0, sock_len);
server_addr.sin_family = AF_INET;
server_addr.sin_addr.s_addr = htonl(INADDR_ANY);
server_addr.sin_port = htons(80);

if (bind(server_sock, (struct sockaddr *)&server_addr, sizeof(server_addr))) {
  perror("bind(2) error");
  goto err;
}
```

#### 连接服务器 

对于客户机， 使用 TCP 协议时，在通讯前必须调用 connect连接到服务器的特定通信端口后才能正确进行通信。 

对于使用 UDP 协议的客户机，这个步骤是可选项。 

```c
int connect(int socket, const struct sockaddr *address, socklent address_len);
```

```c
struct sockaddr_in server_addr;

(void)memset(&server_addr, 0, sizeof(server_addr));
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(7007);

server_addr.sin_addr.s_addr = inet_addr("192.168.0.1");
if (connect(conn_sock, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
  perror("connect(2) error");
  goto err;
}
```

#### 设置 Socket 为监听模式 

基于 TCP 协议的服务器，需调用 listen函数将其 Socket 设置成被动模式，等待客户机的连接。  

```c
int listen(int socket, int backlog);
//backlog 是指等待连接的队列长度
//调用成功返回 0，失败返回-1
```

#### 接受连接 

TCP 服务器还需要调用 accept来处理到来的客户机连接请求。 

```c
int accept(int socket, struct sockaddr *restrict address, socklen_t *restrict address_len);
// address 返回请求连接的客户端的地址和端口。
//address_len 返回的地址数据结构的长度
//成功返回一个有效的文件描述符， 此文件描述符指向成功与客户端建立连接可以进行数据交换的 Socket。服务器程序使用这个文件描述符来与客户端进行后续交互。
//返回-1时应该检查 errno 是否为 EINTR，如果是被信号中断的，程序一般需要重新启动 accept()调用。
```

#### 读数据函数 

```c
ssize_t read(int fd, void *buf, size_t count); //TCP
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
struct sockaddr *src_addr, socklen_t *addrlen);
ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);
```

#### 写数据函数 

```c
ssize_t write(int fd, const void *buf, size_t count);
ssize_t send(int sockfd, const void *buf, size_t len, int flags);
ssize_t sendto(int sockfd, const void *buf, size_t len, int flags,
const struct sockaddr *dest_addr, socklen_t addrlen);
ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);
```

## TCP/UDP ECHO 服务器 

### 面向流的 Socket 

面向流的 ECHO 服务器就是使用 TCP 协议 

![](https://note.youdao.com/yws/public/resource/ea43181f60c17fcc4449163ab62b9e2c/xmlnote/WEBRESOURCE1c159f368bfb7881086ff859183efa6d/9388)

#### TCP server示例

```c
#define LISTEN_PORT	((uint16_t)7007)
#define BUFF_SIZE	(1024 * 4)

void zombie_cleaning(int signo)
{
    int status;
    (void)signo;
    while (waitpid(-1, &status, WNOHANG) > 0);
}

int tcp_echo(int client_fd)
{
    char				buff[BUFF_SIZE]	= {0};
    ssize_t				len				= 0;
	//接收客户端数据
    len	= read(client_fd, buff, sizeof(buff));
    if (len < 1) {
        goto err;
    }
	//向客户端发送数据
    (void)write(client_fd, buff, (size_t)len);

    return EXIT_SUCCESS;
 err:
    return EXIT_FAILURE;
}

int main(void)
{
    int server_sock, conn_sock;
    struct sockaddr_in server_addr, client_addr;
    socklen_t	sock_len	= sizeof(client_addr);
    pid_t	chld_pid;
    struct sigaction clean_zombie_act;

  	//创建 TCP Socket
    server_sock	= socket(AF_INET, SOCK_STREAM, 0);
    if (server_sock < 0) {
        perror("socket(2) error");
        goto create_err;
    }

    (void)memset(&server_addr, 0, sock_len);
    server_addr.sin_family		= AF_INET;
    server_addr.sin_addr.s_addr	= htonl(INADDR_ANY);
    server_addr.sin_port		= htons(LISTEN_PORT);
	//绑定地址和端口
    if (bind(server_sock, (struct sockaddr *)&server_addr, sizeof(server_addr))) {
        perror("bind(2) error");
        goto err;
    }
	//设置 Socket 为监听模式
    if (listen(server_sock, 5)) {
        perror("listen(2) error");
        goto err;
    }

    (void)memset(&clean_zombie_act, 0, sizeof(clean_zombie_act));
    clean_zombie_act.sa_handler	= zombie_cleaning;
    if (sigaction(SIGCHLD, &clean_zombie_act, NULL) < 0) {
        perror("sigaction(2) error");
        goto err;
    }

    while (true) {
      	//接受连接
        sock_len	= sizeof(client_addr);
        conn_sock	= accept(server_sock, (struct sockaddr *)&client_addr, &sock_len);
        if (conn_sock < 0) {
            if (errno == EINTR) {
                /* restart accept(2) when EINTR */
                continue;
            }
            goto end;
        }

        printf("client from %s:%hu connected\n",
               inet_ntoa(client_addr.sin_addr),
               ntohs(client_addr.sin_port));
        fflush(stdout);
		
      	//产生子线程
        chld_pid	= fork();
        if (chld_pid < 0) {
            /* fork(2) error */
            perror("fork(2) error");
            close(conn_sock);
            goto err;
        } else if (chld_pid == 0){
            /* child process */
            int ret_code;

            close(server_sock);
            ret_code	= tcp_echo(conn_sock);
            close(conn_sock);

            /* Is usage of inet_ntoa(2) right? why? */
            printf("client from %s:%hu disconnected\n",
               inet_ntoa(client_addr.sin_addr),
               ntohs(client_addr.sin_port));

            exit(ret_code);
        } else {
            /* parent process */
            continue;
        }
    }

 end:
    perror("exit with:");
    close(server_sock);
    return EXIT_SUCCESS;
 err:
    close(server_sock);
 create_err:
    fprintf(stderr, "server error");
    return EXIT_FAILURE;
}

```

#### TCP client示例

```c
#define SERVER_IP	"192.168.1.133"
#define SERVER_PORT	((uint16_t)7007)
#define BUFF_SIZE	(1024 * 4)

int main(int argc, char *argv[])
{
    int conn_sock;
    char test_str[BUFF_SIZE]	= "tcp echo test";
    struct sockaddr_in	server_addr;

  	//创建 TCP Socket
    conn_sock	= socket(AF_INET, SOCK_STREAM, 0);
    if (conn_sock < 0) {
        perror("socket(2) error");
        goto create_err;
    }

    (void)memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family		= AF_INET;
    server_addr.sin_port		= htons(SERVER_PORT);
    if (argc != 3) {
        server_addr.sin_addr.s_addr	= inet_addr(SERVER_IP);
    } else {
        server_addr.sin_addr.s_addr	= inet_addr(argv[1]);
        snprintf(test_str, BUFF_SIZE, "%s", argv[2]);
    }

  	//连接服务器
    if (connect(conn_sock,
                (struct sockaddr *)&server_addr,
                sizeof(server_addr)) < 0) {
        perror("connect(2) error");
        goto err;
    }
	//向服务器发送数据
    if (write(conn_sock, test_str, strlen(test_str)) < 0) {
        perror("send data error");
        goto err;
    }
  	//从服务器中读数数据
    (void)memset(test_str, 0, BUFF_SIZE);
    if (read(conn_sock, test_str, BUFF_SIZE) < 0) {
        perror("receive data error");
        goto err;
    }
    printf("%s\n", test_str);
    fflush(stdout);

    return EXIT_SUCCESS;

 err:
    close(conn_sock);
 create_err:
    fprintf(stderr, "client error");
    return EXIT_FAILURE;
}
```

#### 线程服务器

```c
//线程执行函数负责读写
void *thr_fn(void *arg)
{
	int size,j;
    char recv_buf[1024];
	int *parg=(int *)arg;
	int new_fd=*parg;
	printf("new_fd=%d\n",new_fd);
	while((size=read(new_fd,recv_buf,1024))>0)
	{
		if(recv_buf[0]=='@')
			break;
		printf("Message from client(%d): %s\n",size,recv_buf);
		for(j=0;j<size;j++)
			recv_buf[j]=toupper(recv_buf[j]);
		write(new_fd,recv_buf,size);
	}
	close(new_fd);
	return 0;
}


int main(int argc,char *argv[])
{
    socklen_t clt_addr_len;
	int listen_fd;
	int com_fd;
	int ret;
	int i;
	static char recv_buf[1024];
	int len;
	int port;
	pthread_t tid;

	struct sockaddr_in clt_addr;
	struct sockaddr_in srv_addr;

	//服务器端运行时要给出端口信息，该端口为监听端口 
	if(argc!=2)
	{
	    printf("Usage:%s port\n",argv[0]);
		return 1;
	}

	//获得输入的端口 
	port=atoi(argv[1]);

	//创建套接字用于服务器的监听 
	listen_fd=socket(PF_INET,SOCK_STREAM,0);
	if(listen_fd<0)
	{
	    perror("cannot create listening socket");
		return 1;
	}

	//填充关于服务器的套节字信息
	memset(&srv_addr,0,sizeof(srv_addr));
	srv_addr.sin_family=AF_INET;
	srv_addr.sin_addr.s_addr=htonl(INADDR_ANY);
	srv_addr.sin_port=htons(port);


	//将服务器和套节字绑定
	ret=bind(listen_fd,(struct sockaddr *)&srv_addr,sizeof(srv_addr));
	if(ret==-1)
	{
	    perror("cannot bind server socket");
		close(listen_fd);
		return 1;
	}

	//监听指定端口，连接5个客户端 
	ret=listen(listen_fd,5);
	if(ret==-1)
	{
	    perror("cannot listen the client connect request");
		close(listen_fd);
		return 1;
	}
	//对每个连接来的客户端创建一个线程，单独与其进行通信 
	//首先调用read函数读取客户端发送来的信息 
	//将其转换成大写后发送回客户端 
	//当输入“@”时，程序退出 
	while(1)
	{
	    len=sizeof(clt_addr);
		com_fd=accept(listen_fd,(struct sockaddr *)&clt_addr,&len);
		if(com_fd<0)
		{
		    if(errno==EINTR)
			{
			    continue;
			}
			else
			{
			    perror("cannot accept client connect request");
				close(listen_fd);
				return 1;
			}
		}
		printf("com_fd=%d\n",com_fd);//打印建立连接的客户端产生的套节字
		if((pthread_create(&tid,NULL,thr_fn,&com_fd))==-1)
		{
		    perror("pthread_create error");
			close(listen_fd);
			close(com_fd);
			return 1;
		}
	}
	return 0;
}
```

```c
int main(int argc,char *argv[])
{
    int connect_fd;
	int ret;
	char snd_buf[1024];
	int i;
	int port;
	int len;

	static struct sockaddr_in srv_addr;

	if(argc!=3)
	{
	    printf("Usage: %s server_ip_address port\n",argv[0]);
		return 1;
	}

	//获得输入的端口 
	port=atoi(argv[2]);

	//创建套接字连接服务器
	connect_fd=socket(PF_INET,SOCK_STREAM,0);
	if(connect_fd<0)
	{
	    perror("cannot create communication socket");
		return 1;
	}

	//填充关于服务器的套节字信息
	memset(&srv_addr,0,sizeof(srv_addr));
	srv_addr.sin_family=AF_INET;
	srv_addr.sin_addr.s_addr=inet_addr(argv[1]);
	srv_addr.sin_port=htons(port);

	//连接服务器
	ret=connect(connect_fd,(struct sockaddr *)&srv_addr,sizeof(srv_addr));
	if(ret==-1)
	{
	    perror("cannot connect to the server");
		close(connect_fd);
		return 1;
	}

	memset(snd_buf,0,1024);
	while(1)
	{
	    write(STDOUT_FILENO,"input message:",14);
		len=read(STDIN_FILENO,snd_buf,1024);
		if(len>0)
			write(connect_fd,snd_buf,len);
		len=read(connect_fd,snd_buf,len);
		if(len>0)
			printf("Message form server: %s\n",snd_buf);
		if(snd_buf[0]=='@')
			break;
	}
	close(connect_fd);
	return 0;
}
```



### 面向数据报的 Socket 

面向流的 ECHO 服务器就是使用 UDP 协议 。

#### UDP server示例

```c
#define SERVER_PORT	((uint16_t)7007)
#define BUFF_SIZE	(1024 * 4)

int udp_echo(int client_fd)
{
    char				buff[BUFF_SIZE]	= {0};
    ssize_t				len				= 0;
    struct sockaddr_in	source_addr;
    socklen_t	addr_len	= sizeof(source_addr);
	//等待从客户端获取数据
    (void)memset(&source_addr, 0, addr_len);
    len	= recvfrom(client_fd, buff, BUFF_SIZE, 0,
                   (struct sockaddr *)&source_addr, &addr_len);
    if (len < 1) {
        perror("recvfrom(2) error");
        goto err;
    }
	//发送数据到客户端
    len	= sendto(client_fd, buff, len, 0,
                 (struct sockaddr *)&source_addr, addr_len);
    if (len < 1) {
        perror("sendto(2) error");
        goto err;
    }

    printf("Served client %s:%hu\n",
           inet_ntoa(source_addr.sin_addr),
           ntohs(source_addr.sin_port));
    fflush(stdout);

    return EXIT_SUCCESS;
 err:
    return EXIT_FAILURE;
}

int main(void)
{
    int server_sock;
    struct sockaddr_in server_addr;
    socklen_t	sock_len	= sizeof(server_addr);
	//创建 UDP Socket
    server_sock	= socket(AF_INET, SOCK_DGRAM, 0);
    if (server_sock < 0) {
        perror("socket(2) error");
        goto create_err;
    }

    (void)memset(&server_addr, 0, sock_len);
    server_addr.sin_family		= AF_INET;
    server_addr.sin_addr.s_addr	= htonl(INADDR_ANY);
    server_addr.sin_port		= htons(SERVER_PORT);
	//绑定地址和端口
    if (bind(server_sock, (struct sockaddr *)&server_addr, sizeof(server_addr))) {
        perror("bind(2) error");
        goto err;
    }

    while (true) {
        if (udp_echo(server_sock) != EXIT_SUCCESS) {
            goto err;
        }
    }

    perror("exit with:");
    close(server_sock);
    return EXIT_SUCCESS;
 err:
    close(server_sock);
 create_err:
    fprintf(stderr, "server error");
    return EXIT_FAILURE;
}
```

#### UDP client示例

```c
#define SERVER_IP	"192.168.1.133"
#define SERVER_PORT	((uint16_t)7007)
#define BUFF_SIZE	(1024 * 4)

int main(int argc, char *argv[])
{
    int conn_sock;
    char test_str[BUFF_SIZE]	= "udp echo test";
    struct sockaddr_in	server_addr;
    socklen_t	addr_len	= sizeof(server_addr);
    fd_set	sockset;
    struct timeval timeout	= {
        .tv_sec	= 3,
    };
	//创建 UDP Socket
    conn_sock	= socket(AF_INET, SOCK_DGRAM, 0);
    if (conn_sock < 0) {
        perror("socket(2) error");
        goto create_err;
    }

    (void)memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family		= AF_INET;
    server_addr.sin_port		= htons(SERVER_PORT);
    if (argc != 3) {
        server_addr.sin_addr.s_addr	= inet_addr(SERVER_IP);
    } else {
        server_addr.sin_addr.s_addr	= inet_addr(argv[1]);
        snprintf(test_str, BUFF_SIZE, "%s", argv[2]);
    }
	//不需要绑定地址与端口，直接使用它们发送数据到服务器
    if (sendto(conn_sock, test_str, strlen(test_str), 0,
               (struct sockaddr *)&server_addr, addr_len) < 0) {
        perror("send data error");
        goto err;
    }

    while (true) {
        int num	= 0;
	    //等待服务器回发的数据
        FD_ZERO(&sockset);
        FD_SET(conn_sock, &sockset);
        num	= select(conn_sock + 1, &sockset, NULL, NULL, &timeout);//UDP 可能会丢包，需要设置超时
        if (num < 0) {
            if (errno == EINTR) {
                continue;
            } else {
                perror("select(2) error");
                goto err;
            }
        } else if (num == 1) {	//正常等到数据
            if (FD_ISSET(conn_sock, &sockset)) {
                break;
            }
        } else if (num == 0) {
            fprintf(stderr, "%s\n", "Waiting for echo time out!");
            goto err;
        } else {
            fprintf(stderr, "%s\n", "Code should NOT reach here");
            goto err;
        }
    }
	//读取数据
    (void)memset(test_str, 0, BUFF_SIZE);
    if (recvfrom(conn_sock, test_str, BUFF_SIZE, 0,
                 (struct sockaddr *)&server_addr, &addr_len) < 0) {
        perror("receive data error");
        goto err;
    }
    printf("get echo from %s:%hu\n",
           inet_ntoa(server_addr.sin_addr),
           ntohs(server_addr.sin_port));
    printf("%s\n", test_str);
    fflush(stdout);

    return EXIT_SUCCESS;

 err:
    close(conn_sock);
 create_err:
    fprintf(stderr, "client error");
    return EXIT_FAILURE;
}
```



# 第十二章 Shell编程

## 基础概念 

Unix Shell 的设计哲学就是“字符流+过滤器” ，一个程序的输出，要能够方便地变成另外一个程序的输入，这样就能够把许许多多用途简单的小工具组合起来，完成一些看起来不可思议的奇妙功能。 



Shell 程序一般被称为脚本(Script)，它其实就是一组命令的集合 。

Shell 脚本只需要给脚本文件加上可执行权限 便可执行。



Shell 解释脚本的过程就是从一个文件读入字符流，然后进行处理，最后把结果送到一个文件，故交互式 Shell 与执行脚本的 Shell 本质上并无区别。只不过交互式运行的 Shell的输入文件是标准输入，输出文件是标准输出。  



### Sha-Bang 

Sha-Bang 就是通常脚本开头的头两个字符“#!”连在一起的读音。 

“#!”是一个魔数（Magic，其值为 0x23,0x21），可执行文件在被读取的时候，内核通
过这个特定的数字组合识别出这是一个需要运行解释器的脚本，并且根据约定将其后的字符串在读到换行以前解释成该脚本需要的解释器所在路径。 然后系统根据路径调用解释器，最后再把整个文本的内容传递给解释器。 



以“#!/bin/sh”或“#!/bin/bash”开头，表明脚本使用的解释器是 sh(POSIX Shell)或者 bash。 

```shell
#!/bin/sh
# hello world demo.
echo "hello world" # 打印 hello world
```



执行脚本通常有五种方法 

```sh
$ ./hello.sh
$ sh hello.sh
$ sh < hello.sh #将脚本文件重定向到 shell 程序的标准输入
$ cat hello.sh | sh #通过一个管道将脚本内容输出到 shell 程序的标准输入
$ source hello.sh
```



### 字符串与引号 

- 单引号(' ') 

  ​单引号中的字符串 Shell 不会做任何处理，在需要保持字符串原样不变的时候使用。 

```shell
ver = 123
$ echo '$ver'
```

- 双引号(" ") 

  ​双引号中的字符串 Shell 会进行处理，若其中含有可以求值的部分，会被 Shell 替换为求值的结果，其中包含变量、表达式或命令。 

```shell
ver = 123
$ echo "$ver"
```

- 反引号(` `) 

  ​一般用来引用一条命令，并且将这个命令的输出结果（输出到标准输出上）作为这个字符串最终的值，作用与符号“$()”相同。 



### 特殊字符 

- 星号(*)和问号(?)
  - **一般用作通配符，可以用来匹配文件名字，“*”匹配任意多个字符，“?”匹配任意一个字符。 


- 冒号(:)

  - 表示空命令(NOP no-op)，因其返回值恒为 0，在循环条件中，可与 true 命令等价。 

- 分号(;)

  - 是分行符，可以表示一行命令结束，可用分号将多条命令写在一行中。 

- 美元符($)
  - 用于取值， 根据其后的不同结构，可以取变量或表达式的值。 
  - ${var}和$var 均是取变量 var 的值 ,使用大括号({ })可以当变量作为在一个字符串的一部分的时候，变量名不会和字符串内容混淆。 
  - $()可以取一个命令的值作为字符串内容，与反引号()含义相同。 
  - $(())可以取一个数学表达式的值 

- 句点(.)

  - 等效于 source 命令。 

- 反斜线(\) 

  - 转义符，是一种引用单个字符的方法。 一个具有特殊含义的字符前加上转义
    符就是告诉 Shell 该字符失去了特殊含义。 

  - ```shell
    $ touch a b	#创建文件 a 和 b 两个文件
    $ touch c\ d #创建文件名为“c d” 
    ```

  - 有了转义符，将换行符转义，就可以实现 Shell 命令的跨行 。




## 必要高级概念 

### 内部命令和外部命令 

- 外部命令 
  - 这个命令其实是一个独立的外部程序 ，如同/bin/ls 这样，是一个独立的可执行文件 。
  - 当外部命令被调用时，其实就是调用了另外一个软件， Shell 会先创建子进程，然后再在子进程里执行这个软件。 其效率往往被诟病。所以 Shell 程序一般不会用于需要高性能的场合。 
- 内部命令 
  - 命令内建在 Shell 软件中 ，如cd、 source、 export、 time 等命令，必须以内部命令的形式实现。 

### I/O 重定向与管道 

默认情况下，任何一个进程都至少有三个已经打开的“文件”：标准输入(stdin)、标准
输出(stdout)和标准错误(stderr)，它们对应的文件描述符通常情况下默认为 0、 1 和 2。 



I/O 重定向其实就是捕捉一个文件、命令、程序、脚本甚至代码块的输出，然后将这个 输出作为输入发送给另外一个文件、命令、程序或脚本中。 



#### 输出重定向 

“>”和“>>” ，可以把标准输出的内容重定向到一个文件中，如果目标文件不存在则创建文件。 “>”会将原文件的长度截断为 0，即表现为覆盖原文件；而“>>”会在文件已经存在的时候将内容追加在文件的原内容之后 。

#### 输入重定向 

“<”和“<<”是输入重定向，用来将命令的标准输入重定向到一个文件。  

“<”一般跟着一个文件 ；“<<”一般用于 Here Document，就是将一段文本直接写在脚本之中，以特定的字符序列来表示终止。这一段文字其实就相当于一个独立文件的内容。  

```shell
#!/bin/bash

echo "hd-new doesn't exist"
ls -l # 先验证目录下已有的文件
cat > hd-new.c << EOF # 使用 Here Document 的方式产生 hd-new.c 文件
#include <stdio.h>
#include <stdlib.h>
int main(void)
{
  printf("hello here documents\n")
  return EXIT_SUCCESS;
}
EOF # 源代码文件内容结束
cc -W -Wall -o hd-new hd-new.c # 编译产生 hd-new 二进制文件
ls –l # 验证现在产生的新文件
./hd-new # 执行程序
rm hd-new hd-new.c # 删除新产生的文件
```

#### 管道 

管道符（|）用于连接命令，将前一条命令的标准输出内容变成下一条命令的标准输入。 

管道有一个重要的特征是管道符两边是不同的进程。 



### 常量、变量与环境变量 

#### 基本概念 

变量使用前不必声明。赋值时直接使用变量名，且等号两边不能有空格。 

引用变量时一定要使用“$”符号 。

局部变量须用 local 关键字来声明，它只在自己被声明使用的块或函数中可见。 

通过$?可以引用上一个命令的返回值。  

ture命令返回值为0，false命令返的回值恒为 1。

#### 环境变量 

环境变量是可以改变用户接口和 Shell 行为的变量。每一个进程都有自己的环境变量，用于保存进程可能有用的信息。 

任何一个变量都可以使用 export 导出成为环境变量，环境变量可以被子进程继承。所以它也是父进程给子进程传递信息的一种方式。 

#### 位置参数 

位置参数就是按照位置来引用的命令行参数。 在脚本中按照顺序$0、 $1„„来引用，依此类推。其中， $0 代表命令本身 。

```shell
#!/bin/sh
echo $0
echo $1
echo $2
echo $3
```



命令行参数的特殊变量还有三个： \$\#  , \$\* 和$@ 

| 变量   | 说明                                   |
| ---- | ------------------------------------ |
| $#   | 代表命令行参数的个数                           |
| $*   | 代表全部的命令行参数，而且全部参数作为一个单词。引用时必须在“”之中   |
| $@   | 代表所有的命令行参数，但是每个参数都是一个独立的单词，引用时也要在“”中 |

说明：以上三个变量在统计命令行参数时均不包含$0。 

```shell
#!/bin/sh
echo $#
touch "$*"
ls -l
touch "$@"
ls -l
```



#### 操作符与表达式 

- 数学运算符 

+、 -、 *、 /、 **、 %。除了**代表的幂运算外，其它运算符与对应的 C 语言运算符意义相同。但 Bash 只支持整数运算，如果需要浮点运算，应该需要调用外部的工具。 

- 逻辑操作符 

&&和||，分别代表逻辑“与”和逻辑“或”。 

对于&&来说，若左边的表达式为假，右边表达式将不用被执行即可确定整个表达式结果为假 。



## 脚本编程 

### 脚本返回值 

脚本也是一个程序， 故 Shell 脚本应该返回一个值，若脚本未显式指定返回值，则自动使用最后一条命令的返回值；如果需要显式指定脚本的返回值，需要用 exit 命令实现。 

```shell
#!/bin/sh
echo "hello world"
false #false 命令返回值恒为 1
exit 0 #显式指定脚本返回 0
```

### 函数 

Bash 里的函数行为就像是一个独立的子脚本，Shell 中的函数和一个独立的命令区别并不大。 

- 两种定义函数的方法 

```shell
function function_name {
	command...
}

function_name () {
	command...
}
```

Bash 中没有类似于 C 中提前“声明”函数的方法，任何函数都应该在其被调用前完整定义。 

- 函数定义和调用示例 

```shell
#!/bin/sh
echo "demo for function and call"
fun() {
	echo "I‘am in func()"
}
fun
echo "end"
```

### 条件测试  test 

条件测试 有外部命令 test（别名“[”） ，还有内建的结构“[]” 或“[[]]” 。

使用“[]”或者“[[]]”结构时，应该注意测试表达式与 test 符号之间应该留有空白。 



支持很多种不同类型条件的测试 ，可分为文件测试、整数测试和字符串测试等几大类。 

#### 文件测试 

文件测试通常基于文件属性进行判断，在系统管理脚本或者启动脚本中很常用。 

| 条件        | 含义                                       | 范例                                |
| --------- | ---------------------------------------- | --------------------------------- |
| -e/-a     | 文件存在（-a 与-e 相同，但-a 已经被弃用，不鼓励再使用）         | [ -e ~/.bashrc ]                  |
| -f        | 普通文件                                     | [ -f ~/.profile ]                 |
| -s        | 文件长度不为 0                                 | [ -s /etc/mtab ]                  |
| -d        | 文件是目录                                    | [ -d /etc ]                       |
| -b        | 文件是块设备文件                                 | [ -b /dev/sda ]                   |
| -c        | 文件是字符设备                                  | [ -c /dev/ttyS0 ]                 |
| -p        | 文件是管道                                    | [ -p /tmp/fifo ]                  |
| h/-L      | 文件是符号链接                                  | [ -L /etc/mtab ]                  |
| S         | 文件是 Socket                               | [ -S /tmp/socket ]                |
| -t        | 是一个关联到终端的文件描述符（一般用来检测在一个给定脚本中的 stdin[-t0]或[-t1]是否是一个终端） | [ -t /dev/stdout ]                |
| -r        | 文件可读                                     | [ -r ~/.bashrc ]                  |
| -w        | 文件可写                                     | [ -w ~/.profile ]                 |
| -x        | 文件可执行                                    | [ -x /bin/ls ]                    |
| -g        | 文件有 SGID 标识                              | [ -g /bin/su ]                    |
| -u        | 文件有 SUID 标识                              | [ -u /usr/bin/sudo ]              |
| -k        | 具有粘滞位（常见/tmp 目录的属性）                      | [ -k /tmp ]                       |
| -O        | 测试者是文件拥有者                                | [ -O ~/.bashrc ]                  |
| G         | 文件的组 ID 与测试者相同                           | [ -G ~/.profile ]                 |
| -N        | 从文件最后被阅读到现在，是否被修改过                       | [ -N ~/.profile ]                 |
| f1 -nt f2 | f1 文件较新                                  | [ ~/.bashrc –nt ~/.profile ]      |
| f1 -ot f2 | f1 文件较旧                                  | [ ~/.bashrc –ot ~/.profile ]      |
| f1 -ef f2 | 两个文件是同一个文件的硬链接                           | [ /usr/bin/test -ef /usr/bin/\[ ] |
| !         | 反转以上测试结果，若没有条件则返回 true                   | [ ! -d ~/.profile ]               |

#### 整数比较 

| 条件   | 含义   | 备注          | 范例               |
| ---- | ---- | ----------- | ---------------- |
| -eq  | 等于   |             | ["$m" -eq "$n" ] |
| -ne  | 不等于  |             | ["$m" -ne "$n" ] |
| -gt  | 大于   |             | ["$m" -gt "$n" ] |
| -ge  | 大于等于 |             | ["$m" -ge "$n" ] |
| -lt  | 小于   |             | ["$m" -lt "$n" ] |
| le   | 小于等于 |             | ["$m" -le "$n" ] |
| <    | 小于   | 需要用(())进行测试 | (("$m" < "$n"))  |
| <=   | 小于等于 | 需要用(())进行测试 | (("$m" <= "$n")) |
| >    | 大于   | 需要用(())进行测试 | (("$m" > "$n"))  |
| >=   | 大于等于 | 需要用(())进行测试 | (("$m" >= "$n")) |

#### 字符串比较 

| 条件   | 含义     | 备注                           | 范例                    |
| ---- | ------ | ---------------------------- | --------------------- |
| =/== | 相等     | ==在[[]]和[]中行为可能不同            | ["$str1" = "$str2" ]  |
| !=   | 不相等    |                              | ["$str1" != "$str2" ] |
| >    | 大于     | 按照 ASCII 字母顺序比较，在[]中需要转义写成\> | ["$str1" \> "$str2" ] |
| <    | 小于     | 按照 ASCII 字母顺序比较，在[]中需要转义写成\< | ["$str1" \< "$str2" ] |
| -z   | 长度为 0  |                              | [ -z "$str" ]         |
| -n   | 长度不为 0 | 在[]中使用，应该将字符串放入“”中           | [ -n "$str" ]         |



### 流程控制 

#### 条件 

```shell
if 条件
then
	代码块
fi

或
if 条件； then
	代码块
fi
```

```shell
if 条件； then
	代码块 1
else
	代码块 2
fi
```

#### 循环 

```shell
for arg in [list]
do
	命令
done
//list 是一个字符串的列表，由空白字符分隔。 

for wd in Monday Tuesday Wednesday Thursday Friday; do
	echo $wd
done
```

C 风格的 for 循环条件 

```shell
for ((表达式 1; 表达式 2; 表达式 3))
do
	命令
done

for ((i=0; i < 7; i++))
do
	echo $i
done
```

##### while 循环 

```shell
while [条件]
do
	命令
done

while ((表达式))
do
	命令
done
```

##### until 循环 

```shell
until [条件]
do
	命令
done

until ((表达式))
do
	命令
done
```

(()) 所支持的 C 风格循环条件测试，脚本第一行的解释器必须是 bash，只有较新版
本的 Bash 才支持这种风格。 

#### 分支 

```shell
case " $var" in
" $cond1")
	命令
;;
" $cond2")
	命令
;;
*)
	命令
;;
esac
```































































































































































































































