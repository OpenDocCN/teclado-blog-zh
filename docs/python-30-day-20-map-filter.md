# 第 20 天:映射、过滤和条件理解

> 原文：<https://blog.teclado.com/python-30-day-20-map-filter/>

欢迎来到 Python 系列 [30 天的第 20 天！今天，我们将扩展我们对理解的使用，以纳入过滤条件。这将允许我们从原始值的某个子集创建一个新集合。](https://blog.teclado.com/30-days-of-python/)

我们还将看到两个基于函数的理解语法的替代品，叫做`map`和`filter`。

## 理解的快速回顾

我们已经多次使用理解，所以我不想在这里重述语法。我想说的是的*是什么理解。*

当我们想从其他可重复的事物中收集新的集合时，我们使用理解。然而，有些情况下，我们想做一个新的集合，理解是没有必要的。当我们制作这个新系列的时候，我们只有在想要改变一些价值观的时候才会用到理解。例如，我们可能希望将列表中的每个字符串转换为标题大小写:

```py
`names = ["tom", "richard", "harold"]
names = [name.title() for name in names]` 
```

如果我们只是想把`names`列表变成一个集合，我们可以这样做:

```py
`names = ["tom", "richard", "harold"]
names = set(names)` 
```

我们不必为更加冗长的集合理解而烦恼:

```py
`names = ["tom", "richard", "harold"]
names = {name for name in names}` 
```

考虑到这一点，我们真的可以把理解看作是对某个可重复项中的每一项执行动作，然后存储结果的一种方式。

## `map`功能

现在我们有了这个理解的心智模型，与`map`的相似之处将会非常清楚。

是一个函数，它允许我们对 iterable 中的每一项调用其他函数。什么是功能？它们只是一种定义我们想要以可重用的方式执行的一些动作的方法。换句话说，`map`是对 iterable 中的每一项执行某种动作的方式，就像理解一样。

假设我想对数字列表中的每个数字进行立方运算。我们可以这样使用`map`:

```py
`def cube(number):
    return number ** 3

numbers = [1, 2, 3, 4, 5]
cubed_numbers = map(cube, numbers)` 
```

这里我们应该注意的一点是，我们只传递了我们希望`map`为我们调用的函数的名称。我们自己还没有调用那个函数。

在上面的例子中，我们的函数被称为`cube`，所以我们传入的是`cube`，而不是`cube()`。

你们中的一些人可能试图打印上面例子中的`cubed_numbers`,但是也许你没有得到你想要的。我们得到的不是立方数的列表，而是类似这样的东西:

```py
`<map object at 0x7f8a284ab3d0>` 
```

这是因为`map`对象是另一种*惰性*类型，就像我们通过调用`zip`、`enumerate`或`range`得到的东西。`map`实际上，在我们要求之前，不会计算任何值。

这使得`map`比我们到目前为止看到的理解更有效，因为它不需要一次存储所有的值。它也可能永远不会计算任何值，因为它只在我们请求时计算下一个值。如果我们从不请求这些值，它们就不会被计算！

那么我们如何从一个`map`对象中得到东西呢？它们是可迭代的，所以我们可以迭代这些值:

```py
`def cube(number):
    return number ** 3

numbers = [1, 2, 3, 4, 5]
cubed_numbers = map(cube, numbers)

for number in cubed_numbers:
    print(number)` 
```

因为它们是可迭代的，我们也可以使用`*`来解包它们:

```py
`def cube(number):
    return number ** 3

numbers = [1, 2, 3, 4, 5]
cubed_numbers = map(cube, numbers)

print(*cubed_numbers, sep=", ")` 
```

如果愿意，我们还可以将它们转换为普通收藏:

```py
`def cube(number):
    return number ** 3

numbers = [1, 2, 3, 4, 5]
cubed_numbers = list(map(cube, numbers))` 
```

## `map`具有多个可重复项

关于`map`的一个非常好的事情是它可以同时处理几个可迭代的对象。

当我们提供多个 iterable 时，`map`在调用所提供的函数时从每个 iterable 中取一个值。这意味着函数被多个参数调用。这些参数的顺序与我们将 iterables 传递给`map`的顺序相匹配。

假设我们有两个数字列表:`odds`和`evens`。我想将`odds`中的第一个值添加到`evens`中的第一个值，然后我想添加每个集合的第二个值，以此类推。用`map`很容易做到这一点。

```py
`def add(a, b):
    return a + b

odds = [1, 3, 5, 7, 9]
evens = [2, 4, 6, 8, 10]

totals = map(add, odds, evens)
print(*totals, sep=", ")  # 3, 7, 11, 15, 19` 
```

注意，`add`函数需要能够接受多个值，因为`map`总是用两个参数调用它:一个来自`odds`的值，一个来自`evens`。

如果的 iterables 长度不同，`map`将在用完值后立即停止，就像使用`zip`时一样。

## `map`用λ表示

`map`经常用于简单的操作，这意味着通常不值得定义一个完整的函数。Lambda 表达式经常被使用，因为它们允许我们在调用`map`时定义一个内联函数。

例如，我们可以使用 lambda 表达式重新创建上面的`cube`示例，如下所示:

```py
`numbers = [1, 2, 3, 4, 5]
cubed_numbers = map(lambda number: number ** 3, numbers)` 
```

如果你需要一个关于 lambda 表达式如何工作的提示，我们在第 16 天的中已经介绍过了。

## `operator`模块

虽然 lambda 表达式很棒，但我们经常最终使用 lambda 表达式来复制某些操作符的功能。例如，`lambda number: number ** 3`实际上只是对每个值使用`**`操作符的一种方式。

由于这种 lambda 表达式非常常见，标准库中有一个名为`operator`的模块，它包含所有操作符的函数版本。它还包含了一些函数，使调用方法或访问集合中的值变得容易。

让我们再来看看我们的`add`例子。

```py
`def add(a, b):
    return a + b

odds = [1, 3, 5, 7, 9]
evens = [2, 4, 6, 8, 10]

totals = map(add, odds, evens)
print(*totals, sep=", ")  # 3, 7, 11, 15, 19` 
```

我们可以像这样使用 lambda 表达式:

```py
`odds = [1, 3, 5, 7, 9]
evens = [2, 4, 6, 8, 10]

totals = map(lambda a, b: a + b, odds, evens)
print(*totals, sep=", ")  # 3, 7, 11, 15, 19` 
```

在我看来，这有点乱。这不像只写`add`那么清楚，但是如果不是必须的话，我也不太想必须定义`add`。

`operator`有一个`add`函数已经准备好了，所以我们不必自己定义它。

```py
`from operator import add

odds = [1, 3, 5, 7, 9]
evens = [2, 4, 6, 8, 10]

totals = map(add, odds, evens)
print(*totals, sep=", ")  # 3, 7, 11, 15, 19` 
```

`operator`中另一个真正有用的函数是`methodcaller`。`methodcaller`允许我们轻松定义一个为我们调用方法的函数。我们只需要提供一个字符串形式的方法名。

例如，我们可以对列表中的每个名字调用`title`方法，如下所示:

```py
`from operator import methodcaller

names = ["tom", "richard", "harold"]
title_names = map(methodcaller("title"), names)` 
```

在我看来，这比 lambda 表达式方法好得多:

```py
`names = ["tom", "richard", "harold"]
title_names = map(lambda name: name.title(), names)` 
```

在`operator`模块中有很多有趣的东西，所以我推荐你看一看[文档](https://docs.python.org/3/library/operator.html)。

## 条件理解

正如我在这篇文章开始时所说的，我们不仅仅可以使用理解对 iterable 中的每一项执行操作，我们还可以使用理解进行过滤。

我们通过在理解的末尾提供一个条件来做到这一点，这个条件决定了一个项目是否能进入我们的新集合。在条件评估为`True`的情况下，添加项目；否则，它将被丢弃。

例如，假设我们有一组数字，我只想要偶数。我们可以使用条件理解来实现这一点:

```py
`numbers = [1, 56, 3, 5, 24, 19, 88, 37]
even_numbers = [number for number in numbers if number % 2 == 0]` 
```

这里我的过滤条件是`if number % 2 == 0`。因此，我们将把任何被`2`整除的数相加，并留下`0`的余数。

这种理解相当于这样写一个`for`循环:

```py
`numbers = [1, 56, 3, 5, 24, 19, 88, 37]
even_numbers = []

for number in numbers:
    if number % 2 == 0:
        even_numbers.append(number)` 
```

我们可以对任何一种理解进行过滤操作，例如，我们可以对一组理解做同样的事情。

```py
`numbers = [1, 56, 3, 5, 24, 19, 88, 37]
even_numbers = {number for number in numbers if number % 2 == 0}` 
```

作为创建新集合的一部分，我们还可以对值进行修改。过滤条件是通常理解语法的扩充，它不限制我们通常能做的任何事情。

## `filter`功能

就像`map`是“正常”理解的功能模拟一样，`filter`扮演着与条件理解相同的角色。

与`map`非常相似，`filter`为 iterable 中的每一项调用一个函数(称为谓词)，并丢弃该函数返回 falsy 值的任何值。

一个**谓词**是一个接受某个值作为参数并返回`True`或`False`的函数。

例如，我们可以这样改写上面例子中的条件理解:

```py
`numbers = [1, 56, 3, 5, 24, 19, 88, 37]
even_numbers = filter(lambda number: number % 2 == 0, numbers)` 
```

在这种情况下，我们在`operator`模块中没有简单的解决方案——尽管有一个`mod`函数——所以我们要么使用 lambda 表达式，要么定义一个要调用的函数。

在这种情况下，我认为定义一个小的助手函数是值得的，因为它使事情更具可读性。

```py
`def is_even(number):
    return number % 2 == 0

numbers = [1, 56, 3, 5, 24, 19, 88, 37]
even_numbers = filter(is_even, numbers)` 
```

就像`map`，`filter`给了我们一个懒惰的`filter`对象，所以这些值直到我们需要的时候才会被计算出来。

然而，与`map`不同的是，`filter`函数一次只能处理一个 iterable。不过这并不是一个大问题，因为我们有许多不同的方式将集合组合在一起，我们可以将这个组合的集合传递给`filter`。

## 使用`None`和`filter`

除了向`filter`传递函数，还可以使用值`None`。这告诉`filter`我们想要直接使用值的真值，而不是执行某种比较，或者计算什么。

在这种情况下，`filter`将保留原始 iterable 中的所有真值，而所有假值将被丢弃。

```py
`values = [0, "Hello", [], {}, 435, -4.2, ""]
truthy_values = filter(None, values)

print(*truthy_values, sep=", ")  # Hello, 435, -4.2` 
```

## 练习

1)使用`map`调用下面列表中每个字符串的`strip`方法:

```py
`humpty_dumpty = [
    "  Humpty Dumpty sat on a wall,  ",
    "Humpty Dumpty had a great fall;     ",
    "  All the king's horses and all the king's men ",  
    "    Couldn't put Humpty together again."
]` 
```

在控制台的不同行上打印童谣的行。

记住，如果你愿意，你可以使用`operator`模块和`methodcaller`函数来代替 lambda 表达式。

2)下面你会发现一个包含几个名字的元组:

```py
`names = ("bob", "Christopher", "Rachel", "MICHAEL", "jessika", "francine")` 
```

使用带有过滤条件的列表理解，以便只有少于 8 个字符的名称出现在新列表中。确保新列表中的每个名字都是大写的。

3)使用`filter`删除以下范围内的所有负数:`range(-5, 11)`。将剩余的数字打印到控制台。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-20-exercise-solutions)

## 额外资源

在`itertools`模块中有一个`filter`的版本叫做`filterfalse`。它就像`filter`一样工作，但是只保留谓词返回 falsy 值的项目。

本质上，它给了我们一套`filter`会丢弃的价值。

你可以在这里找到`filterfalse` [的文档。](https://docs.python.org/3.8/library/itertools.html#itertools.filterfalse)