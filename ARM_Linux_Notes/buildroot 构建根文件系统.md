# buildroot构建根文件系统

[TOC]

## 一、简介

Buildroot是Linux平台上一个构建嵌入式Linux系统的框架。整个Buildroot是由Makefile脚本和Kconfig配置文件构成的。本文使用buildroot为freescale imx287 开发板制作根文件系统。

参考 <https://www.cnblogs.com/kele-dad/p/8231434.html>



### 1.buildroot目录说明

```shell
.
├── arch: 目录存放CPU架构相关的配置脚本，如arm/mips/x86 ，这些CPU相关的配置，在制作工具链，编译boot和内核时很关键。
├── board：存放了一些默认开发板的配置补丁之类的
├── boot
├── CHANGES
├── Config.in
├── Config.in.legacy
├── configs: 放置開發板的一些配置參數. 
├── COPYING
├── DEVELOPERS
├── dl: 存放下載的源代碼及應用軟件的壓縮包. 
├── docs: 存放相關的參考文檔. 
├── fs: 放各種文件系統的源代碼. 
├── linux: 存放着Linux kernel的自動構建腳本. 
├── Makefile
├── Makefile.legacy
├── output: 是編譯出來的輸出文件夾. 
│   ├── build: 存放解壓後的各種軟件包編譯完成後的現場.
│   ├── host: 存放着製作好的編譯工具鏈，如gcc、arm-linux-gcc等工具.
│   ├── images: 存放着編譯好的uboot.bin, zImage, rootfs等鏡像文件，可燒寫到板子裏, 讓linux系統跑起來.
│   ├── staging
│   └── target: 用來製作rootfs文件系統，裏面放着Linux系統基本的目錄結構，以及編譯好的應用庫和bin可執行文件. (buildroot根據用戶配置把.ko .so .bin文件安裝到對應的目錄下去，根據用戶的配置安裝指定位置)
├── package：下面放着應用軟件的配置文件，每個應用軟件的配置文件有Config.in和soft_name.mk。
├── README
├── support
├── system：这里就是根目录的主要骨架和相关的启动初始化配置,当制作根目录时就是将此处的文件cp到output里去.然后再安装toolchain的动态库和你勾选的package的可执行文件之类的.
└── toolchain
```

### 2.buildroot工作原理

Buildroot本身提供构建流程的框架，开发者按照格式写脚本，提供必要的构建细节，配置整个系统，最后自动构建出你的系统。

- buildroot的编译流程是先从dl/xxx.tar下解压出源码到output/build/xxx, 然后它利用本身的配置文件(如果有的话)覆盖output/build/xxx下的配置文件,在开始编译连接完成后安装到output/相应文件夹下.
- Buildroot提供了函数框架和变量命令框架，采用它的框架编写的app_pkg.mk这种Makefile格式的自动构建脚本，将被package/pkg-generic.mk 这个核心脚本展开填充到buildroot主目录下的Makefile中去。
- package/pkg-generic.mk中通过调用同目录下的pkg-download.mk、pkg-utils.mk文件，已经帮你自动实现了下载、解压、依赖包下载编译等一系列机械化的流程。你只要需要按照格式写Makefile脚app_pkg.mk，填充下载地址，链接依赖库的名字等一些特有的构建细节即可。

### 3.iMX287开发板硬件说明

1）主板外观及基本接口分布

