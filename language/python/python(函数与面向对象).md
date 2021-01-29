# **Python**(函数与面向对象)
@(Knowledge)[Auspice Tian][2021年1月24日20:50:48]

---

[TOC]

---

<div style="page-break-after:always"></div>

## 函数(function)

> 函数也是 **对象**
> 保存可执行代码，并可重复调用

### 定义函数
> 函数名中保存内存首地址

```python
def 函数名([形参1,形参2...]):
	代码块
```

### 调用函数
> 函数名()

### 参数(形参)
> 形参声明变量，但不赋值

- 形参可指定默认值
- 调用时不传参，默认值生效
```python
def fn(a,b,c = 20):
	print(c)

fn(1,2,3)# 3
```

#### 实参
> 调用函数时，传递的参数

- 关键字参数和位置参数可混合使用
- 混合使用时，**位置参数必须在关键字参数前面**
- 同一形参只能赋值一次
##### 位置参数
> 将对应位置实参，赋值给对应的形参

##### 关键字参数
> 直接根据参数名传递参数

```python
fn(b = 1,c = 3,a = 2)
```

##### 实参类型
> 解析器不会检查实参类型
> 包括函数

- 如果形参指向的是一个对象，修改会影响指向对象的值

```python
c = [1,2,3]

fn(c.copy())
fn(c[:])
```

#### 不定长参数
> 形参前加 * ，将获取所有剩余形参保存至元组

- 接收所有位置实参，将这些实参保存到同一个元组
- 装包参数最好放于参数列表末尾
	- 若装包参数位于参数列表中间，其后参数必须以关键字参数形式传递
	- 关键字参数必须放于参数列表最后
```python
def fn(a,*b,c):
	print(a,b,c)
	# a=1,b=(2,3,4),c=5
f(1,2,3,4,c=5)
```
- 带 * 形参只能写一个
```python
# 求任意数字的和
def sum(*a):
	print("a=",a,type(a))# tuple
	result = 0
	for i in a:
		result += n
	return n
```
- 要求所有参数以关键字形式传递
```python
def fn(*,a,b,c):
	pass
```
- *参数只能接收位置参数
- ** 可以接受其他关键字参数，保存至字典
	- 键：关键字参数名
	- 值：参数值
	- 只能有一个
	- 只能写在参数列表最后
```python
def fn(**a):
	pass
	# a = {'a':1,'b':2,'c':3}
fn(a=1,b=2,c=3)
```

#### 参数解包
```python
def fn(a,b,c):
	pass

t = (10,11,12)
fn(*t)
```

- 序列中元素个数与形参个数一致
- *序列解包
- **字典解包
![Alt text](python(函数与面向对象).assets\1585301982654.png)
![Alt text](python(函数与面向对象).assets\1585301991659.png)

### 返回值
> 关键字 **return**

- 如果return后不写或没有return ，返回空
- return用来结束函数

### 文档字符串
> 直接在函数第一行写一个字符串

```python
def fn(a:int,b:bool,b:str = 'Hello')-> int:
```
	文档字符串实例
	形参列表，参数名后加:表明参数类型
	函数声明 -> 表示返回类型
	```
	return 10
```

#### help(fnName)
> 查询python中函数用法

### 作用域（scope）
> 变量生效区域

1. **两种作用域**
- 全局作用域:
	- 在程序执行时创建，程序结束时销毁
	- 所有函数以外的区域
- 函数作用域
	- 在函数调用时创建 ，调用结束销毁
	- 每调用一次，产生新的函数作用域
	- 函数作用域中定义的变量，是局部变量，只能在函数内部访问

2. **作用域嵌套**
- 赋值默认为当前作用域
3. **同名变量**
- 优先在当前作用域寻找，如果有则使用
- 当前作用域没有，去上一级作用域查找
4. **函数内部修改全局变量**
- **global**声明变量
- global a

### 命名空间(namespace)
> 变量作用域

- 每个变量都需要存储到指定的命名空间内
- 存储变量的字典

1. 全局命名空间
- 保存全局变量 
2. 函数命名空间
- 保存函数中的变量

