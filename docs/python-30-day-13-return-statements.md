# 第 13 天:作用域和函数返回值| Teclado

> 原文：<https://blog.teclado.com/python-30-day-13-return-statements/>

大家好，欢迎来到 Python 系列 [30 天的第 13 天！今天我们将开始学习*范围*这个非常重要的话题。](https://blog.teclado.com/30-days-of-python/)

我们可以把作用域看作是在应用程序中引用给定名称的描述。理解这在 Python 中是如何工作的是我们学习旅程中至关重要的一步。

我们还将扩展我们使用功能的方式。到目前为止，我们所有的函数都只是打印了一些东西到控制台，但是有些情况下我们也想从函数中获取一些东西。我们将在今天的帖子中讨论如何做到这一点。

## 范围的演示

正如我已经提到的，作用域是一个概念，它描述了在我们的应用程序中可以引用给定名称的地方。

我们还没有遇到过我们定义的名字不可访问的情况，所以让我们看一个例子。

首先，我们将定义一个名为`greet`的简单函数。

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)` 
```

所有的`greet`做的就是接受一个名字并打印一个问候给用户。在`greet`中，我们定义了一个名为`greeting`的变量，在这里我们分配格式化字符串，我们将这个`greeting`字符串作为参数传递给`print`。

问题是，我们能在函数之外访问这个`greeting`字符串吗？

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)

print(greeting)` 
```

在这种情况下，我们似乎得到了一个错误:

```py
`Traceback (most recent call last):
  File "main.py", line 6, in <module>
    print(greeting)
NameError: name 'greeting' is not defined` 
```

但也许这不是一个公平的例子。毕竟，我们从未真正调用过`greet`，所以定义`greeting`的代码从未真正运行过。怎么会呢？我们甚至还没有告诉它`name`是什么意思。

也许在我们调用`greet`之后这个会起作用。

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)

greet("Phil")
print(greeting)` 
```

现在我们的输出看起来像这样:

```py
`Hello, Phil!
Traceback (most recent call last):
  File "main.py", line 7, in <module>
    print(greeting)
NameError: name 'greeting' is not defined` 
```

正如我们所看到的，当`greet`被调用时，`"Hello, Phil!"`被打印，但是`greeting`仍然是未定义的。这是怎么回事？

问题实际上是`greeting`是“超出范围”的。

## 名称空间

为了更好地理解上面例子中发生的事情，我们需要考虑当我们定义一个变量时会发生什么。

Python 实际上记录了我们定义的变量，以及与这些名称相关的值。我们把这个记录叫做名称空间，你可以把它想象成一个字典。我们可以通过调用`globals`函数并打印结果来稍微查看一下这本字典。

首先，让我们看看在空文件中调用`globals`会发生什么。

如果您运行这段代码，您可能会惊讶地发现这个字典不是空的。我们有各种各样有趣的东西在里面。

```py
`{
    '__name__': '__main__',
    '__doc__': None,
    '__package__': None,
    '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x7f2b30020bb0>,
    '__spec__': None,
    '__annotations__': {},
    '__builtins__': <module 'builtins' (built-in)>,
    '__file__': 'main.py',
    '__cached__': None
}` 
```

虽然这有一定的道理。毕竟，我们不需要定义我们在 Python 中使用的每一个名字。例如，我们从未定义过`print`、`input`或`len`。

现在不要太担心这本字典的实际内容。只要记下已经存在的东西，这样我们就可以看到当我们定义一些自己的名字时会发生什么。

让我们改变我们的文件，使它实际上有一些内容。我将定义几个变量和一个函数，这样我们就有了一点多样性:

```py
`names = ["Mike", "Fiona", "Patrick"]
x = 53657

def add(a, b):
    print(a, b)

print(globals())` 
```

现在让我们看看我们的字典里有什么。请特别注意最后三项。

```py
`{
    '__name__': '__main__',
    '__doc__': None,
    '__package__': None,
    '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x7fae511ffbb0>,
    '__spec__': None,
    '__annotations__': {},
    '__builtins__': <module 'builtins' (built-in)>,
    '__file__': 'main.py',
    '__cached__': None,
    'names': ['Mike', 'Fiona', 'Patrick'],
    'x': 53657,
    'add': <function add at 0x7fae512021f0>
}` 
```

