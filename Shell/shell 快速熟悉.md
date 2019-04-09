# shell快速熟悉

shell 脚本的第一行以：#/bin/sh 或 #/bin/bash 开头

[TOC]

## 基础变量

1.多行注释

```shell
:<<EOF
# #! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。
# 首次运行出错运行：sed -i -e 's/\r$//' xxx.sh
EOF
```

2.变量定义与调用

```shell
name="michael"	#变量名和等号之间不能有空格		!!!
echo ${name}	#调用变量:$变量名 或 ${变量名}
readonly name	#设置为只读变量
echo "My name is $name"
unset name		#删除变量，不能删除只读变量
```

3.整型

```shell
a=1
#第一种整型变量自增方式
a=$(($a+1))
#第二种整型变量自增方式
a=$[$a+1]
#第三种整型变量自增方式
a=`expr $a+1`
#第四种整型变量自增方式
let a++
#第五种整型变量自增方式
let a+=1
#第六种整型变量自增方式
((a++))
```

3.字符串

```shell
str='this is a string "name" $name'	#单引号字符串中的变量是无效的
str2="this is a string \"name\" $name"	#双引号里可以有变量和转义字符
#拼接字符串
str3='hello,'$str''
str4="hello,$str2"
#获取字符串长度
echo ${#name}
#提取子字符串
str5=${str4:0:5}	#第0个字符开始截取5个字符
```

4.数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

```shell
num=(1 2 3 4 5)	#用括号来表示数组，数组元素用"空格"符号分割开
students=("jack" "tom" "mike" "sherry" "ming")
echo ${num[0]}
# 获取数组所有元素
echo "array:,${students[*]}"
# 获取数组元素的个数
echo "length of array:,${#students[*]}"
# 取得数组单个元素的长度
echo "length of array element:,${#students[0]}"
```

5.脚本参数传递

```shell
向脚本传递参数，脚本内获取参数的格式为：$n, n 代表一个数字。
$# 为传递参数的个数
$* 以一个单字符串显示所有向脚本传递的参数。
$@ 作为迭代器返回每个参数，使用时加引号:"$@"
$0 为执行的文件名
$1 为执行脚本的第一个参数, $2 为执行脚本的第二个参数, 当n>=10时，需要使用${n}来获取参数。
$? 最后命令的退出状态，0代表没有错误
```

```shell
echo "执行的文件名：$0"
echo "参数个数：$#"
echo "传递的参数作为一个字符串显示：$*";
echo "第一个参数：$1"
echo "第二个参数：$2"
for i in "$@"; do
	echo $i
done
```



## 运算操作

1.算术运算

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr。

```shell
val=`expr 2 + 6`	#使用反引号而不是单引号,运算符之间必须用空格隔开
val=`expr 2 \* 6`
val=`expr 12 / 3`
val=`expr 13 % 3`
val=`expr 3 == 3`
```

2.关系运算

```shell
-eq	#等于
-ne	#不等于
-gt	#大于
-lt	#小于
-ge	#大于等于
-le	#小于等于
```

```shell
#中括号两边必须有空格
if [ $a -eq $b ]	#等于	[ ]是执行运算
then
	echo "result is true"
else
	echo "result is false"
fi
```

3.布尔运算

```shell
-o #或
-a #且
!  #非
```

```shell
if [ $a -eq $b -o $a -lt $b ]	#或
then
	echo "result is true"
else
	echo "result is false"
fi

if [ ! $a -ne $b ]				#非
then
	echo "result is true"
else
	echo "result is false"
fi
```

4.字符串运算

```shell
=	检测两个字符串是否相等
!=	检测两个字符串是否不相等
-z	检测字符串长度是否为0
-n	检测字符串长度是否不为0
$	检测字符串是否为空
```

```shell
if [ $c = $d ]		#用==也可以		
then
	echo "result is true"
else
	echo "result is false"
fi

if [ -z $c ]		
then
	echo "result is true"
else
	echo "result is false"
fi
```



## 流程控制

1.if else

```shell
#格式
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

2.for循环

```shell
#格式
for var in item1 item2 ... itemN	#迭代器
do
    command1
    command2
    ...
    commandN
done
```

```shell
for str in 'This is a string'
do
    echo $str
done
```

3.while循环

```shell
#格式
while condition
do
    command
done
```

```shell
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```

```shell
#无限循环
while :	#或while true
do
    command
done

for (( ; ; ))
do
    command