#### locals()
> 获取当前作用域的命名空间

1. 向scope添加变量
- 向字典中添加键值对
​```python
scope = locals()
scope['c'] = 1000
```
2. **只用于查看当前命名空间**
3. 外部不能访问内部

#### global()
> 在任意命名空间中获取全局命名空间


### 递归
> 将大问题分解为小问题，知道不可再分
> 解决小问题

- 递归与循环基本可互相转换
- 可用循环优化代码

```python
# 递归求阶乘
def fac(n):
	if n==1:
		return 1
	else:
		return n*fab(n-1)
```

```python
# 汉诺塔实践
# 函数定义
# def hanoi (n, src, dst, mid):
#     pass
# 参数说明
# n是整数，表示圆柱 A 上面的圆盘个数。
# src是大写字母A，表示最左边的圆柱。
# dst是大写字母C，表示最右边的圆柱。
# mid是大写字母B，表示中间的圆柱。

count = 0

def hanoi (n, src, dst, mid):
    global count
    if n == 1:
        print("{}: {}->{}".format(1, src, dst))
        count += 1
    else:
    	hanoi(n-1,src,mid,dst)
    	count += 1
    	print(f"{count}:{src}->{dst}")
    	hanoi(n-1,mid,dst,src)


hanoi(3, "A", "C", "B")
print(count)
```

```python
# 幂运算
# n：底数
# i：次数
def fn(n,i):
	if i == 1:
		return n
	else:
		return n*fn(n,i-1)

print(fn(2,3))
```

```python
# 回文串

def huiwen(str,beg,end):
	if beg==end:
		return True	
	if str[beg]!=str[end]:
		return False
	else:
		return huiwen(str,beg+1,end-1)

str = "abaa"
print(huiwen(str,0,len(str)-1))

def hui_wen(str):
	if len(str)<2:
		return True
	elif　str[0] != str[-1]:
		return False
	return hui_wen(str[1:-1])
```

### 函数式编程
> Python中，函数式一等对象

#### 一等对象
1. 对象是运行是创建
2. 能赋值给变量或作为数据结构中的元素
3. 能作为参数传递
4. 能作为返回值返回

### 高阶函数
> 函数的函数

1. 接收一个或多个函数作为参数
2. 将函数作为返回值返回

#### 函数作为参数
```python
def fn1(a):
	# 检查是否为偶数
	if a%2==0:
		return True
	else:
		return False
def fn5(a):
	# 检查是否大于5
	if a > 5:
		return True
	else:
		return False
		
def fn(fnc,a):
	# 判断a是否满足fnc
	if fnc(a):
		return True
	else:
		return False

print(fn(fn5,1))
```

##### 匿名函数(lambda)
> 创建一些简单函数

###### 语法
- lambda 参数列表:返回值

```python
def fn(a,b):
	return a+b

lambda a,b:a+b
```

##### fliter(fnc,iteSeq)
> 从序列中过滤出符合条件的元素，返回一个可迭代结构


##### map(fnc,iteSeq)
> 对可迭代元素做指定操作，添加到新对象中返回

```python
print(map(lambda i:i+1,lst))
```

##### list.sort(fnc)
> 默认比较大小

- 每次取列表中的一个元素作为一个参数来调用函数
- 使用函数返回值比较元素大小
- key函数只在比较时起作用，不改变原数据

```python
lst = ['a','bbb','a']

lst.sort(key=len)

print(lst)
```

##### sorted(seq,key=)
> 可对任意序列进行排列

- 不影响原序列，返回新对象

#### 闭包
> 将函数作为返回值返回

- 返回内部函数
- 通过闭包创建只有当前函数可访问的变量
- 函数嵌套


### 装饰器
> 通过装饰器可以在不修改原函数情况下对函数进行扩展

```python
def fn(old_fun):
	def new_fun(*args,**kwargs): 
		# *args接收所有位置参数
		# kwargs接收所有关键字参数
		print('开始执行')
		result = old_fun(*args,**kwargs)
		return result
	
	return new_fun

@fn
def old_fun():
	pass

# old_fun被装饰
```

