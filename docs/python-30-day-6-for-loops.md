# 第 6 天:For Loops 

> 原文：<https://blog.teclado.com/python-30-day-6-for-loops/>

欢迎来到 Python 系列 [30 天的第 6 天！在本帖中，我们将讨论`for`循环，它允许我们对集合中的每个项目重复一次动作。](https://blog.teclado.com/30-days-of-python/)

如果你错过了[第 5 天](/30-days-of-python/python-30-day-5-conditionals-booleans)，如果你不熟悉以下任何话题，或者如果你需要复习，我建议你查看一下:

*   布尔值
*   Python 中的真值和假值
*   比较运算符
*   `if`报表
*   `elif`和`else`条款
*   使用真值作为条件

此外，我们还制作了第 4 天和第 5 天的简短回顾，您可以在此处查看来刷新您的记忆。

## 为什么我们需要循环？

让我们用一个我们以前的帖子中的例子，我们定义了一个表示电影库中电影的元组列表。

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]` 
```

这里我们有一个名为`movies`的列表，它包含许多元组。这些元组包含设定顺序的数据:每个元组中的第一个元素是电影标题；第二个要素是电影的导演；最后一个元素是发行年份。

假设我们想打印出这个电影列表的内容。利用我们目前的知识，我们可能会这样做:

```py
`movie = movies[0]
print(f"{movie[0]} ({movie[2]}), by {movie[1]}")

movie = movies[1]
print(f"{movie[0]} ({movie[2]}), by {movie[1]}")

movie = movies[2]
print(f"{movie[0]} ({movie[2]}), by {movie[1]}")` 
```

这将为我们提供如下输出:

```py
`Eternal Sunshine of the Spotless Mind (2004), by Michel Gondry
Memento (2000), by Christopher Nolan
Requiem for a Dream (2000), by Darren Aronofsky` 
```

如果我们想更简洁一点，我们可以放弃`movie`变量，将几个订阅表达式连接在一起:

```py
`print(f"{movies[0][0]} ({movies[0][2]}), by {movies[0][1]}")
print(f"{movies[1][0]} ({movies[1][2]}), by {movies[1][1]}")
print(f"{movies[2][0]} ({movies[2][2]}), by {movies[2][1]}")` 
```

如果这些对你来说是新的，我建议看看第四天的帖子。

我们上面写的代码有一些问题。首先，我们有很多重复的代码，这使得这个程序很难维护。想象一下，如果我们想改变输出:

```py
`Eternal Sunshine of the Spotless Mind (2004), by Michel Gondry` 
```

对此:

```py
`Eternal Sunshine of the Spotless Mind (2004) - Michel Gondry` 
```

我们必须在三个不同的地方做出改变。即使只有这么少的电影，也很容易漏掉一部，或者在格式上犯一个小错误，这可能会导致电影之间的输出出现意想不到的差异。

现在想象我们有 50 部电影。或者 1000 部电影。不仅我们的可维护性问题变得如此糟糕，甚至第一次写出来都有点像噩梦。

我们的代码也非常脆弱。我们无法解释对`movies`列表的任何更改，我们需要提前知道有多少部电影，以便我们可以编写适当数量的`print`调用。

因为这种代码重复带来了如此大的挑战，所以现代编程语言给了我们很多工具来消除这种重复。在这种特殊情况下，我们想要的工具是一个`for`循环。

## 什么是`for`循环？

Python 中的`for`循环是对集合中的每一项，或者更一般地说，对 iterable 中的每一项执行一些操作的一种方式。在一个非常宽泛的意义上，我们可以把一个 iterable 看作是能够一次给我们一个值的东西。

在我们的电影库示例中，`for`循环将允许我们从`movies`列表中一次获取一个项目，并为每部电影执行一个动作(或一系列动作)。在这种情况下，动作是以某种特定的格式打印电影。

以下是一个具体的循环示例，用于以我们之前使用的格式打印`movies`列表中的每部电影:

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]

for movie in movies:
      print(f"{movie[0]} ({movie[2]}), by {movie[1]}")` 
```

如您所见，这种循环非常强大。列表中可能有 10，000 部电影，而我们只用了两行代码就成功地将它们全部打印出来。

## 定义一个`for`循环

循环定义从关键字`for`开始。这个关键字告诉 Python 我们想要定义一个`for`循环。

紧随其后的是一个变量名，但这不是我们在代码中定义的名字。我们一会儿将回到这一点。

接下来是`in`关键字，后面是我们想要从中获取值的 iterable。这一整行都用冒号结尾。

缩进在块下面的是我们想要为 iterable 中的每一项运行的代码。就像条件语句一样，这个缩进很重要，因为它指出了什么代码与`for`循环相关联。这个缩进的块有时被称为循环体。

好的，那么在我们的例子中，`movie`这个名字是怎么回事呢？我们实际上定义了一个新变量作为循环的一部分，这个变量将允许我们引用 iterable 中的一个给定值。