我们可以看到我们定义的名字(`names`、`x`和`add`)已经被写成了键，并且与我们的每个名字相关联的是我们赋予那个名字的值。

`add`看起来有点奇怪，但是这个`<function add at 0x7fae512021f0>`只是我们定义的函数的一个表示。我们可以看到这是一个函数，它的名字是`add`，最后一个数字是这个函数的内存地址。

当我们在应用程序中使用一个名称时，Python 只是在名称空间中查看它是否被定义。如果是，它可以只引用与该名称相关联的值。如果它找不到我们请求的名字，Python 会说这个变量是未定义的。

## 函数和命名空间

回头看看我们的`globals`输出，有几个名字明显不存在:我们为`add`定义的参数`a`和`b`。也许你希望它们和我们定义的其他名字放在一起。那些怎么了？

Python 实际上有不止一个名称空间。我们在这里看到的只是*全局名称空间*，但是函数的参数不是这个全局名称空间的一部分。

当我们调用一个函数时，Python 会创建一个新的名称空间。换句话说，它创建了一个新的字典来存储我们在运行这个函数时想要使用的任何名字。一旦函数运行完毕，这个名称空间就被销毁了，所以当我们下次运行这个函数时，我们是在一张白纸上工作。

就像我们使用`globals`时一样，我们可以通过调用`locals`来窥见这个函数名称空间。让我们对`add`做一个小小的改动来看看这个:

```py
`def add(a, b):
    print(locals())
    print(a, b)

add(7, 25)` 
```

在这种情况下，我们得到的输出如下:

第一行是`locals`字典，包含这个函数调用的名称空间中的名字。

现在我们可以更好地解释这篇文章前面的`greet`例子中发生了什么。

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)` 
```

全局名称空间不包含名称`greeting`，所以当我们试图在函数外部引用它时，Python 找不到它。然而，在`greet`内部，我们使用的是调用函数时创建的第二个本地名称空间。这个名称空间包含`greeting`，因为我们在函数体中定义`greeting`时添加了它。

通过这样做，我们可以更详细地了解这一点:

```py
`def greet(name):
    print(locals())
    greeting = f"Hello, {name}!"
    print(locals())
    print(greeting)

greet("Phil")` 
```

如果我们运行这段代码，我们会看到以下内容:

```py
`{'name': 'Phil'}
{'name': 'Phil', 'greeting': 'Hello, Phil!'}
Hello, Phil!` 
```

我们可以看到，当我们为这个函数调用进入函数体时，我们只定义了`name`。然后我们移动到第二行，在那里`greeting`被赋予字符串插值的结果。当我们在函数体的第三行再次打印`locals()`时，我们可以看到`greeting`已经被添加到这个函数调用的本地名称空间中。

我认为这有一些直观的意义。这个函数有一些只有它自己知道的私有变量，我们有一个变量名和值的表，这个表是我们在运行函数时创建的，用来跟踪函数内部的情况。当我们完成这个函数时，我们把表清空，因为我们不再需要这些值。

## 从函数中获取值

因为我们在函数中定义的所有变量只存在于函数内部，问题是，我们如何从函数中得到一个值？例如，`input`函数能够向我们提供用户对我们的提示的响应。我们希望能够做类似的事情。

我们从函数中获取值的方法是使用一个`return`语句。

`return`语句实际上有两个作用。首先，它用来结束一个函数的执行。当 Python 遇到`return`关键字时，它会立即终止函数。

例如，如果我们写这样的东西:

```py
`def my_func():
    return
    print("This line will never run")` 
```

我们永远不会在函数体的第二行运行`print`调用。在我们使用`print`调用到达那一行之前，`return`关键字将导致函数终止。

我们也可以直接在关键字`return`后面放一个表达式，这就是我们从函数中获取值的方式。我们把想要得到的值放在与关键字`return`相同的行上。

例如，让我们根据昨天的练习编写一个新版本的`add`函数。我们不打印结果，而是返回结果。

