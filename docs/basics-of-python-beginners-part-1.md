# 初学者应该知道的 Python 基础知识——第 1 部分

> 原文：<https://blog.teclado.com/basics-of-python-beginners-part-1/>

编程语言是一种专门设计用来表达计算机可以执行的计算的语言。

编程语言用于创建控制系统行为的程序、表达算法或作为人类交流的一种模式。Python、C、C++和 Java 都有广泛的用途。

Python 是由 Guido Van Rossum 于 1991 年开发的一种高级编程语言。由于其易读性和多用途性，它比其他语言更好。

与 C 或 C++相比，Python 非常适合初学者。它有很少的关键字，定义良好的语法，可以在各种硬件平台上运行，并且在所有平台上都有相同的界面。

Python 是一种面向对象的编程语言，可以将代码封装在对象中。

在 Python 中学习的主要主题是缩进、数据类型、注释、操作符、决策、循环、函数、模块、关键字、oops、库等等。

今天我们将讨论前五个。

## 1.刻痕

在 Python 程序中，逻辑行开头的前导空格决定了缩进级别。

“块内的所有语句都应该在同一缩进级别”。

Python 非常严格地检查缩进级别，如果缩进不正确，就会给出一个错误。

在大多数编程语言中，缩进对程序逻辑没有影响。它用于对齐语句，使代码可读。然而，在 Python 中，缩进是用来关联和分组语句的。

作为一个例子，我们正在创建一个函数`a()`，在这个函数中，你可以看到两个最初带有空格的注释和一个最初带有空格的 pass 语句。这个“初始空间”叫做缩进，这三行是函数内部的代码块。

```py
def a(param1, param2):

    # code line 1 with indentation
    # code line 2 with indentation
    pass 
```

## 2.数据类型

Python 是一种动态类型语言；因此，我们不需要在声明变量时定义它的类型。

"解释器隐式地将值和它的类型绑定在一起.""

数据类型是数据项的分类或归类。它表示一种值，这种值决定了可以对特定数据执行什么操作。

python 中有 5 种数据类型:数值型、序列型、布尔型、集合型和字典型。

**Numeric** :在 Python 中定义为 int、float、complex 类。

*   *int* :全部由整数组成。整数可以是正数，也可以是负数。内置整数的大小是无限的。例子:`-5`、`0`、`890`。
*   *float* :浮点数写成用小数点隔开的十进制数。它们以整数和小数点分隔的小数部分的形式表示。例子:`-5.2`、`11.325`。
*   *复数*:复数以 a+bi 的形式表示，其中 a 为实部，b 为虚部。例子:`4+5i`。

**序列类型**:序列是相似或不同数据类型的有序集合。三种主要的序列类型是字符串、元组和列表。

*   *字符串*:字符串是 Unicode 字符的序列。我们可以用单引号、双引号或三引号来表示字符串。多行字符串可以用三重引号(“”...)来表示')和单行字符串可以用单引号(' ... ')来表示)或双引号("...").
*   *元组*:元组是值的有序集合。元组是不可变的，因此一旦创建就不能修改。它们通常在圆括号`( )`中定义，圆括号中的项目用逗号分隔。元组可以包含任意数量的元素和任意数据类型(如字符串、整数、列表等)。).
*   *列表*:列表是项目的有序序列，类似于元组。它们还表示值的集合，这些值可以是不同的数据类型。列表中的项目用逗号分隔，并用括号`[ ]`括起来。列表是可变的，而元组是不可变的。

**Boolean** : Boolean 是一种数据类型，具有两个内置值`True`或`False`。带有大写字母“T”和“F”的`True`和`False`是有效的布尔值。

**Set** :它是一个无序的值集合，是可迭代的，可变的，没有重复的元素。

**字典**:字典是一个无序的条目集合，结构为键:值对。在字典中，每个键-值对由大括号内的逗号分隔，`{ }`。在字典中，每个键都应该是唯一的。

代码示例:

```py
x = "Hello, world!"  # string
x = 20  # int
x = 20.5  # float
x = 1j  # complex
x = ["values", "separated", "by", "commas"]  # list
x = ("also", "separated", "here")  # tuple
x = {1, 2, 3}  # set
x = {"name": "Bob", "age": 35}  # dictionary
x = True  # boolean 
```

