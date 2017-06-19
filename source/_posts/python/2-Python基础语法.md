---
title: 2 Python基础语法
date: 2017-06-19 11:51:25
category: python
tags: [python,语法]
---
## Python 标识符
在 Python 里，标识符有字母、数字、下划线组成。
在 Python 中，所有标识符可以包括英文、数字以及下划线(_)，但不能以数字开头。
Python 中的标识符是区分大小写的。
以下划线开头的标识符是有特殊意义的。以单下划线开头 _foo 的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用 from xxx import * 而导入；
以双下划线开头的 __foo 代表类的私有成员；以双下划线开头和结尾的 __foo__ 代表 Python 里特殊方法专用的标识，如 __init__() 代表类的构造函数。
Python 可以同一行显示多条语句，方法是用分号 ; 分开
<!-- more -->
## Python 保留字符
### 下面的列表显示了在Python中的保留字。这些保留字不能用作常数或变数，或任何其他标识符名称。所有 Python 的关键字只包含小写字母。
|   |   |   |
|:--------:|:--------:|:------:|
|and|exec|not|
|assert|finally|or|
|break|for|pass|
|class|from|print|
|continue|global|raise|
|def|if|return|
|del|import|try|
|elif|in|while|
|else|is|with|
|except|lambda|yield|
## 行和缩进
学习 Python 与其他语言最大的区别就是，Python 的代码块不使用大括号 {} 来控制类，函数以及其他逻辑判断。python 最具特色的就是用缩进来写模块。
缩进的空白数量是可变的，但是所有代码块语句必须包含相同的缩进空白数量，这个必须严格执行。
## 多行语句
Python语句中一般以新行作为为语句的结束符。
但是我们可以使用斜杠（ \）将一行的语句分为多行显示
## Python 引号
Python 可以使用引号( ' )、双引号( " )、三引号( ''' 或 """ ) 来表示字符串，引号的开始与结束必须的相同类型的。
其中三引号可以由多行组成，编写多行文本的快捷语法，常用于文档字符串，在文件的特定地点，被当做注释。
```python
word = 'word'
sentence = "这是一个句子。"
paragraph = """这是一个段落。
包含了多个语句"""
```
##　Python注释
python中单行注释采用 # 开头，多行注释使用三个单引号(''')或三个双引号(""")。
```python
#第一个注释
print("hello，Python") #第二个注释
'''
这是多行注释，使用单引号
这是多行注释，使用单引号
这是多行注释，使用单引号
'''

"""
这是多行注释，使用双引号
这是多行注释，使用双引号
这是多行注释，使用双引号
"""
```
## 等待用户输入
下面的程序执行后就会等待用户输入，按回车键后就会退出：
```python
user=input("\nplease input name!\n")
print("your name is " + user)
```
## 同一行显示多条语句
Python可以在同一行中使用多条语句，语句之间使用分号(;)分割

