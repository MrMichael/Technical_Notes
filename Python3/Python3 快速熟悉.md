# Python3 快速熟悉



[TOC]

## 一、基础

python脚本解析器声明

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-
```



### 数据类型

1.整型、浮点型、字符串、布尔类型

- 数据类型是在被调用的时候定义的

```python
a = 12
b = 0x12
c = 3.14
d = 'hello world'
e = True

print('%s' % a)	#数据类型是在被调用的时候定义的
print('0x%x' % b)
print('%s' % d)
print('I\'m \"OK\"!')	#字符串内含有特性字符可以使用转义字符
print('''michael
jack
tony''')	#打印多行的其中一种方法
print('你好', 'python3')	#组合打印
```

2.列表list

- 列表元素可以是不同数据类型，用中括号[]表示

```python
classmates = ['michael', 'jack', 'tony']
classmates.append('gavin')	#追加元素
classmates.insert(1, 'tom')	#插入元素
classmates.pop()	#删除末尾元素
classmates.pop(2)	#删除指定位置元素
classmates[1] = 'lily'	#修改元素
```

3.元组tuple

- 元素可以是不同数据类型,一旦初始化就不能修改，用小括号()表示

```python
fruit = ('apple', 'banana', 'orange', 123)
fruit = ('apple', classmates, 'orange')	#变量可以被重定义
fruit[1][0] = 'ming'	#如果元组中含有可变元素，则可变元素的内容是可修改的
```

4.字典dict

- 字典是无序的对象集合，字典的元素的值通过键来取，用大括号{}表示

```python
dict = {'name':'join', 'code':1234, 'dept':'sales', 5:'ok'}
print(dict)
print(dict['name'])	#通过键搜索值
print(dict[5])		#通过键搜索值
dict['age'] = 22	#添加元素
if dict.get('age'):
	print('%s is in dict' % 'age')
else:
	print('%s is not in dict' % 'age')
dict.pop(5)		#删除键值
print(dict.keys())	#打印所有键
print(dict.values())	#打印所有值
```

5.集合set

- 集合是一组键的集合，但不包含值，而且键不能重复
- 创建一个set，需要提供一个list作为输入

```python
s = set([1,2,3])	#创建一个set，需要提供一个list作为输入集合
print(s)
s.add(4)	#添加元素
s.add(3)	#添加元素，元素重复，只保留一个
print(s)
s.remove(2)	#删除元素
print(s)
s1 = set([1,2,3])
s2 = set([2,3,4])
print(s1 & s2)	#set可以看成数学意义上的无序和无重复元素的集合
```

6.数据类型转换

```python
f = int('123')		#字符串转整型
g = int(123.456)	#浮点型转整型
h = float('123.4')	#字符串转浮点型
i = str(123)		#整型转字符串
j = tuple([1,2,3,4])		#列表转元组
k = tuple(set([1,2,3,4]))	#集合转元组
l = list((1,2,3)) 		#元组转列表
m = list(set([1,2,3])) 	#集合转列表
```



### 流程控制

1.if-else

```python
age = input("birth:")	#input输出的是字符串
age = int(age)	#字符串转整型
if age >= 18:
	print('adult')
elif age >= 6:
	print('teenager')
else:
	print('kid')
```

2.for循环

```python
i = [1,2,3]
for value in i:		#通过迭代器循环
	print(value)

for value in list(range(2,5)):	#range生成从2开始小于5的数
	print(value)
	
for value in range(2,5):	#range生成从2开始小于5的数
	print(value)
	
l = [1,2,3,4,5,6]
for var in l[2:4]:	#分段循环
	print(var)

people = {'name':'jony', 'age':22, 'adpt':'sale'}
for item, value in people.items():	#多变量
	print(item,'\t:',value)
```

3.while循环

```python
n = 0
while n < 5:
	print(n)
	n += 1
	
#break
n = 0
while n < 10:
	if n > 5:
		break
	print(n)
	n += 1
	
	
#continue
n = 0
while n < 10:
	n += 1
	if n == 5:
		continue
	print(n)
```

4.列表生成式

- 对迭代器的元素进行修改，产生新的列表

```python
num1 = [x*x for x in range(1,5)] #x是迭代器的元素，x*x是组合而成的新的列表元素

num2 = [x*y for x in range(1,5) for y in range(2,5)]	#两重循环