done
```

4.case

```shell
#格式
case var in
mode1)
    command1
    command2
    ...
    commandN
    ;;
mode1）
    command1
    command2
    ...
    commandN
    ;;
esac
```

```shell
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```



## 函数

1.函数定义与调用

```shell
#格式 []表示可选
[ function ] funname [()]
{
    action;
    [return int;]
}
```

```shell
demoFun(){
    echo "这是一个 shell 函数!"
}

demoFun	#函数调用
```

2.函数参数

与脚本传递参数类似

```shell
获取参数的格式为：$n, n 代表一个数字。
$# 为传递参数的个数
$* 以一个单字符串显示所有向函数传递的参数。
$@ 作为迭代器返回每个参数，使用时加引号:"$@"
$0 为执行的函数名
$1 为执行函数的第一个参数, $2 为执行函数的第二个参数, 当n>=10时，需要使用${n}来获取参数。
$? 最后命令的退出状态，0代表没有错误
```

```shell
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```



## 输入输出重定向

将一个命令的标准输入/输出 重定向到其他文件。

1.重定向命令列表

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

*文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）*

```shell
#输出重定向 从默认的标准输出设备（终端）重定向到指定的文件。
echo "hello" > test.c

#输入重定向 本来需要从键盘获取输入的命令会转移到文件读取内容。
echo < test.c

#输入重定向 将标记之间的内容作为输入
cat << EOF
hello
world
EOF
```

2./dev/null 文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null

```shell
echo "hello" > /dev/null
```



## 文件包含

在脚本中调用别的脚本

test2.sh 代码

```shell
#!/bin/bash

#使用 . 号来引用test1.sh 文件
. ./test1.sh

#使用 source来引用test1.sh 文件
source ./test1.sh
```



## 常用字符

1.shell通配符

| **字符**              | **含义**                                    | **实例**                                                     |
| --------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| *                     | 匹配 0 或多个字符                           | a*b  a与b之间可以有任意长度的任意字符, 也可以一个也没有, 如aabcb, axyzb, a012b, ab。 |
| ?                     | 匹配任意一个字符                            | a?b  a与b之间必须也只能有一个字符, 可以是任意字符, 如aab, abb, acb, a0b。 |
| [list]                | 匹配 list 中的任意单一字符                  | a[xyz]b   a与b之间必须也只能有一个字符, 但只能是 x 或 y 或 z, 如: axb, ayb, azb。 |
| [!list]               | 匹配 除list 中的任意单一字符                | a[!0-9]b  a与b之间必须也只能有一个字符, 但不能是阿拉伯数字, 如axb, aab, a-b。 |
| [c1-c2]               | 匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z] | a[0-9]b  0与9之间必须也只能有一个字符 如a0b, a1b... a9b。    |
| {string1,string2,...} | 匹配 sring1 或 string2 (或更多)其一字符串   | a{abc,xyz,123}b    a与b之间只能是abc或xyz或123这三个字符串之一。 |

2.元字符 （特殊字符）

| 字符 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| IFS  | 由 <space> 或 <tab> 或 <enter> 三者之一组成(我们常用 space )。 |
| CR   | 由 <enter> 产生。                                            |
| =    | 设定变量。                                                   |
| $    | 作变量或运算替换(请不要与 shell prompt 搞混了)。             |
| >    | 重导向 stdout。 *                                            |
| <    | 重导向 stdin。 *                                             |
| \|   | 命令管线。 *                                                 |
| &    | 重导向 file descriptor ，或将命令置于后台执行。 *            |
| ( )  | 将其内的命令置于 nested subshell 执行，或用于运算或命令替换。 * |
| { }  | 将其内的命令置于 non-named function 中执行，或用在变量替换的界定范围。 |
| ;    | 在前一个命令结束时，而忽略其返回值，继续执行下一个命令。 *   |
| &&   | 在前一个命令结束时，若返回值为 true，继续执行下一个命令。 *  |
| \|\| | 在前一个命令结束时，若返回值为 false，继续执行下一个命令。 * |
| !    | 执行 history 列表中的命令。*                                 |

3.转义字符

| 字符       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| ‘’(单引号) | 又叫硬转义，其内部所有的shell 元字符、通配符都会被关掉。注意，硬转义中不允许出现’(单引号)。 |
| “”(双引号) | 又叫软转义，其内部只允许出现特定的shell 元字符：$用于参数代换 `用于命令代替 |
| \(反斜杠)  | 又叫转义，去除其后紧跟的元字符或通配符的特殊意义。           |

