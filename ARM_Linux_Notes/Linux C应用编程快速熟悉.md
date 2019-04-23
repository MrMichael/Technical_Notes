# Linux C 应用编程快速熟悉

[TOC]



## 一、文件IO操作

### 1.open与close

```c
#include <stdio.h>
//IO操作需要包含的头文件
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

char filename[] = "text.txt";

int main(void)
{
	int fd;

	fd = open(filename, O_RDWR | O_CREAT, 0777);
	if (fd != -1) {
		printf("open %s ok\r\n",filename);
		if (close(fd) != -1) {
			printf("close %s ok\r\n",filename);
			
		} else {
			printf("close %s fail\r\n",filename);
		}
	} else {
		printf("open %s fail\r\n",filename);
	}

	return 0;
}
```

**常用flags有:**

以下三个常数中必须指定一个，且仅允许指定一个。

- `O_RDONLY` 只读打开
- `O_WRONLY` 只写打开
- `O_RDWR` 可读可写打开

以下可选项可以同时指定0个或多个，和必选项按位或起来作为`flags`参数。可选项有很多，这里只介绍一部分，其它选项可参考`open(2)`的Man Page：

- `O_APPEND` 表示追加。如果文件已有内容，这次打开文件所写的数据附加到文件的末尾而不覆盖原来的内容。
- `O_CREAT` 若此文件不存在则创建它。使用此选项时需要提供第三个参数`mode`，表示该文件的访问权限。
- `O_EXCL` 如果同时指定了`O_CREAT`，并且文件已存在，则出错返回。
- `O_TRUNC` 如果文件已存在，并且以只写或可读可写方式打开，则将其长度截断（Truncate）为0字节。
- `O_NONBLOCK` 对于设备文件，以`O_NONBLOCK`方式打开可以做非阻塞I/O（Nonblock I/O）

### 2.read与write

```c
#include <stdio.h>
#include <stdlib.h>
//IO操作需要包含的头文件
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

char filename[] = "./text.txt";

int main(void)
{
	int fd;
	char buf[10];
	int n;
	int i = 0;
	int len;
	
	//如果文件不存在就创建文件
	fd = open(filename, O_RDWR | O_CREAT, 0777);
	//fd = open(filename, O_RDONLY | O_WRONLY);
	if(fd == -1) {
		printf("open %s fail\r\n",filename);
		exit(1);
	} else {
		printf("open %s ok\r\n", filename);
	}
	
	n = write(fd, "1234567890", 10);
	if(n == -1) {
		printf("write %s fail\r\n",filename);
	} else {
		printf("write %s %d buff\r\n",filename, n);
	}
	
	//由于write操作会使读写位置处于内容末尾，需要指向到当前位置的前10偏移位置
	lseek(fd, -10, SEEK_CUR);
	
	n = read(fd, buf, 10);
	if(n == -1) {
		printf("read %s fail\r\n",filename);
	} else {
		printf("read %s %d buff\r\n",filename, n);
	}

	printf("read buf len:%d\r\n", n);
	//write(STDOUT_FILENO, buf, 10);
	for (i = 0; i < 10; i++)
	{
		printf("%c",buf[i]);
	}
	
	if(close(fd) == -1) {
		printf("close %s fail\r\n",filename);
		exit(1);
	} else {
		printf("close %s ok\r\n", filename);
	}

	return 0;
}
```

### 3.mmap

`mmap`可以把磁盘文件的一部分直接映射到内存，这样文件中的位置直接就有对应的内存地址，对文件的读写可以直接用指针来做而不需要`read`/`write`函数。