P = [str(item) + '=' + str(value) for item, value in people.items()]
```

5.生成器 generator

- 在循环的过程中不断推算出后续的元素,不必创建完整的list，从而节省大量的空间

```python
g = (x*x for x in range(1,5))	#通过生成器产生的对象与元组是不同的
k = (1,2,3,4)
print(g)	
print(k)

for n in g:	#生成器和元组、列表都能迭代，但是生成器占用空间很少
	print(n)

for n in k:
	print(n)

def odd():
	print('step1')
	yield(1)	#函数中有yield，那么函数就会变为生成器
	print('step2')
	yield(3)
	print('step3')
	yield(5)
	return 
o = odd()	#通过生成器生成一个实例
next(o)		#每次调用next()，开始执行，遇到yield返回参数，再次执行next从上次返回yield的地方执行
next(o)
next(o)

for var in odd():
	print(var)
```



### 函数

1.函数定义与调用

```python
def printstd( str ):		#函数的参数类型在实际调用中决定
	'打印传入的字符串到标准输出'
	print(str)
	return

printstd(123)

f = printstd	#变量可以指向函数和调用函数
f('function f')
```

2.默认参数

```python
def printinfo(name = 'jack', age = 22):
	'打印传入的信息'
	print('name:', name)
	print('age:', age)
	return
	
printinfo()
```

3.不定长度参数

- 加了星号（*）的变量名会存放所有未命名的变量参数

```python
def printinfo2( argc, *argv ):
	"打印任何传入的参数"
	print(argc)
	print('argv len:', len(argv))
	for var in argv:
		print(var)
	return

printinfo2(5,['a','b'],2,3,4)
```

4.递归函数

```python
def fact(n):
	'递归处理参数'
	if n==1:
		return 1
	return n*fact(n-1)
```

5.高阶函数

- 一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数

```python
def add(a,b):
	return a+b

def fun1(x,y,f):
	return f(x,y)*y

print(fun1(1,2,add))
```

1）高阶函数map

- map()函数接收两个参数，一个函数，一个序列
- map()将传入的函数依次作用到序列的每一个元素，把结果作为新的序列返回

```python
def f(x):
	return x*x;

r = map(f, range(5))
print(list(r))
```

2）高阶函数reduce	（积累）

- reduce()函数接收两个参数，一个函数，一个序列
- reduce()将函数作用在序列上，这个函数必须接收两个参数，reduce将结果与序列下一元素作累积计算
- reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)

```python
from functools import reduce	#插入reduce函数 only python3

s = reduce(add, [1,2,3,4,5])
print(s)

#定义字符串转整型函数
DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
def str2int(s):
	def fn(x,y):
		return x*10+y
	def char2num(s):
		return DIGITS[s]
	return reduce(fn, map(char2num, s))

print('sum:', str2int('1245'))
```

3）高阶函数filter	（过滤）

- filter()函数接收两个参数，一个函数，一个序列
- filter将传入的函数依次作用到序列的每一个元素，然后根据返回`值为真`的元素

```python
def is_odd(n):
	return n % 2 == 1

m = filter(is_odd, range(10))
```

4）高阶函数sort	（排序）

- sorted()函数接收两个参数，一个函数（通过key指定），一个序列
- sorted通过参数key指定的函数依次作用到序列的每一个元素，并根据函数返回的结果对原来的元素进行排序（并非使用结果生成新的序列）
- ascii值越小的元素排在前面

```python
n = sorted([10,-5,3,-20,8], key=abs)	#序列按绝对值结果排序
print(n)

L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
def by_name(t):
	return t[0]	#字典元素的第一个对象
def by_score(t):
	return -t[1]	#字典元素的第二个对象
	
m = sorted(L, key=by_name)
print(m)
m = sorted(L, key=by_score) #分数从高到低
print(m)
```

6.返回函数

- 把函数作为结果值返回

```python
def lazy_sum(*args):
	def sum():
		ax = 0
		for n in args:
			ax = ax + n
		return ax
	return sum

