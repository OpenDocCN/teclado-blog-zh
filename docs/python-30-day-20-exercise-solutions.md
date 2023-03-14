# 第 20 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-20-exercise-solutions/>

以下是我们为 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 20 天练习](/30-days-of-python/python-30-day-20-map-filter)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)使用`map`调用下面列表中每个字符串的`strip`方法。在控制台的不同行上打印童谣的行。

```py
`humpty_dumpty = [
    "  Humpty Dumpty sat on a wall,  ",
    "Humpty Dumpty had a great fall;     ",
    "  All the king's horses and all the king's men ",  
    "    Couldn't put Humpty together again."
]` 
```

首先，让我们通过定义一个单独的函数在给定的行上执行 strip 来解决这个问题。我准备叫我的`line_stripper`，不过你爱怎么叫都行。

```py
`def line_stripper(line):
    return line.strip()` 
```

现在我们可以编写我们的`map`调用了。当调用`map`时，我们需要首先传入我们想要调用的函数，然后传入我们想要从中获取参数的 iterables。在这种情况下，我们只有一个可迭代的:`humpty_dumpty`。

然后我们可以解包`map`对象并将其传递给`print`。

```py
`def line_stripper(line):
    return line.strip()

humpty_dumpty = [
    "  Humpty Dumpty sat on a wall,  ",
    "Humpty Dumpty had a great fall;     ",
    "  All the king's horses and all the king's men ",  
    "    Couldn't put Humpty together again."
]

print(*map(line_stripper, humpty_dumpty), sep="\n")` 
```

如果您愿意，您也可以使用一个`for`循环来打印这些值。

除了定义这个`line_stripper`函数，我们还可以使用 lambda 表达式:

```py
`humpty_dumpty = [
    "  Humpty Dumpty sat on a wall,  ",
    "Humpty Dumpty had a great fall;     ",
    "  All the king's horses and all the king's men ",  
    "    Couldn't put Humpty together again."
]

print(*map(lambda line: line.strip(), humpty_dumpty), sep="\n")` 
```

但是如果我们不想用 lambda 定义函数，我们也可以使用来自`operator`模块的`methodcaller`函数。

```py
`from operator import methodcaller

humpty_dumpty = [
    "  Humpty Dumpty sat on a wall,  ",
    "Humpty Dumpty had a great fall;     ",
    "  All the king's horses and all the king's men ",  
    "    Couldn't put Humpty together again."
]

print(*map(methodcaller("strip"), humpty_dumpty), sep="\n")` 
```

这些方法中的任何一种都非常好。

### 2)下面你会发现一个包含几个名字的元组。使用带有过滤条件的列表理解，以便只有少于 8 个字符的名称出现在新列表中。确保新列表中的每个名字都是大写的。

```py
`names = ("bob", "Christopher", "Rachel", "MICHAEL", "jessika", "francine")` 
```

首先，让我们创建一个理解，把名字变成小写。一旦我们知道过滤条件有效，我们就可以添加过滤条件。

```py
`names = ("bob", "Christopher", "Rachel", "MICHAEL", "jessika", "francine")
names = [name.title() for name in names]` 
```

快速打印的`names`显示我们没有任何问题，所以现在我们需要添加我们的过滤器。在这种情况下，我们希望检查名称是否少于 8 个字符。

我们可以使用`len`函数找出一个字符串中有多少个字符，并且我们可以使用`<`将结果与`8`进行比较。

```py
`names = ("bob", "Christopher", "Rachel", "MICHAEL", "jessika", "francine")
names = [name.title() for name in names if len(name) < 8]` 
```

如果你想知道我们如何用`map`和`filter`复制这样的东西，我们可以像这样将调用`filter`的结果传递给`map`:

```py
`from operator import methodcaller

names = ("bob", "Christopher", "Rachel", "MICHAEL", "jessika", "francine")
names_title = map(methodcaller("title"), filter(lambda name: len(name) < 8, names))` 
```

对于像这样的情况，我们需要执行修改，我们需要做一些过滤，理解语法通常更整洁，更容易理解。

### 3)使用`filter`删除以下范围内的所有负数:`range(-5, 11)`。将剩余的数字打印到控制台。

如果我们愿意，我们实际上可以在一行中这样做:

```py
`print(*filter(lambda number: number >= 0, range(-5, 11)))` 
```

这里我们说的是，对于`range(-5, 11)`中的每个数字，如果它大于`0`，就保留这个数字。然后我们使用`*`解包`filter`对象，这样我们就可以访问我们保存的值。

这是极其简洁的；然而，在这种情况下，我认为为我们的`filter`谓词创建一个单独的函数很有意义。

```py
`def is_positive(number):
    return number >= 0

print(*filter(is_positive, range(-5, 11)))` 
```