当我们运行一个`for`循环时，我们开始*迭代我们提供的*可迭代对象。这仅仅意味着我们一次从一个可迭代的对象中获取值。

当我们从 iterable 中获得一个新值时，我们将它赋给这个名称，我们将它定义为循环的一部分，然后我们执行循环体中的代码。完成后，我们从 iterable 中获取一个新值，将这个新值赋给循环变量，并使用这个新值再次运行循环体。

一旦我们用完了所有的值，循环就结束了，然后我们继续处理循环后面的代码。

如果我们以电影库为例来考虑这个问题，我们定义了一个名为`movie`的循环变量，我们从中获取值的 iterable 是`movies`列表。

在循环的第一次迭代(循环)中，我们从`movies`中获取第一项，它是这个元组:

```py
`(
    "Eternal Sunshine of the Spotless Mind",
    "Michel Gondry",
    2004
)` 
```

这个元组被赋值给`movie`，然后我们运行循环体中的代码。我们在主体中只有一行代码，看起来像这样:

```py
`print(f"{movie[0]} ({movie[2]}), by {movie[1]}")` 
```

如你所见，我们在这里引用了这个`movie`变量，所以在循环的第一次迭代中，`movie[0]`是字符串，`"Eternal Sunshine of the Spotless Mind"`；`movie[1]`是弦，`"Michel Gondry"`；而`movie[2]`是整数，`2004`。

我们现在已经运行完了循环体中的代码，所以我们尝试从`movies`中获取另一个值。由于列表中还有电影，Python 给了我们下一个元组:

```py
`(
    "Memento",
    "Christopher Nolan",
    2000
)` 
```

这个新的元组被赋值给`movie`，替换旧的值，我们再次运行循环体。

这种情况一次又一次地发生，直到我们看完电影。

需要记住的一件重要事情是，从功能的角度来看，`movie`这个名字并不重要。Python 并没有神奇地将名字`movie`和`movies`联系起来。我们可以做以下任何一项，效果都是一样的:

```py
`for m in movies:
    print(f"{m[0]} ({m[2]}), by {m[1]}")` 
```

```py
`for film in movies:
    print(f"{film[0]} ({film[2]}), by {film[1]}")` 
```

```py
`for movie_details in movies:
    print(f"{movie_details[0]} ({movie_details[2]}), by {movie_details[1]}")` 
```

它只是一个变量名，就像我们定义的其他变量一样。

### 风格注释

虽然我们可以随意命名循环变量，但最好确保它们准确地描述了我们正在处理的数据。

在上面的例子中，每个元组代表关于一部电影的信息，所以像`movie`这样的名字非常合适，也很有描述性。像`x`这样的名字就不是了，它会使我们的代码更难推理，尤其是在更复杂的循环中。

## `break`声明

有时我们不想遍历整个集合，或者我们想在某些情况下停止循环。在这些情况下，我们可以使用名为`break`的语句。

`break`通常与条件语句结合使用，因为否则它将在循环的第一次迭代中运行，这使得循环基本上没有意义。

使用`break`语句的一个常见方法是防止不必要的操作。例如，假设我们想要检查某部电影是否在我们一直引用的电影库中。

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]

for movie in movies:
      # Check the title of the current movie is Memento
      if movie[0] == "Memento":
            # If the title is Memento, inform the user that the movie exists and break the loop
            print("Memento is in the movie library!")
            break` 
```

在这种情况下，我们所关心的是电影是否存在，所以如果它是第一个项目，我们就没有必要继续检查其余的项目。如果有 10，000 部电影，我们就有可能避免检查 9，999 部电影。

记住，为了在 for 循环中包含 if 语句，它需要缩进 4 个空格。因此，if 语句的主体需要缩进 8 个空格，以便同时出现在循环和 if 语句中。

## `range`功能

Python 有一个名为`range`的内置函数，它能够根据某种设定的模式产生一系列整数。例如，我们可能想得到一系列数字，从 1 开始，到 100 结束，以 3 为步长移动。在这种情况下，我们会有`1`、`4`、`7`、`10`、`13`、`16`、`19`等。

我们调用`range`函数就像我们调用`print`或`input`一样，但是`range`可以接受可变数量的值。

定义`range`最简单的方法是为`range`提供停止值:

这将给我们一个整数序列，从零开始，一直到停止值，但不包括停止值。`range(10)`因此会给我们一个这样的序列:

```py
`0, 1, 2, 3, 4, 5, 6, 7, 8, 9` 
```

或者，我们可以提供一个起始值和一个终止值。在这种情况下，起始值首先出现，并且该值包含在范围内。因此`range(10)`和写`range(0, 10)`是一回事。

假设我们出于某种原因希望我们的`range`从 3 开始，我们希望它上升到 10，但不包括 10。我们将这样定义`range`:

其中包含这些值:

如果我们指定了起始值和终止值，我们可以选择指定一个步长值。默认情况下，该值为`1`，这意味着序列中的下一个数字比之前的数字高`1`。