f = lazy_sum(1,2,3,4,5)
print(f())	#当调用函数时，才真正计算求和结果
```

1）外部作用域

- 如果内部函数有其外部变量，每当返回一个函数时，都有自己的外部变量，从初始值开始；调用函数，外部变量就保持改变。

```python
def createcounter():
	x = [0,]	#外部作用域 对内部函数counter而言，x就是外部变量
				#每调用createcounter返回一个函数，都有自己的外部变量，从初始值开始，调用函数，外部变量就保持改变
	def counter():
		x[0] += 1 	#对序列操作不是对变量赋值，而是对指向的对象赋值，不受局部作用域影响
		return x[0]
	return counter

counterA = createcounter()
counterB = createcounter()
print(counterA(),counterA(),counterA())		#1，2，3
print(counterB(),counterB(),counterB())		#1，2，3
```

2）全局变量、外部作用域、内部作用域区别

- global k 声明为全局变量
- nonlocal n 声明为非局部变量
- 调用外层函数，其变量重新初始化

```python
k = 0
def outside():
    #外部作用域
	m = 1		
	n = 1
	global k	#声明为全局变量，不会重复定义一个变量
	k += 1
	def inside():
		m = 2	#局部作用域		两个m是不一样的变量
		nonlocal n	#声明为非局部变量，不会重复定义一个变量
		n += 1
		global k	#声明为全局变量，不会重复定义一个变量
		k += 1
		print(m,n,k)
		return
	inside()
	print(m,n,k)
	return

outside()	#2，2，2   1，2，2
outside()	#2，2，4   1，2，4
```

7.匿名函数

- 关键字lambda表示匿名函数，冒号前面的表示函数参数，冒号后面的表示返回值。
- 用最简单方式定义函数

```python
m = map(lambda x: x * x, [1,2,3,4,5])
print(list(m))

f = filter(lambda n: n%2==1, range(10))
print(list(f))

```

8.装饰器 decorator

- 装饰器是一个函数，其输入参数是函数，并返回一个函数，即对输入函数进行装饰。

1）自定义装饰器

```python
def log(func):
	def wrapper(*args, **kv):	#参数定义为(*args, **kv)，可以接受任意参数的调用
		print('call %s()'% func.__name__)	#额外添加的代码
		return func(*args, **kv)	#运行函数原来的代码
	return wrapper

def now():
	print('2019-01-24')
	return

f = log(now)
f()
```

2）使用python内置函数定义装饰器

```python
import functools

def log(func):
	@functools.wraps(func)	#表明是使用装饰器
	def wrapper(*args, **kv):
		print('call %s()'% func.__name__)
		return func(*args, **kv)
	return wrapper

g = log(now)
g()
```

3）带输入参数的装饰器

```python
import functools

def log(text):
	def decorator(func):
		@functools.wraps(func)
		def wrapper(*args, **kv):
			print('%s %s():' % (text, func.__name__))
			return func(*args, **kv)
		return wrapper
	return decorator
		
h = log("hello")(now)
h()
```

9.偏函数

- 通过functools.partial函数，把一个函数的某些参数设置默认值，返回一个新函数

```python
import functools

def info(x = 10, y = 5):
	print(x,y)
	return

info2 = functools.partial(info, y=2)	#y从默认的5改为2
info2()
```



## 二、模块使用

- 在Python中，一个.py文件就称之为一个模块（Module）
- 模块提高代码的可维护性，其包括Python内置的模块和第三方模块
- 相同名字的函数和变量完全可以分别存在不同的模块中，但尽量不要与内置函数名字冲突

### 自定义模块

模块命名：module.py

```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

'a test module'	#任何模块的第一行字符串被视为模块的文档注释

__author__ = 'Michael Lee'	#作者署名

import sys	#导入sys模块

def test():
	args = sys.argv		#sys.argv指向命令行的所有参数
	if len(args) == 1:
		print('hello world')
	elif len(args) == 2:
		print('hello, %s' % args[1])
	else:
		print('Too many arguments')
		
#_xxx和__xxx这样的函数或变量表示私有的，不应该被外部引用
def _private_1(name):
	return 'Hello, %s' % name
		
def _private_2(name):
	return 'Hi, %s' % name		
		
def greeting(name):
	if len(name) > 3:
		return _private_1(name)
	else:
		return _private_2(name)
		
	
#如果运行模块，解析器会将特殊变量__name__置为__main__
#所以如果是运行模块，就调用模块函数；只是导入模块，则不会运行
if __name__=='__main__':	
	test()
```

调用模块

```python
import module