```py
`def add(a, b):
    return a + b` 
```

如果我们运行这个函数，似乎什么也不会发生。毕竟，我们不再印刷任何东西了。然而，如果我们愿意，我们现在可以将返回值赋给一个变量。

```py
`def add(a, b):
    return a + b

result = add(5, 12)
print(result)  # 17` 
```

那么，这里到底发生了什么？

记住函数调用是一个表达式。它评估为某个值。什么价值？当我们调用一个函数时，该函数调用计算出该函数返回的值。

在上面的例子中，我们返回了表达式的结果，`a + b`。因为`a`是`5`，而`b`是`12`，所以那个表达式的结果是整数`17`。这是函数返回的内容，这是函数调用评估的值。

顺便说一下，因为`add(5, 12)`只是一个表达式，我们本可以直接将函数调用传递给`print`:

```py
`def add(a, b):
    return a + b

print(add(5, 12))  # 17` 
```

但是，当我们没有一个`return`语句时，该怎么办呢？如果我们不指定要返回的值，Python *隐式地*为我们返回`None`。

例如，如果我们试图找到我们的`greet`函数调用的值，我们得到`None`:

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)

print(greet('Phil'))` 
```

我们得到的输出是:

首先我们得到调用函数的结果，因为 Python 需要调用函数来计算出`greet('Phil')`的值是多少；然后我们打印函数的返回值，就是`None`。

如果您键入不带值的`return`,也会发生同样的情况。Python 会返回`None`。

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)
    return

print(greet('Phil'))` 
```

最后，记住我们可以在任何可能使用普通旧值的地方使用函数的返回值。例如，在变量赋值中或作为字符串的一部分:

```py
`def greet(name):
    greeting = f"Hello, {name}!"
    print(greeting)

print(f"The value of greet('Phil') is {greet('Phil')}.")` 
```

这里我们得到一个字符串，告诉我们`greet('Phil')`的值是`None`:

```py
`Hello, Phil!
The value of greet('Phil') is None.` 
```

## 多个`return`语句

有时一个函数定义可能有不止一个`return`语句。这是完全合法的，但是只有当我们有某种条件逻辑将我们导向某个`return`语句时才有用。记住，一旦我们遇到任何`return`语句，函数就会终止，所以如果我们有不止一个串联的语句，我们永远不会遇到第一个之后的语句。

多个`return`语句有意义的一个例子是我们的`divide`函数:

```py
`def divide(a, b):
    if b == 0:
        return "You can't divide by 0!"
    else:
        return a / b` 
```

这是有意义的，因为虽然我们有多个`return`语句，但是我们的条件逻辑只将我们导向一个`return`语句。

因为`return`会导致函数调用终止，我们可以对上面的代码做一点小小的修改:

```py
`def divide(a, b):
    if b == 0:
        return "You can't divide by 0!"

    return a / b` 
```

这仍然有效，因为在任何情况下，当`b`是`0`时，我们点击这个`return`语句并脱离这个函数。因此，我们从未触及执行计算的点。我们到达那里的唯一方法是如果`b`不是`0`。

这是人们用来保存编写这个`else`子句的一个非常常见的模式。尽管加入并没有什么害处，并且可以随意将它包含在您自己的代码中。你觉得舒服就行。

## 练习

1)定义一个接受两个数字的`exponentiate`函数。第一个是基数，第二个是把基数提高到多少的功率。该函数应该返回该操作的结果。记住，我们可以使用`**`操作符来执行取幂运算。

2)定义一个`process_string`函数，该函数接收一个字符串并返回一个新的字符串，该字符串已被转换为小写，并已删除任何多余的空格。

3)编写一个函数，该函数接收一个包含演员信息的元组，并将该数据作为字典返回。数据应该采用以下格式:

```py
`("Tom Hardy", "English", 42)  # name, nationality, age` 
```

您可以为字典选择任何您喜欢的关键字名称。

4)编写一个函数，该函数接受一个数字，并根据该数字是否为质数返回`True`或`False`。如果你需要复习如何计算一个数是否是质数，我们将在本系列的第 8 天展示一种方法。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-13-exercise-solutions)