​	![](https://note.youdao.com/yws/public/resource/9d86dc6165a9983a76017ec5af49d69e/xmlnote/WEBRESOURCE27c216bdbbe0955f25deb25f172a1abf/16777)

2.硬件资源

![](https://note.youdao.com/yws/public/resource/9d86dc6165a9983a76017ec5af49d69e/xmlnote/WEBRESOURCE2a4f7033ff7a0098a1ba961108a12e10/16780)



## 二、构建步骤

### 1.下载buildroot

```shell
git clone git://git.busybox.net/buildroot
```



### 2.配置buildroot

查看buildroot包含的开发板配置

```shell
make list-defconfigs
```

选择imx28默认配置

```shell
cd buildroot
make freescale_imx28evk_defconfig
```

#### 进入menuconfig逐项配置

```shell
make menuconfig
```

![](https://images2017.cnblogs.com/blog/1142140/201801/1142140-20180107155147346-674357698.png)

- Target options（目标配置）
  - Target Architecture：目标架构，这里选择 ARM(little endian)，ARM小端模式
  - Target Binary Format：二进制格式，为 ELF
  - Target Architecture Variant：架构变体为 arm920t，内核类型
  - Target ABI：应用程序二进制接口，为EABI
  - Floating point strategy：浮点数的策略，选择为 Soft float
  - ARM instruction set：arm 汇编指令集，选择  ARM

- Build options（编译选项）

  对编译过程进行一些设置，通常用默认设置即可。 

  ![](https://upload-images.jianshu.io/upload_images/15877540-153fa3f2b77cf2a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- Toolchain（工具链）

  使用内部工具链。

  ![](https://upload-images.jianshu.io/upload_images/15877540-e2cdfb2af4771c86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  也可以手动选择Kernel Header版本和外部工具链。

  注意：实际测试发现，根文件系统与内核使用不同版本的交叉工具链编译，得到的根文件系统镜像文件也可以与内核镜像搭配运行。



- System configuration（系统配置）

  对目标系统进行配置，包括主机名称（System hostname）、欢迎旗标（System banner）、初始化系统（Init system）、设备管理方式（/dev management）、登录方式和 Shell 等。

  ![](https://upload-images.jianshu.io/upload_images/15877540-2c5d565bd2a986a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  “Run a gretty after root”需要根据硬件进行设置，必须与系统调试串口对应。 EasyARM28x 使用默认的 console 即可。

- kernel和bootloaders配置

  内核定制裁剪以及 Bootloader 的定制，建议独立管理， Kernel 和 Bootloaders 这两项留空即可。 

- Target Packages（软件包）

  Buildroot 提供了海量软件包可选，只需在配置界面选中所需要的软件包，交叉编译后即可使用。

  Busybox是必选。

  ![](https://upload-images.jianshu.io/upload_images/15877540-8ba38ab046a432ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- Filesystems images（文件系统镜像选择）

  可以设置生成的文件系统镜像类型 ，如.tar、cpio、ext2/3/4、 jffs2、 yaffs2 和 ubifs 等多种方式 。

  ![](https://upload-images.jianshu.io/upload_images/15877540-82a2b367bf0515ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  如果要生成ubifs，需要知道芯片逻辑擦除块大小、最小IO单元（页大小）、可用物理擦除块数量（PEB）。可以在uboot环境下，执行如下命令得知：

  ```shell
  > mtpart default
  > ubi part rootfs
  ```

  ![](https://upload-images.jianshu.io/upload_images/15877540-87d753545a3eee70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 3.编译buildroot

```shell
make
```

编译完成，在 output 目录下可以得到生成的文件系统和镜像文件 

```shell
$ ls output/
build host images staging target
```

在images目录下有烧录镜像文件



### 4.完善文件系统

1）增加/dev/null 文件 

Buildroot 编译后，生成的文件系统中通常没有/dev/null 文件，而系统启动通常是需要的，可以自行创建： 

```shell
cd output/target/dev/
sudo mknod null c 1 3
```

2）增加/dev/console文件 

对应于System configuration-“Run a gretty after root”选择的调试端口

```shell
cd output/target/dev/
sudo mknod console c 5 1
```

3）再次编译builtroot

```shell
make
```





## 三、使用根文件系统

将生成的rootfs.tar.bz2文件或rootfs.ubifs文件烧录到处理器，进入系统后即可以通过shell命令操作，已有的shell命令参考output/bin/目录下的链接文件。

![](https://upload-images.jianshu.io/upload_images/15877540-823f38e3c096cd67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

开机成功进入根文件系统

![](https://upload-images.jianshu.io/upload_images/15877540-6b07f72f0e719251.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1.登录

因为在System configuration中把root passwd设置为root，所以用户名和密码都是root。

### 2.系统信息查看

- 查看内核版本：cat /proc/version
- 查看内存使用：free
- 查看磁盘使用：df -m
- 查看CPU信息：cat /proc/cpuinfo

### 3.网络设置

- 修改IP地址：ifconfig eth0 192.168.181.251

- 设置默认网关：route add default gw 192.168.181.1

- 设置子网掩码：ifconfig eth0 netmask 255.255.255.0

- 设置广播地址：ifconfig eth0 broadcast 192.168.181.225

- 修改mac地址：ifconfig eth0 hw ether 00:11:22:33:44:55

- 设置DNS：vi /etc/resolv.conf	修改后保存	#设置好DNS才能解析域名

  ```
  nameserver 8.8.8.8 #修改成你的主DNS
  nameserver 8.8.4.4 #修改成你的备用DNS
  search localhost #你的域名
  ```

- 开机自动设置网络参数：vi /etc/rc.d/init.d/start_userapp   将上述命令加入文件中

- 关闭/开启网关：
  - fconfig eth0 down
  - ifconfig eth0 up
- 设置动态获取ip地址：udhcpc		#重启后无效
- 外网ping测试：ping www.baidu.com