module.test()
print(module.greeting('michael'))
```



### 内建模块

1.time模块

```python
import time	#引入时间模块

#获取UTC时间，单位s
ticks = time.time()	
print('Current UTC', ticks, 's')

#获取当地时间		格林时间，元组格式
localtime = time.localtime()	
print('local time:', localtime)

#获取格式化的时间		当地时间转换为简单的可读时间模式
formattime = time.asctime(localtime) 
print('format time:', formattime)

#获取格式化日期			定制输出格式
formatdate = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())
print('format date:', formatdate)
formatdate2 = time.strftime('%a %b %d %H:%M:%S %Y', time.localtime())
print('format date2:', formatdate2)

#获取某月日历
import calendar
cal = calendar.month(2019, 1)
print(cal)
```



2.datetime模块

```python
from datetime import datetime

now = datetime.now()
print(now)

#指定某个日期和时间
dt = datetime(2019,1,29,12,20)
print(dt)

#datetime转换为timestamp
#timestamp是相对于epoch time（1970年1月1日 00:00:00 UTC+00:00时区）的秒数
tstamp = now.timestamp()
print(tstamp)

#timestamp转换为datetime
dt = datetime.fromtimestamp(tstamp)
print(dt)

#datetime加减
from datetime import datetime, timedelta
now = now + timedelta(hours=8)
print(now)
now = now - timedelta(days=2,hours=8)
print(now)
```



## 三、面向对象编程

- 采用面向对象的程序设计思想，首选思考的不是程序的执行流程，而是如`Student`这种数据类型应该被视为一个对象，这个对象拥有`name`和`score`这两个属性（Property）。给对象发消息实际上就是调用对象对应的关联函数，我们称之为对象的方法（Method）。
- Class是一种抽象概念，比如我们定义的Class——Student，是指学生这个概念，而实例（Instance）则是一个个具体的Student



### 类与实例

- class定义一个类，类是一个抽象的概念，类的内部有属性和方法
- 类名通常是大写开头的单词

```python
class Student(object):

	#__init__方法定义初始化实例时要创建的属性
	#类的方法的第一个参数永远是self，表示创建的实例本身
	def __init__(self, name, score):
		self.__name = name	#属性名称前面加上两个下划线就变为私有属性，外部不能访问
		self.__score = score
	
	#自定义方法
	#类的方法的第一个参数永远是self，调用时不用传递该参数，其他与函数一致
	def get_name(self):
		return self.__name
		
	def get_score(self):
		return self.__score
		
	def set_score(self, score):
		if score >= 0 and score <=100:
			self.__score = score
			return True
		else:
			return False

	def get_grade(self):
		if self.__score >= 90:
			return 'A'
		elif self.__score >= 60:
			return 'B'
		else:
			return 'C'
```

- 通过类创建的变量就是实例object

```python
michael = Student('michael', 100)	#传入创建实例需要的属性的参数
lisa = Student('lisa', 98)
michael.set_score(99)

print(michael.get_name(), michael.get_score(), michael.get_grade())
print(lisa.get_name(), lisa.get_score(), lisa.get_grade())
```

- python中所有的对
- 象（包括整数、字符串、函数和类等）都是从一个类中创建出来的

- str是用来创建字符串对象的类，int是用来创建整数对象的类。type就是创建类对象的类



### 继承与子类

- 当我们定义一个class的时候，可以从某个现有的class继承
- 新的class称为子类（Subclass），而被继承的class称为父类（Base class）
- 其实所有的类都是从object这个父类继承

```python
class Animal(object):
	def __init__(self, type):
		self.type = type
	
	def get_type(self):
		print('type is %s' % self.type)
	
	def run(self):
		print('Animal is running')
		
	def eat(self):
		print('Animal is eating')
		
#子类获得了父类的全部功能
class Dog(Animal):
	def watch(self):	#子类可以添加自己的属性和方法
		print('Dog is watching')
	def eat(self):	#当子类与父类存在相同的方法，子类方法会覆盖父类，即多态
		print('Dog is eating beef')	
	
mammal = Animal('mammal')
jim = Dog('dog')
jim.get_type()
jim.run()
jim.eat()
jim.watch()

def eat_food(animal):
	animal.eat()
	
eat_food(jim)
eat_food(mammal)	#多态的好处是可以根据不同实例对相同方法有不同处理