如果我们想得到每隔一个数字，我们可以指定一个步骤`2`。如果我们想要每三个数字，我们将指定一个步骤`3`。

```py
`range(0, 10, 2)  # 0, 2, 4, 6, 8
range(0, 10, 3)  # 0, 3, 6, 9` 
```

我们也可以指定一个负的步长值，这意味着我们从起始值向后移动到终止值。

```py
`range(10, 0, -1)  # 10, 9, 8, 7, 6, 5, 4, 3, 2, 1` 
```

要小心，因为如果我们指定一个`range`，我们不能从一个值到另一个值，`range`不会抱怨:它只会给我们一个空序列。

一个例子可能是:

我们无法按照`-1`的步骤从`0`到达`10`，所以`range`什么也没有给我们，而是一个无限的集合。

当您打印一个`range`时，您可能会看到这样的内容:

```py
`print(range(0, 10))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]` 
```

但实际上那根本不是要印出来的。

关于`range`有趣的一点是，它是一种懒惰的类型。这意味着我们的`range`实际上并不包含这个序列中的所有值。相反，它只是在我们需要的时候计算出下一个数字应该是什么。

这意味着如果你试图打印一个`range`，你不会看到所有的值:

```py
`print(range(1000))  # range(0, 1000)` 
```

我们得到的只是我们生成的`range`的定义。该范围从`0`开始，到`1000`，并且使用默认步长值。这使得`range`的内存效率非常高，因为它不必存储潜在的数百万个数字的集合。

`range`是可迭代的，这意味着我们可以在`for`循环中使用它。这也意味着，如果我们愿意，我们可以将它转换成一个列表或元组。

我们可以使用`list`和`tuple`函数来实现这一点，就像我们使用`int`、`float`和`str`来转换整数、浮点数和字符串一样。

```py
`numbers = list(range(10))             # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
immutable_numbers = tuple(range(10))  # (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)` 
```

## 在`for`循环中使用`range`

有一件事`range`非常有用，那就是运行一个循环一定次数。

如果我们使用类似于`range(10)`的东西生成一个`range`，这个`range`能够提供十个数字。如果我们迭代这个集合，`range`将一次给我们一个数字，我们将为每个数字运行一次循环体。

```py
`for number in range(10):
    print(number)

# 0
# 1
# 2
# ...
# 9` 
```

即使我们不使用生成的数字`range`，我们仍然知道我们将从循环中得到 10 次迭代。

例如，我们可以像这样将一个给定的字符串打印十次:

```py
`for number in range(10):
    print("Hello!")` 
```

输出:

```py
`Hello!
Hello!
Hello!
Hello!
Hello!
Hello!
Hello!
Hello!
Hello!
Hello!` 
```

当我们编写这样一个循环时，我们生成的数字没有被使用，通常的惯例是将循环变量命名为`_`。这对我们代码的读者来说是一个明确的信号，循环变量没有出现在我们的循环体中。

### 风格注释

有时你会遇到以`_`作为循环变量的循环。这是完全合法的，因为`_`是一个有效的变量名，因此 Python 不会抱怨。

虽然这是合法的，但是请不要使用`_`来代替好的描述性名称。它有一个特定的目的，就是向读者表明循环变量实际上并没有在循环体中使用。

使用`_`作为循环变量的最佳时机如下:

```py
`for _ in range(10):
    print("Hello!")` 
```

这里我们只是使用`range`来确保一定数量的迭代。我们从不提及`range`正在生成的数字。这是很好的实践，对于习惯了这种命名约定的读者非常有帮助。

然而，我们不应该在如下所示的循环中使用`_`:

```py
`for _ in range(10):
    print(_)` 
```

这里我们在循环体内引用了`_`,这不仅非常难看，而且还浪费了使用好的描述性名称的机会。

## 练习

1)下面我们提供了一个元组列表，其中每个元组包含一个商店雇员的详细信息:他们的姓名、上周工作的小时数以及他们的时薪。以一种清晰易读的格式打印出每个员工在周末应得的工资。

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]` 
```

2)对于上述员工，打印出时薪高于平均水平的员工。

提示:您可以使用一个 for 循环和两个变量来跟踪工资总额和雇员人数。然后，使用这两个变量计算平均值。最后，添加另一个循环，再次遍历雇员列表，只打印出时薪高于计算平均值的雇员。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-6-exercise-solutions)

## 项目

一旦你完成了这些练习，我们有[另一个迷你项目让你尝试](/30-days-of-python/python-30-day-6-project)！这次我们将解决一个常见的编码面试问题:嘶嘶的嗡嗡声。

## 额外资源

如果你需要提醒自己`range`选项，如果你想要更多关于它的详细信息，你可以在[官方文档](https://docs.python.org/3/library/stdtypes.html#ranges)中查找。

你也可以在这里找到更多关于`for`循环[的信息。](https://docs.python.org/3/reference/compound_stmts.html#the-for-statement)