- 通过@装饰器，使用指定装饰器
	- 可以同时为一个函数指定多个装饰器，函数 **由内向外** 装饰器装饰

#### OCP(开闭原则)
- 开放对程序的扩展
- 关闭对程序的修改

<div style="page-break-after:always"></div>

## 面向对象

### 类与对象
#### 对象
> 内存中专门用于存储数据的一块区域

1. 三部分
- 对象的标识--id
- 对象的类型--type
- 对象的值--value

#### 类(class)
> 对象是类的实例

- 自定义类使用 **大写字母开头** 

```python
# 内置类创建

a = int(10) # create int object
b = str('hello') # create a string object
```

- **创建自定义类**
	- 语法：
```python
class 类名([父类]):
	代码块

class Myclass():
	pass
```

- 使用自定义类创建对象
```python
class Myclass():
	pass

mc = Myclass()
```

#### 对象的创建流程
> 类是用来创建对象的对象

1. 创建变量
2. 在内存中创建一个新对象
3. 执行_init_()
4. 将对象的id赋值给变量

#### 类的定义
> 属性+方法

- 在类中定义的变量，将成为所有实例的公共属性
	- 公共属性都可以被实例访问

- 类中定义的函数为方法
	- 所有实例都可访问类方法
	- **方法调用** 默认传递一个参数，所以 **方法** 中至少有一个形参


#### 属性和方法
当调用一个对象的属性时，解析器会现在当前对象中找是否有该属性
- 有，则直接返回对象的属性值
- 没有，去类对象中找
	- 有，则返回类属性
	- 无，报错

**类属性(方法)**
> 所有实例共享的属性(方法)

**实例属性(方法)**
> 某个实例特有的属性(方法)

- 一般情况下，**属性** 保存在实例对象中
- **方法**保存在类对象中

#### self
> 本实例，相当于**this**

#### 类init()
- 特殊方法以 **_** 开始和结束
- 特殊方法不需要自己调用

##### _init()_在对象创建时执行
> 在新创建的对象初始化属性

- 通过self向新创建的对象初始化属性
	- 作为实例属性

```python
class Myclass:
	def _init_(self.name):
		self.name = name
```

- 调用类创建对象时，参数列表中的参数会依次传递到init()中

#### 类的基本结构
```python
class 类名([父类]):
	公共属性...
	
	# 对象初始化方法
	def _init_(self,...):
		...
	
	# 其他方法
	def method_1(self,...):
		...
	
	def method_2(self,...):
		...
```

#### 类属性
> 类属性只能通过类方法修改，实例方法无法修改

#### 实例属性
> 实例属性，只能通过实例方法访问或修改的属性，类方法无法修改

#### 实例方法
> 第一个参数为 **self** 的方法是实例方法
> 将 *对象* 作为 *self* 传入

- 类和实例都可调用实例方法
	- 通过实例调用，将当前调用对象作为 **self** 自动传入
	- 通过类调用，不会自动传递当前对象，需手动传入 **实例**

```python
class A:
	pass
	
a = A()

a.test()

A.test(a)
```

#### 类方法
> 使用 **@classmethod** 修饰的方法

- 第一个参数为 **cls**, 自动传递，
	- **cls** 表示当前类对象
```python
class A:
	@classmethod
	def test(cls):
		print(cls)

```
- 通过类和实例调用

#### 静态方法
> 类中 **@staticmethod** 修饰的方法

- 不需要传递参数
- 可通过类和实例调用

### 面向对象三大特性
#### 1. 封装
> 确保对象数据安全

> 隐藏对象中不希望被外部访问的属性或方法

- 通过更换变量名，隐藏
- 提供 **getter** 和 **setter** 外部可以访问到属性
	- **getter** 获取对象中的指定属性
		- get_属性名()
	- **setter** 修改对象中的指定属性
		- set_属性名()
		- 在 **setter** 中增加数据验证，确保数据值的正确性

- 一般情况 **_属性名** 表示隐藏私有属性

##### 对象属性前使用双下划綫
> 实际上将名字修改为 **\__类名__属性名**