```



### 动态绑定属性和方法

给实例绑定属性

```python
class Student(object):
	pass
	
s = Student()
s.name = 'Jack'	#动态给实例绑定一个属性，只对当前实例有效
print(s.name)
```

给实例绑定方法

```python
def set_age(self, age):
	self.age = age	#调用时动态给实例绑定一个属性
	
from types import MethodType
s.set_age = MethodType(set_age, s) #给实例绑定一个方法
s.set_age(23)
print(s.age)
```

给类动态绑定属性和方法

```python
Student.name = ''	#直接给类动态绑定属性
Student.set_age = set_age	#直接给类动态绑定方法, 这样所有实例都可以调用
Tom = Student()
Tom.set_age(25)
```



### 限制类的属性`__slots__` 

类和实例不能绑定限制属性之外的属性

```python
class People(object):
	__slots__ = ('name', 'age')	# 用元组tuple定义允许绑定的属性名称

Worker = People()
Worker.name = 'Mike'
Worker.age = 26
#Worker.height = 180
print(Worker.name, Worker.age)
```



### 类的装饰器@property

- 在方法前一行加上@property，可以把一个类方法转换为类属性
- 用属性的方式操作方法，使调用更简单，也可以很好保护内部参数，避免乱设置

```python
class Animal(object):

	#初始化时的实例属性
	def __init__(self, skill, age):
		self.__skill = skill
		self.__age = age

	@property	#装饰器@property把一个getter方法变为属性，可读功能
	def skill(self):
		return self.__skill

	@skill.setter	#装饰器@skill.setter把一个setter方法变为属性，可写功能
	def skill(self, skill):
		self.__skill  = skill
		
	@property	#装饰器@property把一个getter方法变为属性，可读功能
	def age(self):
		return self.__age

	@age.setter	#装饰器@skill.setter把一个setter方法变为属性，可写功能
	def age(self, age):
		self.__age  = age

cat = Animal('run', 2)
print(cat.skill, cat.age)	#方法变为了可读属性
#print(cat.skill(), cat.age())	#当作方法调用会出错
cat.skill = 'eat'	#方法变为了可写属性
cat.age = 3
```



### 动态地创建类

- type函数可以动态地创建类
- type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）)

```python
MyClass = type('MyClass', (), {})	#类的父类是object，类的属性为空

def set_age(self, age):
	self.age = age
	return

def get_age(self):
	print('get_age:', self.age)
	return

#通过字典定义类的属性或方法
MySubClass = type('MySubClass', (MyClass,), {'age':12, 'set_age':set_age, 'get_age':get_age,})
print(MySubClass)
mysubclass = MySubClass()	#类需要创建实例才能调用方法
mysubclass.set_age(10)
mysubclass.get_age()
```



### 多重继承

- 一个子类可以从多个父类中继承

```python
class Runable(object):
	def run(self):
		print('Running')

class Flyable(object):
	def fly(self):
		print('Flying')

class Mammal(Animal, Runable):	#子类同时继承了多个父类
	pass

class Bird(Animal, Flyable):
	pass
	
dog = Mammal('run', 2)
parrot = Bird('fly', 3)
print(dog.skill, dog.age)
dog.run()
print(parrot.skill, parrot.age)
parrot.fly()
```



### 枚举类

- 从父类Enum中继承的子类是枚举类
- 提供互不冲突的属性

```python
from enum import Enum, unique

@unique	#@unique装饰器可以帮助我们检查保证没有重复值
class Weekday(Enum):	#从父类Enum中继承
	Sun = 0 # Sun的value被设定为0
	Mon = 1
	Tue = 2
	Wed = 3
	Thu = 4
	Fri = 5
	Sat = 6

day = Weekday.Sun
print(Weekday.Sun)
print(Weekday.Sun.value)
print(day == Weekday.Sun)

@unique
class Gender(Enum):
	Male = 0
	Female = 1
	
class Student(object):
	def __init__(self, name, gender):
		self.name = name
		self.gender = gender

bart = Student('Bart', Gender.Male)
if bart.gender == Gender.Male:
	print('Test Pass')
else:
	print('Test Fail')
```



## 四、文件操作

### 读文件操作

1.不限长度读取

- UTF-8编码的文本文件

```python
PATH = '/home/michael/Python3_Program/5_IO/'

