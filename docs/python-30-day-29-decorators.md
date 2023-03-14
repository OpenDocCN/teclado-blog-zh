# 第 29 天:装饰工| Teclado

> 原文：<https://blog.teclado.com/python-30-day-29-decorators/>

欢迎来到 Python 系列 [30 天的第 29 天！今天我们将学习关于装修工的知识。](https://blog.teclado.com/30-days-of-python/)

这是一个相对高级的话题，也可能是我们在本系列中涉及的最棘手的事情之一。不过这是值得努力的，因为 decorators 是一个非常强大的工具，它们在专业 Python 代码中有很多特性。

很多库也提供了 decorators 供我们使用，所以我们至少需要知道它们是什么，以及如何使用它们。例如，Decorators 在 Flask 的 web 开发中被大量使用。

## 什么是室内设计师

从根本上说，装饰器只是一个函数，或者更一般地说是一个可调用的函数。在这篇文章中，我们将把重点放在函数上，因为它们是我们唯一知道的可调用函数。

装饰器是特殊的函数，因为它们接受一些其他的函数，并返回给我们一个新的函数，这个函数比我们传入的函数能做更多的事情。

装饰器非常有用，因为它们允许我们非常容易地为应用程序中的许多功能提供一些额外的功能。由于它们的功能和灵活性，它们在标准库模块和第三方库中被广泛使用。

## 函数的快速回顾

当使用 decorators 时，我们必须清楚地记住引用一个函数和调用一个函数之间的区别。

让我们定义一个简单的函数来演示这两个概念之间的区别。

```py
`def add(a, b):
    return a + b` 
```

当我们像这样定义一个函数时，Python 会创建一个可调用的函数对象。换句话说，我们可以使用这个对象让*调用*函数。此对象被分配给一个与函数同名的变量。

我们以前看过第 13 天的[中的`globals()`。](/30-days-of-python/python-30-day-13-return-statements/)

如果我们在定义了这个`add`函数之后查看全局名称空间，我们会看到类似这样的内容:

```py
`{
    '__name__': '__main__',
    '__doc__': None,
    '__package__': None,
    '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x7f2286f6c590>,
    '__spec__': None,
    '__annotations__': {},
    '__builtins__': <module 'builtins' (built-in)>,
    '__file__': 'app.py',
    '__cached__': None,
    'add': <function add at 0x7f2286f2be60>
}` 
```

正如我们所看到的，名称空间包含一个名为`add`的变量，这个变量的值是一个名为`add`的函数对象。这个函数对象包含 Python 运行我们的`add`函数所需的所有信息。

每当我们写`add`时，我们只是引用这个函数对象。这只是一个和其他值一样的值，因为函数在 Python 语言中是[一等公民](/30-days-of-python/python-30-day-16-lambda-expressions/)。

这和我们写`add()`的时候很不一样。这里我们首先引用函数来获得函数对象，然后括号告诉 Python 我们想要*调用*我们刚刚引用的函数。

`add`和`add()`都是表达式，但是第一个表达式计算的是一个函数对象，第二个表达式计算的是当我们调用它时函数返回的任何内容。

## 定义简单的装饰器

让我们从一个例子开始，我们可以马上解构它。

```py
`from typing import Callable

def example_decorator(func: Callable) -> Callable:
    def inner():
        print(f"Now calling {func.__name__}...")
        func()
        print(f"{func.__name__} has ended.")

    return inner` 
```

### 风格注释

我在这里导入了`typing`模块，这样我们可以更好地看到什么进入了这个`example_decorator`函数，以及它返回了什么。正如我们所见，在这两种情况下，它都是一个`Callable`，或者更具体地说，是一个函数。

我们不需要`typing`模块来使用 decorators。

那么，这`example_decorator`到底是怎么回事呢？

从第一行开始，我们可以看到，在这种情况下，我们有一个名为`func`的参数。正如我们已经讨论过的，我们期望传递给`func`的参数是一个函数。到目前为止，一切顺利。

现在我们到了`example_decorator`的函数体，事情变得有趣了。在函数体内，我们定义了一个新函数。这个新功能叫做`inner`，先明确一下，这个名字并不特别。

这个`inner`函数在被调用时做一些事情。

首先它向控制台打印一行，然后它调用`func`，这是我们传递给`example_decorator`的函数的别名，然后它向控制台打印另一行。如果你想知道，`.__name__`只是一个属性，我们可以用它来找到一个函数的名字。

但是请注意，我们实际上并不调用`inner`。我们刚刚定义了这个新函数。

在我们定义完`inner`之后，我们从`example_decorator`返回一个对`inner`的引用。

让我们来看看这个装饰器的运行情况，以便更好地了解正在发生的事情。要使用装饰器，我们可以这样做:

```py
`from typing import Callable

def example_decorator(func: Callable) -> Callable:
    def inner():
        print(f"Now calling {func.__name__}...")
        func()
        print(f"{func.__name__} has ended.")

    return inner

def greeter():
    print("Hello!")

greeter = example_decorator(greeter)
greeter()` 
```

这里我们定义了另一个名为`greeter`的函数，当我们调用它时，它会将`"Hello!"`打印到控制台。

所有真正的行动都在这条线上进行:

```py
`greeter = example_decorator(greeter)` 
```

在这里，我们调用了我们的`example_decorator`函数，并且传递了一个*引用*到`greeter`。然后我们将`example_decorator`的返回值赋给`greeter`，替换对旧的`greeter`函数的引用。

换句话说，`greeter`这个名字现在指的是完全不同的东西。

当我们现在调用`greeter`时，我们得到这个:

```py
`Now calling greeter...
Hello!
greeter has ended.` 
```

如果我们打印`greeter`来找出它的值，我们看到它不再引用旧的`greeter`函数。

```py
`<function example_decorator.<locals>.inner at 0x7f6e06326830>` 
```

这很有意义，因为我们将`example_decorator`的返回值赋给了`greeter`，`example_decorator`的返回值是对`inner`的引用。

这意味着当我们使用名称`greeter`时，我们现在谈论的是`inner`函数，它解释了我们看到的输出。我们得到了我们在`inner`中定义的东西。

我们在控制台上打印了一行；我们调用了`func`，这种情况下是旧的`greeter`函数；然后我们打印了另一行到控制台。

## `@`语法

在我们看更多的 decorator 例子之前，让我们先来讨论一个让使用 decorator 变得更加容易的语法。

在上一个例子中，我们必须这样做:

```py
`greeter = example_decorator(greeter)` 
```

在我看来，这真的很笨拙，谢天谢地，Python 作者给出了一种更好的方法来完成这种操作。

我们可以不做这种赋值，而是通过在`@`后面加上修饰名来修饰一个函数。这个就放在我们要装饰的功能的正上方。

这与我们之前所做的完全相同:

```py
`from typing import Callable

def example_decorator(func: Callable) -> Callable:
    def inner():
        print(f"Now calling {func.__name__}...")
        func()
        print(f"{func.__name__} has ended.")

    return inner

@example_decorator
def greeter():
    print("Hello!")

greeter()` 
```

## 用参数修饰函数

到目前为止，我们已经修饰了一个不带任何参数的函数，但是我们如何像这样修饰一个函数呢？

```py
`def add(a, b):
    print(a + b)` 
```

如果我们尝试我们的老方法，我们将以一个`TypeError`结束，因为当我们在内部函数中调用`func`时，我们一直在无参数地调用它。

为了解决这个问题，我们需要为我们的`inner`函数定义一组参数，并且我们可以在调用`func`时传递参数。

这里我们必须克服的一个问题是，我们的装饰器应该非常通用，这样我们就可以在许多不同的功能中重用它们。因此，我们不应该这样做:

```py
`from typing import Callable, Union

Real = Union[int, float]

def calculate(func: Callable) -> Callable:
    def inner(a, b):
        print("Calculating...") 
        func(a, b)

    return inner

@calculate
def add(a: Real, b: Real):
    print(a + b)

add(1, 5)` 
```

这是可行的，但是只有当函数有两个参数时才可行。如果我们想执行一元运算，或者我们想装饰类似于`sum`的东西，我们需要一个不同的装饰器。

幸运的是，我们已经了解了一些适合这项工作的完美工具:`*args`和`**kwargs`。

使用`*args`和`**kwargs`，我们可以为`inner`接受任何一组我们喜欢的参数，然后我们可以将它们传递给`func`。

```py
`from typing import Callable, Union

Real = Union[int, float]

def calculate(func: Callable) -> Callable:
    def inner(*args, **kwargs):
        print("Calculating...") 
        func(*args, **kwargs)

    return inner

@calculate
def add(a: Real, b: Real):
    print(a + b)

add(1, 5)` 
```

如果你需要复习一下`*args`和`**kwargs`，再看看[第 17 天](/30-days-of-python/python-30-day-17-args-kwargs/)。

## 用返回值装饰函数

既然我们已经处理了带参数的函数，我们现在有了另一个问题。如果我们正在修饰的函数返回值呢？

目前，我们只是将它返回到`inner`中，但是我们不会对这个值做任何事情。我们不会传递这种价值观。幸运的是，这是一个非常容易解决的问题:我们只需要返回来自`inner`的值。

例如，这里有一个非常愚蠢的装饰者给了我们错误的计算答案:

```py
`from typing import Callable, Union

Real = Union[int, float]

def wrong_answer(func: Callable) -> Callable:
    def inner(*args, **kwargs):
        return func(*args, **kwargs) + 1

    return inner

@wrong_answer
def add(a: Real, b: Real) -> Real:
    return a + b

print(add(1, 5))  # 7` 
```

## `wraps`功能

在这篇文章中我想说的最后一件事是标准库中一个叫做`wraps`的函数，它在`functools`模块中。我们使用`wraps`来装饰我们的`inner`函数，它的工作是保存我们传递给装饰器的函数的原始名称。

如果您还记得我们最初的例子，引用变量名`greeter`会得到类似这样的结果:

```py
`<function example_decorator.<locals>.inner at 0x7f6e06326830>` 
```

我们原来的`greeter`函数的名字完全没了。

使用`wraps`，我们可以保留原始的函数名，更重要的是，该函数中包含的任何文档。

严格来说不是装饰工:这是一家装饰厂。这意味着它是一个返回装饰器的函数。这不是我们需要担心的细节，但这确实意味着语法略有不同，因为`wraps`能够接受自己的参数。

当我们调用`wraps`时，我们使用相同的`@`语法，并且我们传入`func`作为参数。这里有一个例子:

```py
`from functools import wraps
from typing import Callable

def example_decorator(func: Callable) -> Callable:
    @wraps(func)
    def inner():
        print(f"Now calling {func.__name__}...")
        func()
        print(f"{func.__name__} has ended.")

    return inner

def greeter():
    print("Hello!")

greeter = example_decorator(greeter)
print(greeter)  # <function greeter at 0x7fcce99a8830>` 
```

如你所见，我们从`example_decorator`返回的函数仍然被称为`greeter`，尽管我们实际上返回了`inner`函数。

## 真实世界的例子

现在我们已经学习了如何修饰各种类型的函数，让我们看一个更实际的例子。我们将创建一个装饰器来为我们的代码计时。

这是一件很方便的事情，因为它允许我们测试几个实现，看看哪个选项更快。

为了实际测试我们的代码，我们将使用`time`模块，它包含一个名为`perf_counter`的函数。`perf_counter`将给我们一个非常详细的当前时间的再现。

通过调用`perf_counter`，运行我们的代码，并再次调用`perf_counter`，我们可以很好地了解两次调用`perf_counter`之间经过的时间。

下面是一个可能的实现:

```py
`from functools import wraps
from time import perf_counter
from typing import Callable

def stopwatch(func: Callable) -> Callable:
    @wraps(func)
    def inner(*args, **kwargs):
        start_time = perf_counter()
        func(*args, **kwargs)
        stop_time = perf_counter()

        print(f"{func.__name__} ran in {stop_time  -  start_time:.5f}s")

    return inner` 
```

现在我们用这个来比较一些代码。我将比较 Python 从 100，000 个数字范围制作一个列表需要多长时间，以及对一个集合做同样的事情需要多长时间。

```py
`from functools import wraps
from time import perf_counter
from typing import Callable, List, Set

def stopwatch(func: Callable) -> Callable:
    @wraps(func)
    def inner(*args, **kwargs):
        start_time = perf_counter()
        func(*args, **kwargs)
        stop_time = perf_counter()

        print(f"{func.__name__} ran in {stop_time  -  start_time:.5f}s")

    return inner

@stopwatch
def make_list(size: int) -> List:
    return list(range(size))

@stopwatch
def make_set(size: int) -> Set:
    return set(range(size))

make_list(100_000)  # make_list ran in 0.00407s
make_set(100_000)   # make_set ran in 0.00628s` 
```

如果您多次运行这个测试，您可能会发现这些操作所用的时间有很大的波动。这是完全正常的，也是意料之中的，因为你的电脑同时在运行各种其他的进程。

如果我们的性能计数器运行测试一定次数，并计算平均时间，可能会更好。

最好的方法是使用装饰工厂，这将允许我们向装饰者传递参数，但是使用我们目前的知识，我们至少可以进行 10 次测试。

```py
`from functools import wraps
from math import fsum
from time import perf_counter
from typing import Callable, List, Set

def stopwatch(func: Callable) -> Callable:
    @wraps(func)
    def inner(*args, **kwargs):
        times = []

        for _ in range(10):
            start_time = perf_counter()
            func(*args, **kwargs)
            stop_time = perf_counter()

            elapsed = stop_time - start_time
            times.append(elapsed)

        average_time = fsum(times) / len(times)

        print(f"{func.__name__} ran in {average_time:.5f}s on average")

    return inner

@stopwatch
def make_list(size: int) -> List:
    return list(range(size))

@stopwatch
def make_set(size: int) -> Set:
    return set(range(size))

make_list(100_000)  # make_list ran in 0.00337s on average
make_set(100_000)   # make_set ran in 0.00565s on average` 
```

现在，我们的性能测试应该看到更少的波动，让我们更好地了解性能差异。

## 练习

制作一个调用给定函数两次的装饰器。你可以假设这些函数不返回任何重要的东西，但是它们可以接受参数。

假设您有一个名为`books`的列表，您的应用程序中有几个函数与它进行交互。写一个 decorator，使得你的函数只有在`books`不为空时才运行。

编写一个名为`printer`的装饰器，让任何被装饰的函数打印它们的返回值。如果给定函数的返回值是`None`，`printer`应该什么都不做。

您可以在这里找到我们的解决方案[。](/30-days-of-python/python-30-day-29-exercise-solutions)

## 额外资源

我们实际上有一个关于装饰者的完整系列，你可以在我们的 T2 YouTube 频道查看，以获得更多信息和看到更多例子。

另外，如果你正在寻找一个更健壮的选项来测试你的代码，你应该看看`timeit`模块。你可以在这里找到`timeit`T2 的文档。