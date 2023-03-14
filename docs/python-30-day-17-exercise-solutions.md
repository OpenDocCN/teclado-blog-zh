# 第 17 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-17-exercise-solutions/>

以下是我们对 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[天 17 练习](/30-days-of-python/python-30-day-17-args-kwargs)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)创建一个函数，该函数接受任意数量的数字作为位置参数，并打印这些数字的和。

在我们开始之前，记住不要调用我们的新函数`sum`是非常重要的。为什么？因为已经有一个`sum`函数作为 Python 内置的一部分，我们不想通过重用这个名字来使那个函数不可访问！

当我们想要使用与内置函数或关键字相同的名称时，一个常见的惯例是在名称中加下划线，就像这样:`sum_`。如果你愿意，你也可以选择一个完全不同的名字，比如`multi_add`。

让我们去玩吧，因为听起来更有趣。

因为我们想在这里接受任意数量的值，我们知道我们的参数需要`*`操作符，但是我们需要决定如何调用这个参数。

我们当然可以使用`*args`，但是这里我们知道参数是什么。它们是数值或数字。以下任何一种都可以:

```py
`def multi_add(*values):
    pass

def multi_add(*numbers):
    pass` 
```

我将使用第二个选项，因为在我看来，它更加具体。

既然在我们的`numbers`参数中有了这个数字元组，我们就可以将这个值传递给 build in `sum`函数并打印结果。

```py
`def multi_add(*numbers):
    print(sum(numbers))` 
```

注意不要写`print(sum(*numbers))`，因为`sum`需要一个 iterable，而不是很多值。

### 2)创建一个函数，它接受任意数量的位置和关键字参数，并将它们打印回给用户。您的输出应该指出哪些值是作为位置参数提供的，哪些是作为关键字参数提供的。

与上一个练习不同，我们真的不知道我们正在接收的数据是什么，所以这是针对`*args`和`**kwargs`的情况。这里我们需要这两个参数，因为我们希望接受任意数量的位置参数和任意数量的关键字参数。

我将调用我的函数`arg_printer`，但是你可以随意选择你喜欢的名字，只要它很好地描述了这个函数。

```py
`def arg_printer(*args, **kwargs):
    pass` 
```

既然我们已经设置了参数，打印它们就相当简单了。我们只需要这样做:

```py
`def arg_printer(*args, **kwargs):
    print(f"Positional arguments are: {args}")
    print(f"Keyword arguments are: {kwargs}")` 
```

现在，如果我们调用带有一系列随机值作为参数的函数:

```py
`arg_printer(1,  "blue",  [1,  23,  3], height=184, key=lambda x: x ** 2)` 
```

我们得到这个:

```py
`Positional arguments are: (1, 'blue', [1, 23, 3])
Keyword arguments are: {'height': 184, 'key': <function <lambda> at 0x7f1b7c44f1f0>}` 
```

我认为我们可以做得更好。我想在这里完成两件事:

1.  我不希望括号出现在位置参数周围。我只想要一系列逗号分隔的值。
2.  我希望关键字参数是一系列逗号分隔的值，格式如下:`height=184`。

让我们先处理位置论点。我将在这里使用`join`，但是这意味着我们所有的参数都需要是字符串，所以我们需要对参数列表做一些处理。我要带着一种理解来做这件事:

```py
`def arg_printer(*args, **kwargs):
    args = [str(arg) for arg in args]
    print(f"Positional arguments are: {', '.join(args)}")` 
```

这很好，但并不完美，因为在输出中类似于`"1"`和`1`的东西没有区别。这是潜在的误导。

不使用`str`，我将使用`repr`函数，它将为我们提供一种不同类型的字符串表示，与我们实际定义值的方式更加一致。这将保留`1`和`"1"`之间的区别。

你可以在[文档](https://docs.python.org/3/library/functions.html#repr)中找到更多关于`repr`的信息。

```py
`def arg_printer(*args, **kwargs):
    args = [repr(arg) for arg in args]
    print(f"Positional arguments are: {', '.join(args)}")` 
```

现在让我们来看看关键字参数。

我想再次使用 join，所以我要创建一个字符串列表，就像之前一样。这次我将使用`items`方法迭代字典，并将键和值插入到字符串中。

```py
`def arg_printer(*args, **kwargs):
    args = [repr(arg) for arg in args]
    print(f"Positional arguments are: {', '.join(args)}")

    kwargs = [f"{key}={repr(value)}" for key, value in kwargs.items()]` 
```

我再次在这里使用了`repr`,但只是针对值。这使得输出更加符合我们传入关键字参数的方式。

现在我们可以在打印时加入新的`kwargs`列表:

```py
`def arg_printer(*args, **kwargs):
    args = [repr(arg) for arg in args]
    print(f"Positional arguments are: {', '.join(args)}")

    kwargs = [f"{key}={repr(value)}" for key, value in kwargs.items()]
    print(f"Keyword arguments are: {', '.join(kwargs)}")` 
```

使用我们以前的参数集，我们现在得到了更好的输出，如下所示:

```py
`Positional arguments are: 1, 'blue', [1, 23, 3]
Keyword arguments are: height=184, key=<function <lambda> at 0x7f2deaeed700>` 
```

### 3)使用`format`方法和`**`拆包打印以下字典。

```py
`country = {
    "name": "Germany",
    "population": "83 million",
    "capital": "Berlin",
    "currency": "Euro"
}` 
```

在如何实际输出数据方面，我们有很大的自由度，所以我将主要模仿字典格式。

我的模板将看起来像这样:

```py
`country_template = """Name: {name}
Population: {population}
Capital: {capital}
Currency: {currency}"""` 
```

因为我们计划使用`**`解包，我们需要使用命名占位符，因为这是`format`将关键字映射到占位符的方式。

现在我们可以像这样使用模板:

```py
`country = {
    "name": "Germany",
    "population": "83 million",
    "capital": "Berlin",
    "currency": "Euro"
}

country_template = """Name: {name}
Population: {population}
Capital: {capital}
Currency: {currency}"""

print(country_template.format(**country))` 
```

在我的例子中，输出如下:

```py
`Name: Germany
Population: 83 million
Capital: Berlin
Currency: Euro` 
```

如果你想用更多的值来尝试，创建一个国家字典列表，并在一个`for`循环中将你的模板传递给`print`。

### 4)使用`*`拆包和`range`，打印数字`1`到`20`，用逗号分隔。

这里我们必须小心记住的一点是，定义`range`时的停止值是不包含的。这意味着我们需要`range(1, 21)`。

我们实际上可以在一行中完成整个练习，这显示了这种方法的一些强大之处:

```py
`print(*range(1, 21), sep=", ")` 
```

这里我用`*`解包了`range`，把它变成了 20 个独立的整数，并且我使用`sep`参数定义了在这些值之间的字符。我选择的值是一个逗号后跟一个空格。

如果我们运行代码，我们会得到如下输出:

```py
`1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20` 
```

### 5)修改练习`4`中的代码，使每个数字打印在不同的行上。您只能使用一次`print`通话。

我们需要对代码进行的唯一更改是更改我们传递给`sep`的分隔符字符串。我将使用`"\n"`代替逗号和空格，这意味着我们将在每个值之间换行。

```py
`print(*range(1, 21), sep="\n")` 
```

现在当我们打印时，所有的数字都在不同的行上。