try:
	f = open(PATH+'test.txt', 'r')	#打开文件
	#调用read()方法可以一次读取文件的全部内容，用一个str对象表示
	print(f.read())
finally:
	if f:
		f.close()	#关闭文件
```

2.使用with语句自动关闭文件

```python
with open(PATH+'test.txt', 'r') as f:
	print(f.read())
```

3.限制长度读取：read(size)

```python
with open(PATH+'test.txt', 'r') as f:
	str = f.read(10)
	while str!='':	#限定一次读10bytes
		print(str)
		str = f.read(10)
```

4.按行读取：readline()

```python
with open(PATH+'test.txt', 'r') as f:
	str = f.readline()
	while str!='':
		print(str.strip())	# 把末尾的'\n'删掉
		str = f.readline()
```

5.读二进制文件	比如图片、视频等

```python
with open(PATH+'file.bin', 'rb') as f:
	str = f.readline()
	print(str)
```



### 写文件操作

1.以覆盖形式写入

```python
with open(PATH+'test.txt', 'w') as f:
	f.write('hello file2\n')	#当调用close时，系统才能保证数据已经写入文件
```

2.以追加形式写入

```python
with open(PATH+'test.txt', 'a') as f:
	f.write('hello file3\n')
```



### 内存读写操作

1.从内存中读写str：通过StringIO模块

```python
from io import StringIO

f = StringIO()	#创建StringIO后就可以像普通文件那样读写
f.write('hello stringio')
print(f.getvalue())	#getvalue()方法用于获得写入后的str
```

2.从内存中读写二进制数据：通过BytesIO模块

```python
from io import BytesIO

f = BytesIO()
f.write('中文'.encode('utf-8'))
print(f.tell())	#返回当前读写位置
f.write('hello'.encode('utf-8'))
print(f.tell())	#返回当前读写位置
print(f.getvalue())
```



### 序列化

- 把变量从内存中变成可存储或传输的过程称之为序列化，序列化后的内容可以写入磁盘或通过网络传输。
- 把变量内容从序列化的对象重新读到内存里称之为反序列化

1.对象序列化成bytes类型：通过pickle模块

- pickle.dumps()方法把可序列化对象编码成bytes
- pickle.loads()方法将bytes反序列化出对象

```python
import pickle

d = dict(name='Bob', age=20, score=88)
bytes = pickle.dumps(d)	#pickle.dumps()方法把可序列化对象编码成bytes
print(bytes)	
e = pickle.loads(bytes)	#pickle.loads()方法将bytes反序列化出对象
print(e)

with open(PATH+'dump.txt', 'wb') as f:
	pickle.dump(d, f)	#pickle.dump()把对象序列化后写入一个file-like Object(文件描述符)

with open(PATH+'dump.txt', 'rb') as f:
	g = pickle.load(f)	#pickle.load()方法从一个file-like Object(文件描述符)中直接反序列化出对象
	print(g)
```

2.对象序列化成json类型：通过json模块

- JSON表示出来就是一个字符串，可以被所有语言读取，也可以方便地存储到磁盘或者通过网络传输

```python
import json

d = dict(name='Tom', age=20, score=88)	# 不一定要字典，可序列化为JSON的对象都可以
jsonstr = json.dumps(d)	#json.dumps 用于将 Python 对象编码成 JSON 字符串。
print(jsonstr)
e = json.loads(jsonstr)	#json.loads 用于解码 JSON 数据。该函数返回 Python 字段的数据类型。
print(e)

with open(PATH+'json.txt', 'w') as f:
	json.dump(d, f)	#json.dump()直接把对象序列化后写入一个file-like Object(文件描述符)
	pass
	
with open(PATH+'json.txt', 'r') as f:
	g = json.load(f)	#json.load()方法从一个file-like Object(文件描述符)中直接反序列化出对象
	print(g)
```

3.将类序列化成json类型

```python
class Student(object):
	def __init__(self, name, age, score):
		self.name = name
		self.age = age
		self.score = score

#输入一个实例，返回一个字典对象
def student2dict(std):	
	return {
		'name':std.name,
		'age':std.age,
		'score':std.score
	}

#输入一个字典对象，返回一个实例
def dict2student(d):	
	return Student(d['name'], d['age'], d['score'])