```python
# __propertyName
class Person:
	def _init_(self,name):
		self.__name = name
	
	def get_name(self,name):
		return self.__name
	
	def set_name(self,name):
		self.__name = name
```

##### property装饰器
> property 装饰器，将一个get方法转换为对象属性

- 使用 **@property** 方法，必须和属性名一样
	- **setter装饰器**: *@属性名.setter*
	- **getter装饰器**: *@property*
- 使用**setter装饰器** 必须有 **getter装饰器**

```python
# @property
class Person:
	def _init_(self,name):
		self._name = name
	
	@property
	def name(self,name):
		return self._name
	
	@name.setter
	def name(self,name):
		self._name = name

p = Person('name1')
print(p.name)
```

#### 2.继承
> 保证对象的可扩展性

> 子类继承父类中所有方法
> **包括特殊方法**
- \__init__(self,name)

```python
class 子类名([父类列表]):
	pass
```

- 省略父类列表，默认为父类 **object**
- isinstance(instanceName,className)
> 检查是否是某类的实例

- issubclass(subClassName,className)
> 检查是否为子类


##### 重写(overwrite)
> 子类中有与父类同名方法，调用子类方法
> **包括特殊方法**
- \__init__(self,name)

##### super()
> 调用父类方法修改父类属性

- 通过 *super()* 调用父类方法，不需要传递 *self*

```python
def Subclass(SuperClass):
	def __init__(self,[Subclass propertyList]):
		super().__init__([SuperClass propertyList])
		self._propertyName = propertyVlue
```

##### className.\__bases__
> 获取当前类的所有父类

##### 多重继承
> 为一个类指定多个父类

- 父类列表前面方法覆盖后边
	- 第一个父类
	- 第一个父类的父类
	- 第二个父类
	- ...

#### 3. 多态
> 保证程序的灵活性

> 不考虑参数类型，只要符合某些特征，就可以使用方法

1. len()
-	只要对象中具有 **\__len__()** 特殊方法，就可以通过 **len()** 方法获取对象长度

### 垃圾回收
> 没有引用的对象为 **垃圾**

#### 自动垃圾回收机制
> 不需要手动处理垃圾

- 在 **程序结束** 时，自动删除垃圾

#### \__delete__(self)
> 在垃圾回收前删除

### 特殊方法
> **\__** 开头和结尾

- 配合多态使用
- 特殊方法不需要手动调用

#### \__new__(cls)
> 对象创建时调用

#### \__init__(self)
> 对象初始化

#### \__delete__(self)
> 对象删除前调用

#### \__str__(self)
> ** ** 打印的是\__str()__的返回值

#### \__repr__(self)
> 会对当前对象使用 **repr()** 时调用
> 指定对象在 **交互模式**（命令行） 中直接输出的效果

#### \__lt__(self,other)
> 小于 

- <

#### \__le__(self,other)
> 小于等于 

- <=

#### \__eq__(self,other)
> 等于

- ==

#### \__ne__(self,other)
> 不等于

- !=

#### \__gt__(self,other)
> 大于

- >

#### \__ge__(self,other)
> 大于等于

- \>=

```python
# 定义 > < >= <= ==

class Person:
	def __lt__(self,other):
		return True
```

#### \__add__(self,other)
> +

#### \__sub__(self,other)
> -

#### \__mul__(self,other)
> *

#### \__len(self)__
> 获取对象长度

#### \__bool(self)__
> 定义使用 **bool()** 返回值

### 模块(module)
> 将一个完整程序分解为一个个小模块，将模块组合，搭建一个完整程序

**重要特性**:`复用`

#### 模块创建
> 一个文件为一个模块

- 模块名要符合标识符规范
- 在主模块中相当于变量

#### 引用外部模块
1. **import 模块名**
	- 在多次引用同一个模块，但只包含一个实例
2. **import 模块名 as 别名**

通过 **moduleName.\__name__** 可以获取模块名字
- 一个程序只有一个主模块 **\__main__**

#### 包(package)
> 模块中代码过多，或一个模块分解为多个模块
- 包：文件夹
- 模块：.py文件


 - 包中必须有 **\__init__** 文件，包含包中主要内容

 **from packageName import moduleName**:引入包中指定模块