![](http://akaedu.github.io/book/images/io.mmap.png)

```c
#include <stdio.h>
#include <stdlib.h>
//内存映射需要的头文件
#include <sys/mman.h>
//文件操作需要的头文件
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main(void)
{
	int *p;
	int fd;
    
    //判断文件是否存在
    if (access("text.txt",F_OK) == -1) {
        perror("access text.txt");
		exit(1);
    }

	fd	= open("text.txt", O_RDWR);
	if (fd == -1) {
		perror("open text.txt");
		exit(1);
	}
	
	//映射之前文件内容不能为空，否则映射出错
	//mmap没法增加文件长度，通过内存对文件操作的数据长度不会超过文件本身包含的数据长度的
	//length==6，代表将文件中6word的数据映射到内存，但如果length小于一个内存页的长度，
	//测试发现实际设置共享内存的大小是一页。
	p = mmap(NULL, 6, PROT_WRITE, MAP_SHARED, fd, 0);
	if (p == MAP_FAILED) {
		perror("mmap");
		exit(1);
	}
	
	if (close(fd) == -1)
	{
		perror("close text.txt");
		exit(1);
	}
	
	p[0] = 0x31303030;	//低位在前，高位在后 X86架构下
	p[1] = 0x32303030;
	p[2] = 0x33303030;
	p[3] = 0x34303030;
	p[4] = 0x35303030;
	p[5] = 0x36303030;

    //取消映射
	if (munmap(p, 6) == -1) {
		perror("munmap");
		exit(1);
	}
	
	return 0;
}
```

`prot`参数有四种取值：

- PROT_EXEC表示映射的这一段可执行，例如映射共享库
- PROT_READ表示映射的这一段可读
- PROT_WRITE表示映射的这一段可写
- PROT_NONE表示映射的这一段不可访问

flag`参数有很多种取值，这里只讲两种，其它取值可查看`mmap(2)

- MAP_SHARED多个进程对同一个文件的映射是共享的，一个进程对映射的内存做了修改，另一个进程也会看到这种变化。
- MAP_PRIVATE多个进程对同一个文件的映射不是共享的，一个进程对映射的内存做了修改，另一个进程并不会看到这种变化，也不会真的写到文件中去。

### 4.实用工具

**1）access**

用于判断该文件是否存在或者是否可以读/写某一已存在的文件。

```c
#include<unistd.h>
int access(const char * pathname,int mode);
/*
pathname：文件路径
mode： 文件访问的权限
R_OK，W_OK与X_OK：检查文件是否具有读取、写入和执行的权限。
F_OK：判断该文件是否存在。

返回0值，表示成功，只要有一权限被禁止则返回-1。
*/
```

**2）lseek**

改变文件当前读写位置。打开文件时读写位置是0，表示文件开头，通常读写多少个字节就会将读写位置往后移多少个字节。

```c
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);

/*
fd 表示要操作的文件描述符
offset是相对于whence（基准）的偏移量
whence 可以是SEEK_SET（文件指针开始），SEEK_CUR（文件指针当前位置） ，SEEK_END为文件指针尾

返回值：文件读写指针距文件开头的字节大小，出错，返回-1
*/
```



## 二、Makefile编写

### 1.基本规则

```makefile
#规则格式
target ... : prerequisites ... 
	command1
	command2
	
#例如
main: main.o stack.o maze.o
	gcc main.o stack.o maze.o -o main
```

- `main`是规则的目标（Target），`main.o`、`stack.o`和`maze.o`是规则的条件（Prerequisite），command是规则的命令列表。
- 目标和条件之间的关系是：`欲更新目标，必须首先更新它的所有条件；所有条件中只要有一个条件被更新了，目标也必须随之被更新。`
- 如果一个目标拆开写多条规则，则只有一条规则允许有命令列表，其它规则不能有命令列表，否则`make`会报警告并且采用最后一条规则的命令列表。
- “更新”就是执行一遍规则中的命令列表（commands），命令列表中的每条命令必须以一个Tab开头。

**make执行步骤**

1. 尝试更新Makefile中第一条规则的目标`main`，第一条规则的目标称为缺省目标，只要缺省目标更新了就算完成任务了，其它工作都是为这个目的而做的。由于是第一次编译，`main`文件还没生成，显然需要更新，但规则说必须先更新了`main.o`、`stack.o`和`maze.o`这三个条件，然后才能更新`main`。

2. 所以`make`会进一步查找以这三个条件为目标的规则，这些目标文件也没有生成，也需要更新，所以执行相应的命令（`gcc -c main.c`、`gcc -c stack.c`和`gcc -c maze.c`，通过隐含规则）更新它们。

3. 最后执行`gcc main.o stack.o maze.o -o main`更新`main`。

   ![Makefile的依赖关系图](http://akaedu.github.io/book/images/make.graph.png)

### 2.隐含规则与模式规则

- 如果一个目标在Makefile中的所有规则都没有命令列表，`make`会尝试在内建的隐含规则（Implicit Rule）数据库中查找适用的规则。

- `make`的隐含规则数据库可以用`make -p`命令打印，也是Makefile的格式，包括很多变量和规则。如：

  ```makefile
  #通过.c文件编译出.o文件的默认规则
  
  # default
  OUTPUT_OPTION = -o $@
  
  # default
  CC = cc
  
  # default
  COMPILE.c = $(CC) $(CFLAGS) $(CPPFLAGS) $(TARGET_ARCH) -c
  
  %.o: %.c
  #  commands to execute (built-in):
          $(COMPILE.c) $(OUTPUT_OPTION) $<
  ```

  `$@`的取值为规则中的目标，`$<`的取值为规则中的第一个条件。

  `%.o: %.c`是一种特殊的规则，称为模式规则（Pattern Rule）。

### 3.伪目标

伪目标是一个标签，这个目标只执行命令，不创建目标，能避免目标与工作目录下的实际文件名冲突。

```makefile
PHONY:标签
```

例如将 all 设置为伪目标后，尽管在当前目录下有同名的 all 文件，但是在终端输入 make 命令， all 的命令会被正确执行。

### 4.变量

```makefile
foo = $(bar) 
bar = Huh? 

all: 
	@echo $(foo)

#在调用变量是才会把变量$(bar) 展开
```

**1）自动变量（隐含规则变量）**

| 自动变量 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| $@       | 规则的目标文件名                                             |
| $<       | 规则的目标的第一个依赖文件名                                 |
| $^       | 规则的目标所对应的所有依赖文件的列表，以空格分隔             |
| $?       | 规则的目标所对应的依赖文件新于目标文件的文件列表，以空格分隔 |
| $(@D)    | 规则的目标文件的目录部分（如果目标在子目录中）               |
| $(@F)    | 规则的目标文件的文件名部分（如果目标在子目录中）             |

**2）预定义变量（隐含规则变量）**

| 预定义变量 | 含义                                                         |
| ---------- | ------------------------------------------------------------ |
| AR         | 归档维护程序，默认值为 ar                                    |
| AS         | 汇编程序，默认值为 as                                        |
| CC         | C 语言编译程序，默认值为 cc                                  |
| CPP        | C 语言预处理程序，默认值为 cpp                               |
| RM         | 文件删除程序，默认值为 rm -f                                 |
| ARFLAGS    | 传递给 AR 程序的标志，默认为 rv                              |
| ASFLAGS    | 传递给 AS 程序的标志，默认值无                               |
| CFLAGS     | 传递给 CC 程序的标志，默认值无。编译选项，例如`-O`、`-g`等   |
| CPPFLAGS   | 传递给 CPP 程序的标志，默认值无。预处理选项，例如`-D`、`-I`等 |
| LDFLAGS    | 传递给链接程序的标志，默认值无                               |

### 5.常用make命令行选项

**1）-C**

- `-C`选项可以切换到另一个目录执行那个目录下的Makefile。

- 一些规模较大的项目会把不同的模块或子系统的源代码放在不同的子目录中，然后在每个子目录下都写一个该目录的Makefile，然后在一个总的Makefile中用`make -C`命令执行每个子目录下的Makefile。如Linux内核源代码。

```makefile
ARCH ?=
CROSS_COMPILE ?= mipsel-openwrt-linux-gnu-
export

### general build targets

all:
	$(MAKE) all -e -C libloragw
	$(MAKE) all -e -C util_pkt_logger
	$(MAKE) all -e -C util_spi_stress
	$(MAKE) all -e -C util_tx_test
	$(MAKE) all -e -C util_lbt_test
	$(MAKE) all -e -C util_tx_continuous
	$(MAKE) all -e -C util_spectral_scan

clean:
	$(MAKE) clean -e -C libloragw
	$(MAKE) clean -e -C util_pkt_logger
	$(MAKE) clean -e -C util_spi_stress
	$(MAKE) clean -e -C util_tx_test
	$(MAKE) clean -e -C util_lbt_test
	$(MAKE) clean -e -C util_tx_continuous
	$(MAKE) clean -e -C util_spectral_scan

### EOF
```

### 6.实例

文件目录

```shell
$ tree
.
├── makefile
├── testadd
│   ├── add.c
│   └── add.h
├── test.c
├── test.h
└── testsub
    ├── sub.c
    └── sub.h
```

Makefile文件

```makefile
# 预定义变量 指令编译器和选项
CC = gcc			#C 语言编译程序
CFLAGS = -Wall -g	#传递给 CC 程序的标志
LDFLAGS = -L -lFOO	#传递给链接程序的标志 库文件链接

# 自定义变量，目标文件, 变量引用为$(变量)
TARGET = test			
SRC = test.c \
	./testadd/add.c \
	./testsub/sub.c

#头文件路径
INC = -I ./ -I ./testadd -I ./testsub


# makefile规则，第一个目标文件为make默认的执行目标
all:test.c ./testadd/add.c ./testsub/sub.c
	gcc -I ./ -I ./testadd -I ./testsub -o test test.c ./testadd/add.c ./testsub/sub.c
	
exe1:$(SRC)			#输入make exe1执行特定的目标规则
	$(CC) $(LDFLAGS) $(INC) -o $(TARGET) $(SRC)
	
$(TARGET):$(SRC)
	$(CC) $(LDFLAGS) $(INC) -o $@ $^		# $@目标文件，$^包含所有的依赖文件

.PHONY:clean		#将目标文件clean设置为伪目标，该目标的规则被执行后不产生文件
clean:				#makefile规则
	rm -f $(TARGET) *.o	#*.o是指任意.o文件
```



## 三、进程使用

### 1.fork产生子进程

![](http://akaedu.github.io/book/images/process.fork.png)

```c
    
#include <stdio.h>
#include <stdlib.h>
//进程需要使用的头文件
#include <sys/types.h>
#include <unistd.h>
//waitpid函数需要的头文件
#include <sys/types.h>
#include <sys/wait.h>

int main(void)
{
	pid_t pid;
	char *message;
	int n = 1;
	int count = 0;
	
	pid = fork();
	if (pid < 0) {
		perror("fork failed");
		exit(1);
	}
	
	//fork成功以后，子进程和父进程都会执行下面的代码
	//后续变量都是独立的，操作互不影响
	if (pid == 0) {
		//fork函数对子进程返回0
		printf("child id:%d, parent id:%d\r\n",getpid(), getppid());
		message = "This is the child";
		n = 6;
	} else {
		//fork函数对父进程返回子进程id
		printf("parent id:%d, child id:%d\r\n",getpid(), pid);
		message = "This is the parent";
		n = 3;
	}
	
	for (; n > 0; n--) {
		count++;
		printf("%s,count:%d\r\n",message, count);
		sleep(1);
	}
	
	if (pid > 0) {
		int stat_val;
		waitpid(pid, &stat_val, 0);
		//如果子进程是正常终止的，WIFEXITED取出的字段值非零
		if (WIFEXITED(stat_val)) {
			//WEXITSTATUS取出的字段值就是子进程的退出状态
			printf("Child exited with code %d\n", WEXITSTATUS(stat_val));
		} else if (WIFSIGNALED(stat_val)) {
			//如果子进程是收到信号而异常终止的，WIFSIGNALED取出的字段值非零
			//WTERMSIG取出的字段值就是信号的编号
			printf("Child terminated abnormally, signal %d\n", WTERMSIG(stat_val));
		}
	}
	
	return 0;
}
```

### 2.exec族函数

当进程调用一种`exec`函数时，该进程的用户空间代码和数据完全被新程序替换，从新程序的启动例程开始执行。调用`exec`并不创建新进程，所以调用`exec`前后该进程的id并未改变。

```c
#include <unistd.h>

int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ..., char *const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execve(const char *path, char *const argv[], char *const envp[]);
```

- 不带字母p（表示path）的`exec`函数第一个参数必须是程序的相对路径或绝对路径
- 带有字母l（表示list）的`exec`函数要求将新程序的每个命令行参数都当作一个参数传给它，命令行参数的个数是可变的，最后一个可变参数是`NULL`
- 对于带有字母v（表示vector）的函数，应该先构造一个指向各参数的指针数组，然后将该数组的首地址当作参数传给它，数组中的最后一个指针也应该是`NULL`
- 对于以e（表示environment）结尾的`exec`函数，可以把一份新的环境变量表传给它

```c
#include <stdio.h>
#include <stdlib.h>

#include <unistd.h>

//命令行参数
char *const ps_argv[] = {"ps", "-o", "pid,ppid,pgrp,session,tpgid,comm", NULL};
//环境变量
char *const ps_envp[] = {"PATH=/bin:/usr/bin", "TERM=console", NULL};

int main(void)
{
	//ps是linux自带的应用程序
	//"ps", "-o", "pid,ppid,pgrp,session,tpgid,comm", NULL 都是命令行参数
	//这相当于在bash命令行输入：ps -o pid,ppid,pgrp,session,tpgid,comm
	
    //绝对路径，通过列表表示命令行参数
	//execl("/bin/ps", "ps", "-o", "pid,ppid,pgrp,session,tpgid,comm", NULL);
	
    //自动搜索路径，通过列表表示命令行参数
	//execlp("ps", "ps", "-o", "pid,ppid,pgrp,session,tpgid,comm", NULL);
	
    //绝对路径，通过列表表示命令行参数，传入环境变量
	//execle("/bin/ps", "ps", "-o", "pid,ppid,pgrp,session,tpgid,comm", NULL, ps_envp);
	
    //自动搜索路径，通过列表表示命令行参数，传入环境变量
	//这个函数在ubuntu中不存在
	//execlpe("ps", "ps", "-o", "pid,ppid,pgrp,session,tpgid,comm", NULL, ps_envp);
	
    //绝对路径，通过指针数组表示命令行参数
	//execv("/bin/ps", ps_argv);
	
    //自动搜索路径，通过指针数组表示命令行参数
	//execvp("ps", ps_argv);
	
    //自动搜索路径，通过指针数组表示命令行参数，传入环境变量
	execve("/bin/ps", ps_argv, ps_envp);
	
	//由于exec函数只有错误返回值，只要返回了一定是出错
	//所以不需要判断它的返回值，直接在后面调用perror即可。
	perror("exec ps");
	exit(1);
	
	return 0;
}
```

### 3.wait和waitpid函数

父进程可以调用`wait`或`waitpid`等待子进程终止，然后彻底清除掉这个进程。

```c
#include <sys/types.h>
#include <sys/wait.h>

pid_t wait(int *status);
pid_t waitpid(pid_t pid, int *status, int options);
/*
wait等待第一个终止的子进程，而waitpid可以通过pid参数指定等待哪一个子进程
若调用成功则返回清理掉的子进程id，若调用出错则返回-1。
*/
```

### 4.进程间通信

每个进程各自有不同的用户地址空间，任何一个进程的全局变量在另一个进程中都看不到。进程之间要交换数据`必须通过内核`，在内核中开辟一块缓冲区，进程1把数据从用户空间拷到内核缓冲区，进程2再从内核缓冲区把数据读走，内核提供的这种机制称为进程间通信。

![](http://akaedu.github.io/book/images/process.ipc.png)

#### 1）通过匿名管道

```c
#include <unistd.h>

int pipe(int filedes[2]);
```

- 调用`pipe`函数时在内核中开辟一块缓冲区（称为管道）用于通信，它有一个读端一个写端，然后通过`filedes`参数传出给用户程序两个文件描述符，`filedes[0]`指向管道的读端，`filedes[1]`指向管道的写端（就像0是标准输入1是标准输出一样）。
- 仅可用于父进程与子进程之间通信

![](http://akaedu.github.io/book/images/process.pipe.png)

```c
#include <stdio.h>
#include <stdlib.h>
//进程需要使用的头文件
#include <sys/types.h>
#include <unistd.h>
//waitpid函数需要的头文件
#include <sys/types.h>
#include <sys/wait.h>

#define MAXLINE 80

int main(void)
{
	int n;
	int fd[2], fd2[2];
	pid_t pid;
	char line[MAXLINE];
	
    //创建管道1，用于父线程写、子线程读
	if (pipe(fd) < 0) {
		perror("pipe fd");
		exit(1);
	}
	
    //创建管道2，用于父线程读、子线程写
	if (pipe(fd2) < 0) {
		perror("pipe fd2");
		exit(1);
	}
	
	if ((pid = fork()) < 0) {
		perror("fork");
		exit(1);
	}
	
	if (pid == 0) {
		//child
		close(fd[1]); //关闭写端
		n = read(fd[0], line, MAXLINE);
		write(STDOUT_FILENO, "child pipe read:", 16);
		write(STDOUT_FILENO, line, n);
		
		close(fd2[0]); //关闭读端
		write(STDOUT_FILENO, "child pipe write\n", 17);
		write(fd2[1], "hello parent\r\n", 14);
		
	} else {
		//parent
		close(fd[0]); //关闭读端
		write(STDOUT_FILENO, "parent pipe write\n", 18);
		write(fd[1], "hello child\r\n", 13);
		
		close(fd2[1]); //关闭写端
		n = read(fd2[0], line, MAXLINE);
		write(STDOUT_FILENO, "parent pipe read:", 17);
		write(STDOUT_FILENO, line, n);
		
		wait(NULL);
	}
	
	return 0;
}
```

#### 2）通过FIFO文件

- 在文件系统中用`mkfifo`命令创建一个FIFO文件，FIFO文件在磁盘上没有数据块，仅用来标识内核中的一条通道，各进程可以打开这个文件进行`read`/`write`，实际上是在读写内核通道，从而实现进程间通信。
- 能让所有进程相互通信

```c
#include <stdio.h>
#include <stdlib.h>

//进程需要使用的头文件
#include <sys/types.h>
#include <unistd.h>
//wait函数需要的头文件
#include <sys/types.h>
#include <sys/wait.h>
//命名管道需要用的头文件
#include <sys/stat.h>
//文件操作需要的头文件
#include <fcntl.h>

char fifoname[] = {"./pipefifo"};

int main(void)
{
	int fd;
	pid_t pid;
	char buf[20];
	int n;
	
	if (access(fifoname, F_OK) < 0) {
		//FIFO和socket都是特殊文件
		//FIFO也只支持单向传输，双向传输需要使用两个管道
		if (mkfifo(fifoname, 0777) < 0 ) {
			perror("mkfifo");
			exit(1);
		}
		
		//创建普通文件也可以达到进程间通信的效果，就是对文件进行读写
		//使用普通文件传输会有数据残留
		/*fd = creat(fifoname, 0777);
		if (fd < 0) {
			perror("creat");
			exit(1);
		}
		close(fd);*/
	}
	
	if((pid = fork()) < 0) {
		perror("fork");
		exit(1);
	}
	
	if (pid == 0) {
		//child
		fd = open(fifoname, O_RDWR);
		if (fd < 0) {
			perror("fork");
			exit(1);
		}
		
		n = read(fd, buf, 20);
		write(STDOUT_FILENO, "child get:", 10);
		write(STDOUT_FILENO, buf, n);
	} else {
		//parent
		fd = open(fifoname, O_RDWR);
		if (fd < 0) {
			perror("fork");
			wait(NULL);
			exit(1);
		}
		write(STDOUT_FILENO, "parent:hello child\r\n", 20);
		write(fd, "hello child\r\n", 13);
		
		wait(NULL);
	}
	return 0;
}
```

普通文件是不能用于进程间通信的，因为不是通过内核进行数据交互。

#### 3）通过mmap函数映射内存区

参考IO操作说明，几个进程可以映射同一内存区。

#### 4）Socket

目前最广泛使用的IPC（进程间通信）机制，与FIFO一样是利用文件系统中的特殊文件socket来标识内核中的通道。

#### 5）信号

进程之间互发信号，一般使用`SIGUSR1`和`SIGUSR2`实现用户自定义功能。

#### 6）消息队列、信号量和共享内存

以前的SYS V UNIX系统实现的IPC机制，现在已经基本废弃。

### 5.守护进程

守护进程（Daemon）没有控制终端，不需直接和用户交互，不受用户登录注销的影响，一直在后台运行着。

```c
#include <stdio.h>
#include <stdlib.h>
//进程需要使用的头文件
#include <sys/types.h>
#include <unistd.h>

//file操作需要的头文件
#include <sys/stat.h>
#include <fcntl.h>

int main(void)
{
	pid_t pid;
	
	if ((pid = fork()) < 0) {
		perror("fork");
		exit(1);
	}
	
	if (pid > 0) {
		//终结父进程
		exit(0);
	}
	
	//子进程调用setsid创建新的Session，成为守护进程
	setsid();
	
	//按照守护进程的惯例，通常将当前工作目录切换到根目录
	if (chdir("/") < 0) {
		perror("chdir");
		exit(1);
	}
	
	//将标准输入0、标准输出1、标准错误2指向/dev/null
	close(0);
	open("/dev/null", O_RDWR);
	dup2(0, 1);
	dup2(0, 2);
	
	while (1) {
		//执行守护进程的操作
		sleep(1);
		;
	}
	
	return 0;
}
```



## 四、信号

### 1.产生信号

1）通过kill函数

- `kill`函数可以给一个指定的进程发送指定的信号。

- `raise`函数可以给当前进程发送指定的信号（自己给自己发信号）

```c
#include <signal.h>

int kill(pid_t pid, int signo);
int raise(int signo);

/*
成功返回0，错误返回-1。
*/
```

2）通过SIGPIPE函数

```c
#include <unistd.h>

unsigned int alarm(unsigned int seconds);
/*
告诉内核在seconds秒之后给当前进程发SIGALRM信号。
*/
```

注意：信号产生函数不能放在信号处理函数中

### 2.阻塞信号

信号从产生到递达之间的状态，称为信号*未决*（Pending）。进程可以选择阻塞（Block）某个信号。被阻塞的信号产生时将保持在未决状态，直到进程解除对此信号的阻塞，才执行递达的动作。

1）信号集操作函数

修改信号集变量

```c
#include <signal.h>

int sigemptyset(sigset_t *set);//初始化set所指向的信号集，使其中所有信号的对应bit清零，表示该信号集不包含任何有效信号。
int sigfillset(sigset_t *set);//初始化set所指向的信号集，使其中所有信号的对应bit置位，表示该信号集的有效信号包括系统支持的所有信号。
int sigaddset(sigset_t *set, int signo);//添加某种有效信号
int sigdelset(sigset_t *set, int signo);//删除某种有效信号
int sigismember(const sigset_t *set, int signo);//判断一个信号集的有效信号中是否包含某种信号，若包含则返回1，不包含则返回0，出错返回-1。

/*
成功返回0，出错返回-1
*/
```

2）修改进程信号屏蔽字

真正开始屏蔽信号

```c
#include <signal.h>

int sigprocmask(int how, const sigset_t *set, sigset_t *oset);//读取或更改进程的信号屏蔽字
/*
how参数：
SIG_BLOCK	set包含了我们希望添加到当前信号屏蔽字的信号，相当于mask=mask|set
SIG_UNBLOCK	set包含了我们希望从当前信号屏蔽字中解除阻塞的信号，相当于mask=mask&~set
SIG_SETMASK	设置当前信号屏蔽字为set所指向的值，相当于mask=set

先将原来的信号屏蔽字备份到oset里，然后根据set和how参数更改信号屏蔽字

若成功则为0，若出错则为-1
*/

```

3）检测当前进程信号集

通过sigismember函数可以判断出当前信号集是否包含未决信号

```c
#include <signal.h>

int sigpending(sigset_t *set);//读取当前进程的未决信号集，通过set参数传出
/*
调用成功则返回0，出错则返回-1。
*/
```

4）实例

设置进程对SIGINT信号挂起

```c
#include <stdio.h>
#include <stdlib.h>
//信号需要的头文件
#include <unistd.h>
#include <signal.h>

int main(void)
{
	int i = 0;
	//定义信号集
	sigset_t s,p;
	
	//定义信号集
	sigemptyset(&s);
	sigaddset(&s, SIGINT); //在信号集中只将SIGINT置位
	//在进程信号屏蔽字中添加SIGINT，即该进程将会使收到的SIGINT信号pending
	sigprocmask(SIG_BLOCK, &s, NULL);
	
	while (1) {
        //读取当前进程的未决信号集
		sigpending(&p);
		
		for (i = 0; i < 32; i++) {
			//如果进程中有被pending的信号
			if (sigismember(&p, i) == 1) {
				printf("signal:%d\n",i);
			}
		}
		sleep(1);
	}
	return 0;
}
```

### 3.捕捉信号

如果信号的处理动作是用户自定义函数，在信号递达时就调用这个函数，这称为捕捉信号。

1）重定义信号处理函数

```c
#include <signal.h>

int sigaction(int signo, const struct sigaction *act, struct sigaction *oact);
```

2）挂起进程等待信号

```c
#include <unistd.h>

int pause(void);
/*
如果信号的处理动作是捕捉，则调用了信号处理函数之后pause返回-1，errno设置为EINTR，所以pause只有出错的返回值
*/
```

3）解除信号屏蔽并挂起等待信号

```c
#include <signal.h>

int sigsuspend(const sigset_t *sigmask);
/*
进程的信号屏蔽字由sigmask参数指定，可以通过指定sigmask来临时解除对某个信号的屏蔽，然后挂起等待，当sigsuspend返回时，进程的信号屏蔽字恢复为原来的值。

sigsuspend没有成功返回值，只有执行了一个信号处理函数之后sigsuspend才返回，返回值为-1，errno设置为EINTR。
*/
```

4）实例1 

使用pause挂起等待

```c
#include <stdio.h>
#include <stdlib.h>
//signal需要的头文件
#include <unistd.h>
#include <signal.h>

//改变SIGALRM信号的处理函数
//alarm 触发SIGALRM信号，执行sigalrm_handler函数

void sigalrm_handler(int sig)
{
	printf("Get a signal %d\n", sig);
}

int main(void)
{
	struct sigaction actnew, actold;
	
	//重新定义信号处理函数
	actnew.sa_handler = sigalrm_handler;
	actnew.sa_flags = 0;
	sigemptyset(&actnew.sa_mask); //信号处理函数执行期间 不屏蔽信号
	sigaction(SIGALRM, &actnew, &actold); //actold保存原来的处理动作
	
	while (1) {
		alarm(2);
		//bug：如果在alarm与pause之间出现更高级的中断或进程任务，在SIGALRM信号出现后还没恢复，则会导致pluse函数一直挂起程序
		
		//使进程挂起直到有信号抵达
		//信号抵达后先执行信号处理函数，再往下执行
		pause();
		printf("have a signal\n");
	}
	
	return 0;
}
```

5）实例2

使用sigsuspend挂起等待

```c
#include <stdio.h>
#include <stdlib.h>
//signal需要的头文件
#include <unistd.h>
#include <signal.h>

//改变SIGALRM信号的处理函数
//alarm 触发SIGALRM信号，执行sigalrm_handler函数
//sigsuspend保证进程一定可以监听到SIGALRM信号

void signal_handler(int sig)
{
	printf("get a signal %d\n", sig);
}

int main(void)
{
	struct sigaction act_new, act_old;
	sigset_t mask_new, mask_old;
	
	//重新定义信号处理函数
	act_new.sa_handler = signal_handler;
	act_new.sa_flags = 0;
	sigemptyset(&act_new.sa_mask);
	sigaction(SIGALRM, &act_new, &act_old);
	
	//进程先屏蔽SIGALRM信号
	sigemptyset(&mask_new);
	sigaddset(&mask_new, SIGALRM);
	sigprocmask(SIG_BLOCK, &mask_new, &mask_old);
	
	while (1) {
		alarm(2);
		
		//临时设置新的信号屏蔽(打开SIGALRM信号)，并挂起进程
		//直到有信号抵达恢复原来的信号屏蔽并往下执行
		sigsuspend(&mask_old);//将挂起pluse()函数和sigprocmask函数合并
		printf("have a signal\n");
	}
	
	return 0;
}
```

### 4.通过SIGCHLD信号处理子进程终结

- `wait`和`waitpid`函数清理僵尸进程，父进程需要阻塞等待子进程结束。

- 子进程在终止时会给父进程发`SIGCHLD`信号，该信号的默认处理动作是忽略，父进程可以自定义`SIGCHLD`信号的处理函数，当子进程终止时会通知父进程，父进程在信号处理函数中调用`wait`清理子进程即可。

```c
#include <stdio.h>
#include <stdlib.h>
//signal需要使用的头文件
#include <unistd.h>
#include <signal.h>
//进程需要使用的头文件
#include <sys/types.h>
//#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

//子进程终止时都会给父进程发送SIGCHLD信号
//重定义SIGCHLD信号处理函数，自动调用wait清理子进程，父进程不必阻塞

void signal_handler(int sig)
{
	printf("child exit\n");
	wait(NULL); //回收资源
}

int main(void)
{
	pid_t pid;
	
	if ((pid = fork()) < 0) {
		perror("fork");
		exit(1);
	}
		
	if (pid == 0) {
		//child
		sleep(1);
		printf("I am child\n");
		exit(2);
	} else {
		//parent
		struct sigaction act_new;
		
        //重新定义SIGCHLD信号处理函数
		act_new.sa_handler = signal_handler;
		act_new.sa_flags = 0;
		sigemptyset(&act_new.sa_mask);
		sigaction(SIGCHLD, &act_new, NULL);
		
		while (1) {
			printf("I am parent\n");
			sleep(1);
		}
		
	}
	return 0;
}
```



## 五、线程使用

各线程共享的进程资源和环境

- 进程同一地址空间
- 同一进程定义的函数和全局变量
- 文件描述符表
- 每种信号的处理方式（`SIG_IGN`、`SIG_DFL`或者自定义的信号处理函数）
- 当前工作目录
- 用户id和组id

线程各自独立的资源

- 线程id
- 上下文，包括各种寄存器的值、程序计数器和栈指针
- 栈空间
- `errno`变量
- 信号屏蔽字
- 调度优先级



### 1.创建线程

```c
#include <stdio.h>
#include <stdlib.h>
//线程所需要的头文件
#include <pthread.h>
//getpid需要的头文件
#include <unistd.h>

//线程编译需要加上-lpthread

int temp = 0;

void printids(char *s)
{
	pid_t pid;
	pthread_t tid;
	
	pid = getpid();
	tid = pthread_self();//获取当前线程id
	//由于pthread_t并不是一个整型，所以需要做强制类型转换
	printf("%s pid:%d, tid:%u\n", s, pid, (unsigned int)tid);
	
}

//线程处理函数
void  *thread_handler(void *arg)
{
	static int value = 0;
	
	temp++;	//线程间共享全局变量、局部变量、函数
	value++;
	printf("%s value:%d, temp:%d\n", (char*)arg, value, temp);
	
	printids(arg);
	return NULL;
}

int main(void)
{
	pthread_t tid;
	int err;
	
	/*
	 * 返回线程id
	 * 线程属性设置
	 * 线程处理函数
	 * 线程处理函数参数
	 */
	err = pthread_create(&tid, NULL, thread_handler, "new_thread1");
	//pthread_create失败返回错误码
	if (err != 0) {
		//由于pthread_create的错误码不保存在errno中，因此不能直接用perror()打印错误信息
		fprintf(stderr, "pthread_create1\n");
		exit(1);
	}
	printf("create tid %u\n", (unsigned int)tid);
	
	err = pthread_create(&tid, NULL, thread_handler, "new_thread2");
	//pthread_create失败返回错误码
	if (err != 0) {
		//由于pthread_create的错误码不保存在errno中，因此不能直接用perror()打印错误信息
		fprintf(stderr, "pthread_create2\n");
		exit(1);
	}
	printf("create tid %u\n", (unsigned int)tid);
	
	printids("main_thread");
	sleep(2);//预留线程调度的时间
	return 0;
}
```

### 2.终止线程

只终止某个线程可以有三种方法：

- 从线程函数`return`。这种方法对主线程不适用，从`main`函数`return`相当于调用`exit`。
- 一个线程可以调用`pthread_cancel`终止同一进程中的另一个线程。
- 线程可以调用`pthread_exit`终止自己。

1）终止某个线程

```c
#include <pthread.h>

void pthread_exit(void *value_ptr);
/*
成功返回0，失败返回错误号
*/
```

2）自身线程挂起等待某线程终止

```c
#include <pthread.h>

int pthread_join(pthread_t thread, void **value_ptr);
/*
成功返回0，失败返回错误号
*/
```

3）实例

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
//线程所需头文件
#include <pthread.h>

void *thread1_handler(void *arg)
{
	printf("%s running\n", (char*)arg);
	return (void *)1;	//return 终止线程返回return值
}

void *thread2_handler(void *arg)
{
	printf("%s running\n", (char*)arg);
	pthread_exit((void *)2); //pthread_exit终止线程返回其参数值
}

void *thread3_handler(void *arg)
{
	while (1) {
		printf("%s waiting\n", (char*)arg);
		sleep(1);
	}
	//线程被异常终止，返回常数PTHREAD_CANCELED，即-1。
}

int main(void)
{
	pthread_t tid1, tid2, tid3;
	void *ptr;
	int err;
	
	//thread 1
	err = pthread_create(&tid1, NULL, thread1_handler, "new thread1");
	if (err != 0) {
		fprintf(stderr, "pthread_create1\n");
		exit(1);
	}
	
	//阻塞等待thread 1终止  （主进程同时也是一个线程）
	err = pthread_join(tid1, &ptr);
	if (err != 0) {
		fprintf(stderr, "pthread_join\n");
		exit(1);
	}
	printf("thread1 exit code %d\n", (int)ptr);
	
	//thread 2
	err = pthread_create(&tid2, NULL, thread2_handler, "new thread2");
	if (err != 0) {
		fprintf(stderr, "pthread_create2\n");
		exit(1);
	}
	
	//阻塞等待thread 2终止
	err = pthread_join(tid2, &ptr);
	if (err != 0) {
		fprintf(stderr, "pthread_join\n");
		exit(1);
	}
	printf("thread2 exit code %d\n", (int)ptr);
	
	//thread 3
	err = pthread_create(&tid3, NULL, thread3_handler, "new thread3");
	if (err != 0) {
		fprintf(stderr, "pthread_create3\n");
		exit(1);
	}
	
	sleep(3);
	//主线程将thread 3异常终止
	pthread_cancel(tid3);
	//阻塞等待thread 3终止
	err = pthread_join(tid3, &ptr);
	if (err != 0) {
		fprintf(stderr, "pthread_join\n");
		exit(1);
	}
	printf("thread3 exit code %d\n", (int)ptr);
	
	return 0;
}
```

### 3.线程间同步

多个线程同时访问共享数据时可能会冲突，使用互斥量mutex或可以解决冲突问题。

#### 1）互斥量mutex

Mutex变量是非0即1的，可看作一种资源的可用数量，初始化时Mutex是1，表示有一个可用资源，加锁时获得该资源，将Mutex减到0，表示不再有可用资源，解锁时释放该资源，将Mutex重新加到1，表示又有了一个可用资源。

```c
#include <pthread.h>

int pthread_mutex_init(pthread_mutex_t *restrict mutex,const pthread_mutexattr_t *restrict attr);//动态初始化互斥量
int pthread_mutex_destroy(pthread_mutex_t *mutex);//销毁互斥量
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;//静态初始化互斥量
```

实例

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
//线程所需头文件
#include <pthread.h>

#define NLOOP	5000
int counter = 0;

//静态初始化互斥量（静态初始化要比动态初始化好）
pthread_mutex_t	counter_mutex = PTHREAD_MUTEX_INITIALIZER;

void *add(void *arg)
{
	int i = 0;
	int val = 0;
	
	for (i = 0; i < NLOOP; i++) {
		//阻塞获取互斥量
		pthread_mutex_lock(&counter_mutex);
		
		val = counter;
		//printf调用，它会执行write系统调用进内核，为内核调度别的线程执行提供了一个很好的时机
		printf("%s %x: val:%d\n", (char *)arg, (unsigned int)pthread_self(), val);
		counter++;
		
		//解除互斥量
		pthread_mutex_unlock(&counter_mutex);
		
	}
	return NULL;
}

int main(void)
{
	pthread_t tid1, tid2;
	int err;
	
	err = pthread_create(&tid1, NULL, add,"thread1");
	if (err != 0) {
		fprintf(stderr, "pthread_create\n");
		exit(1);
	}
	
	err = pthread_create(&tid2, NULL, add,"thread2");
	if (err != 0) {
		fprintf(stderr, "pthread_create\n");
		exit(1);
	}
	
	pthread_join(tid1, NULL);
	pthread_join(tid1, NULL);
	
	printf("\nEND counter:%d\n",counter);
}
```

#### 2）条件变量condition

如果两个线程在等待对方的资源，就会出现死锁现象。

通过条件变量（Condition Variable）来阻塞等待一个条件，或者唤醒等待这个条件的线程。

```c
#include <pthread.h>

int pthread_cond_init(pthread_cond_t *restrict cond,const pthread_condattr_t *restrict attr);//动态初始化一个条件变量
int pthread_cond_destroy(pthread_cond_t *cond);//销毁一个条件变量
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;//静态初始化一个条件变量
/*
成功返回0，失败返回错误号
*/
```

实例

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
//线程所需头文件
#include <pthread.h>

//程序演示一个生产者-消费者的例子，生产者生产一个结构体串在链表的表头上，消费者从表头取走结构体。

//定义链表
struct msg {
	struct msg *next;
	int value;
};

struct msg *head;

//静态初始化互斥量
pthread_mutex_t product_mutex = PTHREAD_MUTEX_INITIALIZER;

//静态初始化条件变量
pthread_cond_t	product_cond = PTHREAD_COND_INITIALIZER;	

void *consumer(void *arg)
{
	struct msg *mp;
	
	while (1) {
		//阻塞获取互斥量
		pthread_mutex_lock(&product_mutex);
		while (head == NULL) {
			//释放互斥量并阻塞线程，直到被唤醒时重新获取互斥量
			pthread_cond_wait(&product_cond, &product_mutex);
		}
		//修改全局变量
		mp = head;
		head = mp->next;
		//释放互斥量
		pthread_mutex_unlock(&product_mutex);
		
		printf("Consume %d\n", mp->value);
		//释放内存空间
		free(mp);
		sleep(rand() % 5);
	}
	return NULL;
}

void *producer(void *arg)
{
	struct msg *mp;
	
	while (1) {
		//申请内存空间
		mp = malloc(sizeof(struct msg));
		mp->value = rand() % 1000 + 1;
		printf("Produce %d\n", mp->value);
		
		//阻塞获取互斥量
		pthread_mutex_lock(&product_mutex);
		//修改全局变量
		mp->next = head;
		head = mp;
		//释放互斥量
		pthread_mutex_unlock(&product_mutex);
		
		//唤醒一个在等待条件变量的线程
		pthread_cond_signal(&product_cond);
		sleep(rand() % 5);
	}
	
	return NULL;
}

int main(void)
{
	pthread_t tid_p, tid_c;
	int err;
	
	//设置随机数种子
	srand(time(NULL));
	
	err = pthread_create(&tid_p, NULL, producer, "thread producer");
	if (err != 0) {
		fprintf(stderr, "pthread_create\n");
		exit(1);
	}
	
	err = pthread_create(&tid_p, NULL, consumer, "thread consumer");
	if (err != 0) {
		fprintf(stderr, "pthread_create\n");
		exit(1);
	}
	
	pthread_join(tid_p, NULL);
	pthread_join(tid_c, NULL);
	
	return 0;
}
```

#### 3）信号量Semaphore

信号量（Semaphore）和Mutex类似，表示可用资源的数量，和Mutex不同的是这个数量可以大于1。

```c
#include <semaphore.h>

int sem_init(sem_t *sem, int pshared, unsigned int value);//初始化一个semaphore变量，value参数表示可用资源的数量，pshared参数为0表示信号量用于同一进程的线程间同步
int sem_wait(sem_t *sem);//获得资源，使semaphore的值减1，如果semaphore的值已经是0，则挂起等待
int sem_trywait(sem_t *sem);//获得资源，使semaphore的值减1，如果semaphore的值已经是0，不会挂起等待
int sem_post(sem_t * sem);//释放资源，使semaphore的值加1，同时唤醒挂起等待的线程。
int sem_destroy(sem_t * sem);//销毁信号量
```

实例

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
//线程所需头文件
#include <pthread.h>
//信号量所需头文件
#include <semaphore.h>

//生产者－消费者的例子是基于链表的，其空间可以动态分配，
//现在基于固定大小的环形队列重写这个程序

#define NUM		5
int queue[NUM];
sem_t blank_number, product_number;

void *producer(void *arg)
{
	int  p = 0;
	
	while (1) {
		//等待信号量
		sem_wait(&blank_number);
		queue[p] = rand() % 1000 + 1;
		printf("Produce %d\n", queue[p]);
		//释放信号量
		sem_post(&product_number);
		p = (p+1)%NUM;
		sleep(rand()%5);
	}
}

void *consumer(void *arg)
{
	int c = 0;
	
	while(1) {
		sem_wait(&product_number);
		printf("Comsume %d\n", queue[c]);
		queue[c] = 0;
		sem_post(&blank_number);
		c = (c+1)%NUM;
		sleep(rand()%5);
	}
}

int main(void)
{
	pthread_t tid1, tid2;
	int err;
	
	//初始化信号量,0代表信号量用于线程间同步，NUM代表信号量可用资源数量
	sem_init(&blank_number, 0, NUM);
	sem_init(&product_number, 0, 0);
	//创建线程
	err = pthread_create(&tid1, NULL, producer, NULL);
	if (err != 0) {
		fprintf(stderr, "pthread_create\n");
		exit(1);
	}
	err = pthread_create(&tid2, NULL, consumer, NULL);
	if (err != 0) {
		fprintf(stderr, "pthread_create\n");
		exit(1);
	}
	//等待线程退出
	pthread_join(tid1, NULL);
	pthread_join(tid2, NULL);
	
	//销毁信号量
	sem_destroy(&blank_number);
	sem_destroy(&product_number);
	
	return 0;
}
```



## 六、网络通信



### 1.TCP通信

#### 1）TCP进程服务器

```c
/* server */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
//socket 所需的头文件
#include <sys/socket.h>
#include <netinet/in.h>

#include <arpa/inet.h>
#include <ctype.h>

//进程需要使用的头文件
#include <sys/types.h>
#include <unistd.h>
//waitpid函数需要的头文件
#include <sys/types.h>
#include <sys/wait.h>

#define MAXLINE	80
#define SERV_PORT	8000


int main(void)
{
	struct sockaddr_in servaddr, cliaddr;	//socket 地址结构体 服务端和客户端地址
	socklen_t cliaddr_len;
	int listenfd, connfd;//文件描述符
	char buf[MAXLINE];
	char str[INET_ADDRSTRLEN];
	int i,n;
	int status;
	
	pid_t pid;
	struct sigaction act_new;
	
	//创建流式套接字，即TCP socket
	listenfd = socket(AF_INET, SOCK_STREAM, 0);
	if (listenfd == -1) {
		fprintf(stderr, "create socket\n");
		exit(1);
	}
	
	//初始化socket地址
	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family = AF_INET;//地址类型
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);//可监听任意ip地址
	servaddr.sin_port = htons(SERV_PORT);//可监听的网络端口
	//绑定地址与端口到套接字
	status = bind(listenfd, (struct sockaddr *)&servaddr, sizeof(servaddr) );
	if (status == -1) {
		fprintf(stderr, "bind\n");
		exit(1);
	}
	
	//设置socket为监听模式，最多允许有20个客户端处于连接待状态
	listen(listenfd, 20);
	if (status == -1) {
		fprintf(stderr, "listen\n");
		exit(1);
	}
	
	printf("Accepting connections ...\n");
	
	while (1) {
		cliaddr_len = sizeof(cliaddr);
		//服务端阻塞等待客户端连接
		connfd = accept(listenfd, (struct sockaddr *)&cliaddr, &cliaddr_len);
		
		//当有新的客户端连接时，fork出一个新进程来管理接收
		pid = fork();
		if (pid == -1) {
			perror("call to fork");
			exit(1);
		} else if (pid == 0) {
			//child
			close(listenfd);
			
			printf("connect to %s at PORT %d\n", inet_ntop(AF_INET, &cliaddr.sin_addr, str, sizeof(str)), ntohs(cliaddr.sin_port));
			
			while (1) {
				//通过文件描述符读取数据  阻塞等待
				n = read(connfd, buf, MAXLINE);
				if (n == 0) {
					printf("disconnect to %s at PORT %d\n", inet_ntop(AF_INET, &cliaddr.sin_addr, str, sizeof(str)), ntohs(cliaddr.sin_port));
					exit(0);
				}
				//客户端的端口是随机分配的
				printf("received from %s at PORT %d\n", inet_ntop(AF_INET, &cliaddr.sin_addr, str, sizeof(str)), ntohs(cliaddr.sin_port));
				
				for (i = 0; i < n; i++) {
					buf[i] = toupper(buf[i]);//将小写变大写
				}
				//通过文件描述符写入数据
				write(connfd, buf, n);
			}
			close(connfd);
			
		} else {
			//parent
			
			close(connfd);
		}

	}
	
	close(listenfd);
}
```

#### 2）TCP客户端

```c
/* client */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
//socket 所需的头文件
#include <sys/socket.h>
#include <netinet/in.h>

#include <arpa/inet.h>
#include <ctype.h>

#define MAXLINE	80
#define SERV_PORT	8000

//将client改为交互式输入

int main(int argc, char *argv[])
{
	struct sockaddr_in servaddr;
	char buf[MAXLINE];
	int sockfd; //文件描述符
	int n;
	
	//创建流式套接字，即TCP socket
	sockfd = socket(AF_INET, SOCK_STREAM, 0);
	if (sockfd == -1) {
		fprintf(stderr, "create socket\n");
		exit(1);
	}
	
	//初始化socket地址
	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family = AF_INET;
	inet_pton(AF_INET, "127.0.0.1", &servaddr.sin_addr);
	servaddr.sin_port = htons(SERV_PORT);
	
	//客户端请求连接服务器
	connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr));
	
	while (1) {
		if (fgets(buf, MAXLINE, stdin) != NULL) {
			//通过文件描述符写入数据 
			write(sockfd, buf, strlen(buf));
			
			//通过文件描述符读取数据  阻塞等待
			n = read(sockfd, buf, MAXLINE);
			if (n == 0) {
				printf("the other side has been closed.\n");
			} else {
				printf("Response from server:\n");
				write(STDOUT_FILENO, buf, n);
			}
		}
	}

	close(sockfd);
	
	return 0;
}
```



### 2.UDP通信

#### 1）UDP服务器

```c
/* server */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
//socket 所需的头文件
#include <sys/socket.h>
#include <netinet/in.h>

#include <arpa/inet.h>
#include <ctype.h>

#define MAXLINE 80
#define SERV_PORT 8000

int main(void)
{
	struct sockaddr_in servaddr, cliaddr;	//socket 地址结构体 服务端和客户端地址
	socklen_t cliaddr_len;
	int sockfd;//文件描述符
	char buf[MAXLINE];
	char str[INET_ADDRSTRLEN];
	int i,n;
	int status;
	
	//创建块式套接字，即UDP socket
	sockfd = socket(AF_INET, SOCK_DGRAM, 0);
	if (sockfd == -1) {
		fprintf(stderr, "create socket\n");
		exit(1);
	}
	
	//初始化socket地址
	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family = AF_INET;//地址类型
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);//可监听任意ip地址
	servaddr.sin_port = htons(SERV_PORT);//可监听的网络端口
	//绑定地址与端口到套接字
	status = bind(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr) );
	if (status == -1) {
		fprintf(stderr, "bind\n");
		exit(1);
	}
	
	printf("Accepting connections ...\n");
	
	while (1) {
		cliaddr_len = sizeof(cliaddr);
		//阻塞等待客户端数据到来
		n = recvfrom(sockfd, buf, MAXLINE, 0, (struct sockaddr *)&cliaddr, &cliaddr_len);
		if (n == -1) {
			fprintf(stderr, "recvfrom error\n");
			exit(1);
		}
		printf("received from %s at PORT %d\n", inet_ntop(AF_INET, &cliaddr.sin_addr, str, sizeof(str)), ntohs(cliaddr.sin_port));
			
		for (i = 0; i < n; i++) {
			buf[i] = toupper(buf[i]);//小写字母转大写
		}
		
		n = sendto(sockfd, buf, n, 0, (struct sockaddr *)&cliaddr, sizeof(cliaddr));
		if (n == -1) {
			fprintf(stderr, "sendto error\n");
			exit(1);
		}
	}
	
}
```

#### 2）UDP客户端

```c
/* client */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
//socket 所需的头文件
#include <sys/socket.h>
#include <netinet/in.h>

#include <arpa/inet.h>
#include <ctype.h>

#define MAXLINE 80
#define SERV_PORT 8000

int main(int argc, char *argv[])
{
	struct sockaddr_in servaddr;
	int sockfd, n;
	char buf[MAXLINE];
	char str[INET_ADDRSTRLEN];
	socklen_t servaddr_len;
	
	//创建UDP socket
	sockfd = socket(AF_INET, SOCK_DGRAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family = AF_INET;
	inet_pton(AF_INET, "127.0.0.1", &servaddr.sin_addr);
	servaddr.sin_port = htons(SERV_PORT);
	
	while (1) {
		if (fgets(buf, MAXLINE, stdin) != NULL) {
			//通过文件描述符发送数据
			n = sendto(sockfd, buf, strlen(buf), 0, (struct sockaddr *)&servaddr, sizeof(servaddr));
			if (n == -1) {
				fprintf(stderr, "sendto error\n");
				exit(1);
			}
			
			//通过文件描述符接收数据
			n = recvfrom(sockfd, buf, MAXLINE, 0, NULL, 0);
			if (n == -1) {
				fprintf(stderr, "recvfrom error\n");
				exit(1);
			}
			
			write(STDOUT_FILENO, buf, n);

		}
	}
	close(sockfd);
	
	return 0;
}
```

































