# 第 2 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-2-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 2 天练习](/30-days-of-python/python-30-day-2-strings-variables)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)询问用户的姓名和年龄，将这些值赋给两个变量，然后打印出来。

首先，让我们把注意力集中在用户的名字上，因为对于他们的年龄来说，方法是非常相似的。

为了从用户那里获得一个值，我们需要使用`input`函数，提供一个有意义的提示是一个很好的实践，这样用户就知道我们想要什么。

```
`input("Please enter your name: ")` 
```

不要忘记在提示的末尾留一个空格，这样用户就不会直接写在冒号或提示文本的最后一个字母旁边。

下一步是将用户的响应存储在某个地方，所以我将把`input`调用的结果赋给一个名为`name`的变量:

```
`name = input("Please enter your name: ")` 
```

我们遵循同样的方法:

```
`name = input("Please enter your name: ")
age = input("Please enter your age: ")` 
```

现在我们有了两个变量，我们可以通过将变量名传递给`print`来打印值，就像这样:

```
`name = input("Please enter your name: ")
age = input("Please enter your age: ")

print(name)
print(age)` 
```

### 2)调查当你试图给一个已经定义好的变量赋值时会发生什么。尝试在重用该名称之前和之后打印变量。

如果您自己尝试这样做，您应该会看到变量值被替换。例如，如果我们这样写:

我们打印出来的值是`7`，不是`5`。然而，如果我们在给这个名字分配一个新值之前打印`x`,我们得到原始值:

这是因为 Python 自顶向下运行我们的代码，所以当我们在调用`print`时引用`x`，我们还没有更新`x`的值。

### 3)下面你会发现一些有很多错误的代码。尝试一行一行地检查程序，并修复代码中的问题。

我们需要纠正的代码是:

```
`hourly_wage = input("Please enter your hourly wage: ')

prnt("Hourly wage: ")
print(hourlywage)
print("Hours worked: ")
print(hours_worked)

hours_worked = input("How many hours did you work this week? ")` 
```

这一小段代码存在一大堆问题，我强烈建议您充分利用 Python 提供的错误消息来追踪它们。

在第一行，我们对引号有异议。

```
`hourly_wage = input("Please enter your hourly wage: ')` 
```

我们用双引号开始字符串，但是我们试图用单引号结束字符串。引号必须匹配，所以现在无效。我们需要把它改成这样:

```
`hourly_wage = input("Please enter your hourly wage: ")` 
```

几行之后，我们有一个对`print`函数的拼写错误的引用:

我们不小心写了`prnt`，而不是`print`。在这种情况下，错误消息非常有用:

```
`Traceback (most recent call last):
    File "main.py", line 3, in <module>
    prnt("Hourly wage:")
NameError: name 'prnt' is not defined` 
```

一旦我们解决了这个问题，我们还有另一个`NameError`。我们忘记了`hourly_wage`中的下划线:

应该是:

最后，我们还有另一个`NameError`，这个是我们代码的顺序造成的。在我们尝试使用之后，我们已经定义了`hours_worked`。

```
`print(hours_worked)

hours_worked = input("How many hours did you work this week? ")` 
```

记住 Python 是从上到下运行我们的代码的，所以如果它在我们告诉它名字之前遇到了一个名字，它会抱怨未定义的变量。

移动有问题的代码行可以解决问题，我们完整的工作代码可能如下所示:

```
`hourly_wage = input("Please enter your hourly wage: ")
hours_worked = input("How many hours did you work this week? ")

print("Hourly wage: ")
print(hourly_wage)

print("Hours worked: ")
print(hours_worked)` 
```