注释用于添加关于程序或其语句的附加内容信息。有单行注释和多行注释。

**单行注释**以一个散列字符(#)开始，后面是包含进一步解释的文本。

代码示例:

```py
x = "The line below is a single-line comment."

# This is a single-line comment in Python! 
```

**多行注释**块也被 Python 理解。这些注释可以作为其他人阅读您的代码并更详细地解释事情的内嵌文档。

代码示例:

```py
"""
This is a Python code example.

This is a multi-line comment that spans 5 lines.
""" 
```

## 4.经营者

Python 运算符是用于执行数学或逻辑运算的符号。操作数是应用运算符的值或变量。

Python 中有几种类型的运算符，让我们详细讨论每一种。

### 4.1.算术运算符

顾名思义，这些运算符用于执行算术运算。

代码示例将显示所有算术运算符以及每个运算符各自的输出。

```py
x = 23
y = 5

# Addition
x + y  # 28

# Subtraction
x - y  # 18

# Multiplication
x *  y  # 115

# Division (always returns a float)
x / y  # 4.6

# Floor division
x // y  # 4

# Modulus (remainder of division)
x % y  # 3

# Power
x ** y  # 6436343 
```

### 4.2.赋值运算符

赋值运算符用于给变量赋值。

这个代码示例将向您展示 Python 中所有赋值操作符的工作方式。

```py
x = 23
x += 5  # 28
x -= 5  # back to 23
x *= 2  # 46
x /= 2  # back to 23
x %= 5  # 3 (23 / 5 has a remainder of 3)
x **= 2  # 9

# There are more! 
```

### 4.3.比较运算符

它们用于比较两个值。

这个代码示例将向您展示每个比较运算符是如何工作的。

```py
x = 5
y = 6
z = 5

x == y  # False
x == z  # True

y > z  # True
x < y  # False

x >= z  # True
x <= z  # True 
```

### 4.4.逻辑运算符

逻辑运算符通常用于组合两个或多个关系语句。我们经常把它们和布尔值一起使用，但是它们也有更多的用途。

展示如何在 Python 中使用逻辑运算符的代码示例:

```py
x = 3
y = 4

x > y and x == 3  # False
x < y and x == 3  # True
x > y or x == 3  # True
not x > y  # True 
```

### 4.5.标识运算符

它们用于比较对象，如果它们是具有相同内存位置的相同对象，则返回`True`。

```py
x = ["apple", "banana"]
y = ["apple", "banana"]
z = x

print(x is z)  # True, they are the same list

print(x is y)  # False, same values but different "thing" 
```

### 4.6.按位运算符

它们用于比较二进制数。您将很少使用这些工具，但知道它们的存在是件好事。

下面的代码摘自[这里的](https://www.geeksforgeeks.org/python-bitwise-operators/)，如果你感兴趣，可以在这里阅读深入的解释。

```py
a = 10  # 1010
b = 4  # 0100

print(a & b)  # 0

print(a | b)  # 14

print(~a)  # -11

print(a ^ b)  # 14 
```

## 5.Python 中的决策制定

我们可以使用“if 语句”来控制程序的执行流程。我们可以给这个语句一个条件，如果条件是`True`，那么它下面的块就会执行。如果条件是`False`，那么程序块将不执行。

在此代码示例中:

*   如果`a`的值大于`5`，那么我们就给它加上`5`。
*   否则，如果`a`等于`4`，那么我们就给它加上`3`。
*   最后，如果上面两个都没有执行，我们就给它加上`2`。

```py
a = 4

if a > 5:
    a += 5
elif a == 4:
    a += 3
else:
    a += 2

print(a)  # 7 
```

## 结论

这就是“初学者应该知道的 Python 基础知识”的第一部分。我也写了一个关于这个话题的小帖子，这个帖子是受这篇文章的启发。

如果你想学习更多关于 Python 的知识，可以考虑参加我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)，它将带你从初级到高级(包括 OOP、web 开发、异步开发等等！).我们有 30 天的退款保证，所以你试一试也不会有什么损失。我们很希望你能来！

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)