s = Student('Jack', 20, 88)
#方法1
#jsonstr = json.dumps(s, default=student2dict)	#需要传入将类转换为字典的函数
#方法2
jsonstr = json.dumps(s, default=lambda obj: obj.__dict__) #class的实例都有一个__dict__属性，它就是一个dict，用来存储实例变量。
print(jsonstr)
s1 = json.loads(jsonstr, object_hook=dict2student)
print(s1)
```



## 五、进程

- 多进程中，同一个变量，各自有一份拷贝存在于每个进程中，互不影响。

### 进程基础使用

```python
import os	#插入系统调用模块

# Only works on Unix/Linux/Mac, Windows没有fork调用，
pid = os.fork()	#对父进程返回子进程id，对子进程返回0
if pid == 0:
	#child
	print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
	#parent
	print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
```



### 多进程模块multiprocessing

1.通过multiprocessing模块的Process类创建一个进程对象

```python
from multiprocessing import Process
import os

#子进程要执行的代码
def run_proc(name):
	print('Run child process %s (%s).' % (name, os.getpid()))

if __name__ == '__main__':
	print('Parent process %s.' % os.getpid())
    #传入执行函数和执行函数的参数
	p = Process(target=run_proc, args=('test',))
	print('Child process will start.')
	p.start()	#启动子进程
	p.join()	#等待子进程结束后再继续往下运行
	print('Child process end.')
```

2.通过multiprocessing模块的Pool类【进程池】批量创建子进程

```python
from multiprocessing import Pool
import os, time, random

def long_timr_task(name):
	print('Run task %s (%s)...' % (name, os.getpid()))
	start = time.time()	#获取当前时间
	time.sleep(random.random()*3)
	end = time.time()	#再次获取当前时间
	print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
	print('Parent process %s.' % os.getpid())
	p = Pool(4)	#定义一个进程池，最大进程数4
	for i in range(5):
		#Pool.apply_async(要调用的目标,(传递给目标的参数元祖,))
		#传入子进程执行函数和执行函数的参数，并启动进程
		p.apply_async(long_timr_task, args=(i,))

	p.close()	#关闭pool，使其不在接受新的任务。
	p.join()	#等待子进程结束后再继续往下运行
	print('All subprocesses done.')
```



### 子进程模块subprocess

- subprocess模块可以方便地启动一个外部子进程，然后控制其输入和输出

```python
import subprocess

r = subprocess.call(['nslookup', 'www.python.org'])	#运行外部进程：即执行命令nslookup
print('Exit code:', r)
r = subprocess.call(['pwd',])	#运行外部进程：即执行命令pwd
print('Exit code:', r)
```



### 进程间通信

- Python的multiprocessing模块包装了底层的机制，提供了Queue、Pipes等多种方式来交换数据

```python
from multiprocessing import Process, Queue
import os, time, random

#写进程代码
def ps_write(q):
	print('Process to write: %s' % os.getpid())
	for value in ['A', 'B', 'C']:
		print('Put %s to queue...' % value)
		q.put(value)
		time.sleep(random.random())

#读进程代码
def ps_read(q):
	print('Process to read: %s' % os.getpid())
	while(True):
		value = q.get(True)	#如果队列为空，get操作会阻塞等待
		print('Get %s from queue.' % value)
	
if __name__=='__main__':
	q = Queue()	#创建队列
	pw = Process(target=ps_write, args=(q,))
	pr = Process(target=ps_read, args=(q,))
	pw.start()
	pr.start()
	pw.join()
	pr.terminate()	#pr进程里是死循环，无法等待其结束，只能强行终止
```



## 六、线程

- Python解释器由于设计时有GIL全局锁，导致了多线程无法利用多核。

### 线程定义与使用

```python
import time, threading

# 新线程执行的代码
def loop():
	print('thread %s is running...' % threading.current_thread().name)
	n = 0
	while n < 5:
		n = n + 1
		print('thread %s >>> %s' % (threading.current_thread().name, n))
		time.sleep(0.2)
	print('thread %s ended.' % threading.current_thread().name)


print('thread %s is running...' % threading.current_thread().name)
#传入线程执行函数和函数的参数
t = threading.Thread(target=loop, name='LoopThread')
t.start()	#启动线程
t.join()	#等待线程结束
print('thread %s ended.' % threading.current_thread().name)
```



### 互斥量

- 多线程中，所有变量都由所有线程共享，任何一个变量都可以被任何一个线程修改
- 要保证一个线程使用资源时，不被其他线程打扰
- 当两个线程在等待对方的资源时，就有可能导致互斥

```python
import threading

