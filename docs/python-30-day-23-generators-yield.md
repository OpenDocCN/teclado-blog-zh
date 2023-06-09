# 第 23 天:发电机和发电机表达式

> 原文：<https://blog.teclado.com/python-30-day-23-generators-yield/>

欢迎来到 Python 系列 [30 天的第 23 天！今天我们将会看到一些使用生成器和生成器表达式来创建我们自己的迭代器的方法。](https://blog.teclado.com/30-days-of-python/)

我们还将看到一个叫做`iter`的重要函数，它为我们传递给它的任何可迭代对象返回一个迭代器。这将让我们确认我们在昨天的帖子中讨论的许多理论，并且我们也将能够使用它来更深入地理解`for`循环。

这篇文章将在我们昨天的文章的[中讨论的基础上建立大量内容，所以如果你还没有阅读它，我建议你在进一步阅读之前先看一看。](/30-days-of-python/python-30-day-22-iterators)

## `iter`功能

Python 有一个名为`iter`的内置函数，为我们作为参数提供的 iterable 返回一个迭代器。

例如，让我们看一个简单的数字列表，如下所示:

```py
`numbers = [1, 2, 3, 4, 5]` 
```

如果我们将这个数字列表传递给`iter`，我们将得到这个列表的迭代器。

```py
`numbers = [1, 2, 3, 4, 5]
numbers_iter = iter(numbers)

print(numbers_iter)  # <list_iterator object at 0x7f57d138af70>` 
```

在这种情况下，我们得到一个`list_iterator`对象，它将允许我们访问`numbers`中的值。不同的类型有它们自己的迭代器，这些迭代器知道如何从那些可迭代的对象中给我们条目。毕竟，从字典中获取元素与从列表中获取项目有些不同。

我们可以像使用任何其他迭代器一样使用这个`list_iterator`对象。比如我们可以传给`next`。

```py
`numbers = [1, 2, 3, 4, 5]
numbers_iter = iter(numbers)

print(next(numbers_iter))  # 1
print(next(numbers_iter))  # 2` 
```

一个有趣的问题是，当我们在`list_iterator`上调用`iter`时会发生什么？

这是完全合法的，因为`iter`只需要一个 iterable，而所有迭代器都是 iterable。这也产生了一个有趣的效果。

```py
`numbers = [1, 2, 3, 4, 5]
numbers_iter = iter(numbers)

print(numbers_iter is iter(numbers_iter))  # True` 
```

我们发现将`numbers_iter`传递给`iter`函数会导致`iter`返回完全相同的迭代器。乍一看，这似乎很奇怪，但却很有意义。

昨天我们讨论了迭代器，通过它我们可以访问 iterable 中的条目。当我们想要迭代一个 iterable 时，我们需要一个知道如何获取这些值的迭代器。

如果我们请求*迭代器*给我们一个访问这些值的方法，它会主动提供，因为它已经能够做我们想要的事情了。

## 用`iter`复制`for`循环

我们可以用`iter`函数做的一件很酷的事情是复制 Python 的`for`循环的行为。这将让我们一窥`for`循环在幕后真正做了什么。

让我们在这个例子中再次使用我们的数字列表。

```py
`numbers = [1, 2, 3, 4, 5]
numbers_iter = iter(numbers)` 
```

我们有一个迭代器，这是重要的第一步，但是我们还需要一些其他工具来完成这项工作。首先，我们需要一个`while`循环，因为我们想要无限次地循环。其次，我们需要一个`try`语句，这样我们就可以发现一个`StopIteration`异常。

```py
`numbers = [1, 2, 3, 4, 5]
numbers_iter = iter(numbers)

while True:
    try:
        number = next(numbers_iter)
    except StopIteration:
        break
    else:
        print(number)` 
```

就这样，我们用`while`写了一个`for`循环。

我们在`try`中定义了循环变量`number`，在`else`子句中定义了循环体。一旦我们用完了所有的数字，循环就会终止，就像我们看到的`for`循环一样。

这实际上非常接近实际的`for`循环在引擎盖下的工作方式。它为我们想要迭代的对象请求一个迭代器，并调用`next`从迭代器中检索值。当出现`StopIteration`时，Python 通过中断循环来处理该错误。

这不是你应该在你的产品代码中做的事情，但是这是一个有趣的幕后窥视，帮助我们更好地理解我们从第一周开始使用的结构。

## 发电机

让我们暂时抛开`iter`，转到使用*生成器*创建我们自己的迭代器的话题上来。

在 Python 中创建定制迭代器的方法有很多，但大多数都超出了本系列的范围。不过，这并不是很大的限制，我们可以使用生成器做大量非常复杂的事情。

生成器的语法对我们来说会非常熟悉，因为生成器实际上只是一个函数。生成器与常规函数的唯一区别是一个叫做`yield`的特殊关键字。

在我们深入这个新的`yield`关键字之前，让我们看一个简单的生成器例子。

```py
`def first_hundred():
    for number in range(1, 101):
        yield number` 
```

这里我定义了一个生成器，这只是一个特殊的函数，我把它叫做`first_hundred`。

我们可以从函数体中看出，它与数字`1`到`100`有关，我们大概可以推断它会给我们前一百个整数，从`1`开始。

让我们调用我们的函数，看看会发生什么。

```py
`def first_hundred():
    for number in range(1, 101):
        yield number

g = first_hundred()
print(g)` 
```

如果您运行这段代码，我们肯定不会得到任何类似于打印到控制台上的数字`1`到`100`的东西。我们得到这个`generator`物体:

```py
`<generator object first_hundred at 0x7faaa563fc80>` 
```

这实际上被称为*生成器迭代器*，当我们调用任何包含`yield`关键字的函数时，都会返回这个迭代器。

顾名思义，这是一个迭代器，我们可以像使用其他迭代器一样使用它。

```py
`def first_hundred():
    for number in range(1, 101):
        yield number

g = first_hundred()

print(next(g))  # 1
print(next(g))  # 2
print(next(g))  # 3` 
```

### 重要的

当我们调用一个生成器时，它会返回一个新的生成器迭代器。这些生成器迭代器中的每一个都是独立的迭代器，所以要小心不要这样做:

```py
`def first_hundred():
    for number in range(1, 101):
        yield number

print(next(first_hundred()))  # 1
print(next(first_hundred()))  # 1
print(next(first_hundred()))  # 1` 
```

对`first_hundred`的每次调用都给了我们一个新的迭代器，所以我们只能从每个迭代器中获得第一个值。你也没有在任何地方分配迭代器，所以我们不可能在同一个迭代器上再次调用`next`。

## `yield`关键字

既然我们已经看到了一个运行中的生成器，是时候讨论一下这个`yield`关键字在做什么了。

我们已经知道，它向 Python 发出了我们正在定义一个生成器的信号，但它似乎也在实际提供我们想要的来自生成的生成器迭代器的值方面有一些作用。

`yield`实际上做的是在函数体的执行中创建一个暂停。当我们调用`next`并传入我们的生成器迭代器时，函数体中的代码将一直运行，直到我们点击了那个`yield`关键字。

关键字`yield`之后的值是我们在暂停函数体执行之前实际想要提供的。这样，我们可以认为`yield`是一个非终结性的`return`语句。

我们可以通过给`first_hundred`添加几个`print`调用来看到这一切。

```py
`def first_hundred():
    print("First value requested\n")

    for number in range(1, 101):
        print("Starting new iteration")
        yield number
        print("Ending this iteration\n")

g = first_hundred()` 
```

此时，不会打印任何内容。生成器迭代器已经创建，但是我们实际上还没有尝试访问任何值。现在让我们将`g`传递到`next`几次。

```py
`def first_hundred():
    print("First value requested\n")

    for number in range(1, 101):
        print("Starting new iteration")
        yield number
        print("Ending this iteration\n")

g = first_hundred()

print(next(g))
print(next(g))` 
```

现在我们的输出看起来像这样:

```py
`First value requested

Starting new iteration
1
Ending this iteration

Starting new iteration
2` 
```

首先我们得到`"First value requested\n"`字符串，然后我们进入`for`循环。此时，我们从`range`对象获得一个值，假设为`number`，并打印出`"Starting new iteration"`字符串。

然后我们遇到暂停函数体执行的关键字`yield`，我们的生成器迭代器抛出`1`，这是`number`的当前值。这个值是通过调用`next`返回的，我们将它打印到控制台。

然后我们再次调用`next`，并从我们停止的地方继续。这意味着我们打印了`"Ending this iteration\n"`字符串，然后我们进入了`for`循环的新一轮迭代。

我们在循环开始时再次调用`print`函数，然后再次点击`yield`。我们给出这个数字，这是`next`再次返回的。然后像以前一样，将它打印到控制台。

对于第二次迭代，你会注意到我们没有打印`"Ending this iteration\n"`字符串，因为`yield`在我们到达那个点之前暂停了执行。

如果我们再次调用`next`，我们将在开始循环的第三次迭代之前，首先打印这个字符串。

### 注意

`yield`实际上是一个非常复杂的关键字，它能做的远不止我们使用它的目的。然而，我们不打算在本系列中讨论这种额外的行为，因为它只在更高级的代码中有应用。

我提到这一点只是为了让您知道，一旦您在 Python 职业生涯中走得更远，还有更多的东西要学。

## 生成器表达式

除了通过函数创建生成器迭代器，我们还可以使用*生成器表达式*。

生成器表达式语法对我们来说也非常熟悉，因为它与我们在 [day 15](/30-days-of-python/python-30-day-15-comprehensions/) 中所说的理解语法完全相同。唯一的区别是我们使用常规括号，而不是方括号或花括号。

我们可以像理解一样使用它们，但是它们具有迭代器的所有优点，这些优点是`map`和`filter`提供的。如果您想获得这些好处，但不喜欢`map`和`filter`的语法，生成器表达式正适合您。

例如，让我们创建一个简单的生成器表达式，它对一个`range`中的每个数字求平方。

```py
`squares = (number ** 2 for number in range(1, 11))` 
```

由于`squares`指的是迭代器，直接打印出来并不会给我们带来太多有用的东西，但是它至少确认了我们正在使用生成器迭代器。

```py
`<generator object <genexpr> at 0x7f33225a0c80>` 
```

如果我们想要得到值，我们可以将它传递给一个`for`循环，我们可以析构它，或者我们可以使用`next`来执行手动迭代。

```py
`squares = (number ** 2 for number in range(1, 11))

for square in squares:
    print(square)

squares = (number ** 2 for number in range(1, 11))

print(*squares, sep=", ")

squares = (number ** 2 for number in range(1, 11))

print(next(squares))  # 1
print(next(squares))  # 4
print(next(squares))  # 9` 
```

请记住，当我们迭代迭代器时，`squares`中的值会被消耗掉，所以如果您想不止一次地迭代它，您需要重新定义`squares`。

### 风格注释

关于生成器表达式的一个好处是，当我们在函数或方法中使用生成器表达式作为唯一参数时，我们可以放弃圆括号。

这是完全合法的语法，例如:

```py
`total = sum(number ** 2  for number in  range(1,  11))
print(total)  # 385` 
```

这有助于我们减少只会妨碍可读性的嵌套括号。

## 练习

1)编写一个生成器，生成指定范围内的素数。你可以从第 8 天的[开始，利用你的解决方案进行练习 3。](/30-days-of-python/python-30-day-8-while-loops/)

2)下面我们有一个使用`map`处理列表中名字的例子。使用生成器表达式重写这段代码。

```py
`names = [" rick", " MORTY  ", "beth ", "Summer", "jerRy    "]
names = map(lambda name: name.strip().title(), names)` 
```

3)编写一个小程序，为德州扑克游戏发牌。交易顺序如下:

*   这副牌洗好了。
*   每个玩家按顺序拿到一张牌。
*   第二张卡交给每个玩家。

接下来是交易中更复杂的部分。

*   首先，该副牌的顶牌被丢弃。这被称为烧伤。
*   然后将三张牌放在桌子中央，这被称为*翻牌*。
*   另一张牌被烧掉了，这意味着我们从牌堆的顶部再弃一张牌。
*   我们在中心增加了另一张牌，叫做*转*。
*   我们再烧一张卡。
*   最后，还有*河*，第五张也是最后一张牌被添加到中间。

该程序的预期输出如下所示:

```py
`How many players are there? 2

Player 1 was dealt: (4, hearts), (4, clubs)
Player 2 was dealt: (9, clubs), (jack, diamonds)

The flop: (jack, clubs), (4, diamonds), (king, spades)
The turn: (8, hearts)
The river: (ace, hearts)` 
```

正如示例所示，程序应该接受可变数量的玩家。至少要有 2 个玩家，不要超过 10 个。

在翻牌圈、转牌圈和河牌圈之后，通常会有一轮下注，所以如果你想延长这个练习，你可以让玩家选择在每一点暂停。

提示:我们可以使用`random.shuffle`方法洗牌。这会原地打乱序列，这意味着它会修改原始序列。然后，我们可以使用`iter`从该序列创建一个迭代器，使我们一次检索一张卡片变得容易。

你可以在这里找到`random.shuffle` [的文档。](https://docs.python.org/3/library/random.html#random.shuffle)