**\__pycache__**:模块缓存文件
> 使用包时将模块代码转换为机器码，在编译一次后，将代码保存到缓存文件中，避免重复编译

#### python标准库

##### sys
> 提供变量和函数，可以获取到Python解析器信息
>
> 或者通过函数操作解析器

```python
import sys

# sys.argv
# 获取执行代码时，命令行所包含的参数
# 返回列表，保存当前命令的所有参数

# sys.modules
# 获取当前程序中引入的所有模块
# 返回字典，key:模块名字，value:模块内容

# sys.path
# 返回列表，保存模块的搜索路径
# 当前目录优先

# sys.platform
# 表示当前Python运行平台

# sys.exit()
# 退出当前程
```

##### pprint
```python
import pprint

# pprint()
# 对打印数据做简单格式化
# pprint.pprint()
```

##### os
> 操作系统模块，对操作系统进行访问

```python
import os

# os.environ[]
# 获取系统的环境

# os.system()
# 执行操作系统的命令
# os.system('dir')
```

<div style="page-break-after:always"></div>

## 异常

> 程序运行时：
> > 使用没有赋值变量
> > 使用不存在索引值
> > 除0
>
> 此时在异常处终止

### 处理异常
```python
try:
	代码块(出现错误语句)
except:
	代码块(出现错误处理方式)
else:
	代码块(没出错执行语句)
finally:
	代码块(最后都会执行语句)
```

### 异常抛出
1. 在函数中出现的异常进行处理，则不会继续传播
2. 若未处理，继续向调用处传播，直到全局作用域
3. 全局作用域为处理，程序终止，显示异常信息

---

- 所有异常信息会被保存到异常对象中
- 异常传播是，将异常对象抛给调用处 


### 异常对象
| 异常类            | 含义             |
| :---------------- | :--------------- |
| ZeroDivisionError | 表示除0异常      |
| NameError         | 处理变量错误异常 |
| IndexError        | 索引异常         |

**捕获异常对象**
```python
try:
	代码块
except 异常类型:
	# 加上异常类型，只捕获特定类型异常
	代码块
except 异常类型：
	pass
except Exception as 异常名:
	# except后不加内容，捕获到所有异常
	pass
finally:
	#无论是否出现异常，该子句都会执行
	pass
```

**Exception**
- 所有异常的父类

#### 语法
- try必须有
- except和finally至少有一个

### 自定义异常对象
- 使用 **raise** 抛出异常
	- raise 异常类或异常实例(异常信息)

- 所有异常都必须是 **Exception** 子类
```python
class MyError(Exception):
	pass
```

<div style="page-break-after:always"></div>

## 文件

- I/O（Input/Output）

**文件操作步骤**

- 打开文件
- 对文件进行操作
- 保存
- 关闭文件

### 打开文件
> 使用 **open** 函数打开文件
>
> - 返回一个代表打开文件的对象

	open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

- file：要打开的文件路径
	- 需要注意相对路径与绝对路径
	- 在 *Windows* 下，可以用 / 代替 \
	- 或者使用 \\\ 代替 \\
	- 或者使用原始字符串 **r"str"**
	- 使用 **..** 返回上一级目录
	- 如果目标文件距离当前文件较远，使用绝对路径

```python
try:
    fileName = "demo.txt"
    fileObj = open(fileName)  # 打开fileName文件
    print(fileObj.read())

    fileObj.close()
except Exception as identifier:
    print('文件不存在')
```

### 关闭文件
- **close()** 方法关闭文件

```python
# with...as
with open(fileName) as file_obj:
	# 在with语句中可以直接使用file_obj做文件操作
	pass
```

### 文件读取
- **read** 方法用来读取文件的内容，会将内容保存为一个字符串返回
- 默认为纯文本文件格式打开，编码为**none**

```python
try:
    fileName = ''
    with open(fileName) as fileObj:
       content = fileObj.read()

       print(content)
except FileNotFoundError as identifier:
    print(f"{fileName}文件不存在")
```