balance = 0
lock = threading.Lock()	#定义互斥量

def change_it(n):
	global balance	#全局变量声明
	balance = balance + n
	balance = balance - n
	
def run_thread(n):
	for i in range(100000):
		lock.acquire()	#获取互斥锁
		try:
			change_it(n)
		finally:
			lock.release()	#释放互斥锁

t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(5,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)
```



### 线程内的公共局部变量

- 线程的局部变量只有自己能看到，但如果线程中调用函数，通过函数入口参数传递会比较麻烦。
- ThreadLocal是线程的公共变量区，对于线程中调用的其他函数是可访问的，而不必通过参数传递

```python
import threading
local_school = threading.local()
temp_age = 12	#使用全局变量也可以传递参数，但是不安全

def process_student():
	# 获取当前线程关联的student:
	std = local_school.student
	grade = local_school.grade
	age = temp_age	#函数中只读全局变量，不用声明
	print('Hello, %s %d %d(in %s)' % (std, grade, age, threading.current_thread().name))

def process_thread(name, grade, age):
	# 定义ThreadLocal的变量
	local_school.student = name	#ThreadLocal全局变量在每个线程中都有独立副本
	local_school.grade = grade
	global temp_age
	temp_age = age	#函数中对全局变量赋值，需要声明，不声明代表赋值的局部变量变量
	process_student()

#name为线程名称
t1 = threading.Thread(target=process_thread, args=('Alice', 6, 14), name='Thread-A')
t2 = threading.Thread(target=process_thread, args=('Bob', 5, 13), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()
```



## 七、网络通信



### TCP

1.服务器端	使用线程

- 绑定的ip地址需要是本机地址

```python
import socket
import threading

# 创建socket	AF_INET指定使用IPv4协议，SOCK_STREAM指定使用面向流的TCP协议
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)       
s.bind(('192.168.83.130', 8000))	 # 绑定地址和端口
s.listen(5)	#最多监听5个客户端

def fun(sock, addr):
	print('Accept new connection from {}'.format(addr))
	sock.send('Hello!'.encode())	# 向客户端发送数据
	while True:
		data = sock.recv(1024)		# 每次最多接收1024b(1kb)
		if data == 'exit' or not data:
			break
		sock.send('hello,{}'.format(data).encode())	# 向客户端发送数据
	sock.close()
	print('Connection closed {}'.format(addr))

print('Waiting for connection...')
while True:
	sock, addr = s.accept()	# 等待连接，返回socket对象和请求连接的客户端地址
	# 创建新线程（或进程）来处理TCP连接
	t = threading.Thread(target=fun, args=(sock, addr))
	t.start()# 启动线程
```

2.客户端

```python
import socket

#创建socket	AF_INET指定使用IPv4协议，SOCK_STREAM指定使用面向流的TCP协议
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 建立连接，参数是tuple类型
s.connect(('192.168.83.130', 8000))	#阻塞式连接，连接成功继续执行后面代码

print(s.recv(1024))
for data in ['li', 'bo', 'ca']:
	s.send(data.encode())	# 向服务器发送数据	对发送的数据进行编码
	print(s.recv(1024))		# 每次最多接收1024b(1kb)
s.send('exit'.encode())
s.close()
```



### UDP

1.服务器端	使用线程

- udp服务器不需要连接，对接收的数据处理即可

```python
import socket
import threading

 # 创建一个基于UDP的socket对象
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(('192.168.83.130', 8000))	# 绑定地址与端口

def fun(data, addr):
	print('from {}, content is {}'.format(addr, data))
	s.sendto('Hi!{}'.format(data).encode(), addr)	# 向客户端返回内容

while True:
	#等待接收客户端发送的内容
	data, addr = s.recvfrom(1024)	#返回客户端的数据和地址
	t = threading.Thread(target=fun, args=(data, addr))
	t.start()	# 启动线程
```

2.客户端

```python
import socket

# 创建一个基于UDP的socket对象
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
# 向指定服务器与端口号发送内容
s.sendto('hello!'.encode(), ('192.168.83.130', 8000))
print(s.recv(1024))
s.close()
```













































