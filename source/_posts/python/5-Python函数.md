---
title: 5 Python函数
date: 2017-06-22 15:59:00
category: Python
tags: [Python, 函数]
---
## Python 函数
函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。
函数能提高应用的模块性，和代码的重复利用率。你已经知道Python提供了许多内建函数，比如print()。但你也可以自己创建函数，这被叫做用户自定义函数。
<!-- more -->
## 定义一个函数
你可以定义一个由自己想要功能的函数，以下是简单的规则：
* 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号()。
* 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数。
* 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
* 函数内容以冒号起始，并且缩进。
* return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

### 语法
```python
def functionname( parameters ):
   "函数_文档字符串"
   function_suite
   return [expression]
```
> 默认情况下，参数值和参数名称是按函数声明中定义的的顺序匹配起来的。

### 实例
```python
#定义函数
def printme(str):
	"打印传入的字符串"
	print(str)
	return

#调用函数
printme('调用自定义函数')
```
## 参数传递
在 python 中，类型属于对象，变量是没有类型的：
```python
a=[1,2,3]
a="zh"
```
以上代码中，[1,2,3] 是 List 类型，"zh" 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是 List 类型对象，也可以指向 String 类型对象。
### 可更改(mutable)与不可更改(immutable)对象
在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。
* 不可变类型：变量赋值 a=5 后再赋值 a=10，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。
* 可变类型：变量赋值 la=[1,2,3,4] 后再赋值 la[2]=5 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

### python 函数的参数传递：
* 不可变类型：类似 c++ 的值传递，如 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，不会影响 a 本身。
* 可变类型：类似 c++ 的引用传递，如 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响

> python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

## 参数
以下是调用函数时可使用的正式参数类型：
* 必备参数
* 关键字参数
* 默认参数
* 不定长参数

### 必备参数
必备参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。
调用printme()函数，你必须传入一个参数，不然会出现语法错误
### 关键字参数
关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值。
使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。
以下实例在函数 printInfo() 调用时使用参数名：
```python
def printInfo(name,age=20):
	print('name:', name)
	print('age:',age)

printInfo(age=21,name='zh')
```
### 缺省参数
调用函数时，缺省参数的值如果没有传入，则被认为是默认值。下例会打印默认的age，如果age没有被传入：
```python
def printInfo(name,age=20):
	print('name:', name)
	print('age:',age)

printInfo(name='zh')
```
### 不定长参数
你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述2种参数不同，声明时不会命名。基本语法如下：
```python
def functionname([formal_args,] *var_args_tuple ):
   "函数_文档字符串"
   function_suite
   return [expression]
```
> 加了星号（*）的变量名会存放所有未命名的变量参数。选择不多传参数也可。如下实例：

```python
def printInfo(arg1, *vartuple):
	print("输出：")
	print(arg1)
	for var in vartuple:
		print(var)
	return

printInfo(10)
printInfo(10,60,70)
```
### 匿名函数
python 使用 lambda 来创建匿名函数。
* lambda只是一个表达式，函数体比def简单很多。
* lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
* lambda函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
* 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。

语法
lambda函数的语法只包含一个语句，如下：
```python
lambda [arg1 [,arg2,.....argn]]:expression
```
如下实例：
```python
sum = lambda arg1, arg2: arg1 + arg2

print(sum(1,2))
```

## return 语句
return语句[表达式]退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None
```python
def sum(arg1,arg2):
	total = arg1 + arg2
	return total
print(sum(10,23))
```