#### 读取文件格式
1. 纯文本文件(使用utf-8编写的文本文件)
2. 二进制文件(图片，mp3，ppt)

#### 读取中文文件
- 指定文件编码为 **utf-8**

```python
try:
    fileName = 'demo.txt'
    with open(fileName,encoding='utf-8') as fileObj:
       content = fileObj.read()

       print(content)
except FileNotFoundError as identifier:
    print(f"{fileName}文件不存在")
```

#### 读取大文件
- read(size=-1)
	- 指定要读字符数量，默认值为-1，读取文件中所有字符
	- 字符数量少于指定数量，全部读出
	- read()会记录上次读取位置
	- 读取到文件末尾，返回''(空串)

```python
try:
    fileName = 'demo.txt'
    with open(fileName,encoding='utf-8') as fileObj:
       chunk = 100
       fileContent = ""
       while True:
            content = fileObj.read(chunk)

            # 检查是否读取到了内容
            if not content:
                # 内容读取完毕
                break

            fileContent+=content  
            print("本次读取到的内容"+content,end="")
except FileNotFoundError as identifier:
    print(f"{fileName}文件不存在")
```
![Alt text](python(函数与面向对象).assets\1587300054150.png)

#### readline()&readlines()
1. readline() 读取一行内容
- *readline()* 自带换行
2. readlines() 
- 一次读取到的内容返回，封装到一个列表中返回

```python
try:
    fileName = 'demo.txt'
    with open(fileName,encoding='utf-8') as fileObj:
       # readline() 读取一行内容
       print(fileObj.readline())

       print(fileObj.readlines())
except FileNotFoundError as identifier:
    print(f"{fileName}文件不存在")
```
![Alt text](python(函数与面向对象).assets\1587300412647.png)

#### 直接遍历文件对象
```python
fileObj = open(fileName)
for t in fileObj:
	print(t)
```

### 文件写入
> **write()** 

- 使用 **open()** 打开文件，需要指定要做的操作
	- 不指定默认为读取文件
	- r：表示只读
	- w：表示可写
		- 文件不存在，创建文件
		- 文件存在，覆盖原文件
	- a：表示追加内容
		- 文件不存在，创建内容
		- 文件存在，向文件追加内容
	- +：表示增加权限
		- r+： 即可读又可写，文件不存在报错
		- w+
		- a+
	- x：用来新建文件
		- 不存在，则创建
		- 存在，则报错
- 写入文本文件，传递一个字符串作为参数
- 返回写入字符的个数

```python
import pprint

try:
    fileName = 'demo.txt'
    with open(fileName,'w',encoding='utf-8',) as fileObj:
       # write()向文件写入内容
       fileObj.write("Hello World\n")
       fileObj.write("Auspice Vinson")

    with open(fileName,'r',encoding="utf-8") as fileObj:
        for t in fileObj:
               print(t)
except FileNotFoundError as identifier:
    print(f"{fileName}文件不存在")
```
![Alt text](python(函数与面向对象).assets\1587301610953.png)

### 二进制文件
**open()**参数
- p
	- 读取文本文件
- b
	- 读取二进制文件
	- 以字节读取

```python
# 读取filename中的文件，写入新对象中
with open(filename,'rb') as fileobj:
	with open(fileName,'wb') as fileObj:
		chunk = 1024*1024
		while True:
			content = fileObj.read(chunk)
			if not content:
				break
		
		fileObj.write(content)
```

### seek()&tell()
- **fileObj.tell()** 查看当前读取位置
- **fileObj.seek()** 切换读取位置
	- arc1,切换的位置
	- arc2,计算位置方式
		- 0：从头计算，默认
		- 1：从当前位置计算
		- 2：从最后位置开始计算

### 其他操作
- 需要引入 **os**

```python
import os

# 返回列表，元素为目录中每个文件名
r = os.listdir()

# 获取当前所在的目录
r = os.getcwd()

# 切换当前目录 cd
r = os.chdir()

# 创建目录
os.mkdir()

# 删除目录
os.rmdir()

# 删除文件
os.remove()

# 重命名,移动文件
os.rename(oldName,newName)
```