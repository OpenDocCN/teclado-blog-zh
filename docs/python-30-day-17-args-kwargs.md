# 第 17 天:带有*args 和**kwargs | Teclado 的灵活函数

> 原文：<https://blog.teclado.com/python-30-day-17-args-kwargs/>

欢迎来到 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列的第 17 天！今天我们将讨论如何使用`*`和`**`操作符来创建真正灵活的函数，可以接受任意数量的参数。

## 问题是

你们中的一些人可能已经注意到，一些内置函数接受可变数量的参数。举个例子，我们来看看好老`print`。

`print`实际上可以接受任意数量的位置参数，默认情况下它会将它们并排打印出来:

```
`print(1, 2, 3, 4, 5)  # 1 2 3 4 5` 
```

放在这些值之间的字符由另一个名为`sep`的参数控制，该参数的默认值为`" "`。这就是为什么我们在每个项目之间有一个空格。

我们可以这样明确地写:

```
`print(1, 2, 3, 4, 5, sep=" ")  # 1 2 3 4 5` 
```

如果你感兴趣的话，我们有[另一篇文章](https://blog.teclado.com/python-a-closer-look-at-print/)，它对`print`有更详细的描述。

虽然这种行为看起来相对简单，但这不是我们现在可以复制的。我们必须为每个参数提供一个参数，反过来，每个参数也必须有一个对应的参数。这意味着我们需要事先知道我们要接受多少值。

那么，我们如何做类似于`print`的事情，我们可以接受用户提供的尽可能多的参数？

## 接受任意数量的位置参数

让我们从位置论点开始，因为这是我们在`print`中观察到的行为。

我们接受任意数量的位置参数的方法是定义一种特殊的参数，它的名字前面有一个`*`。非常明确地说，这个`*`实际上是一个操作符，而不是参数名的一部分。

一般来说我们称这个特殊参数为`args`，是 arguments 的简称。

当我们包含一个`*`参数时，Python 将在我们调用函数时收集任何未赋值的位置参数，并将它们全部放入一个元组中。这个元组被分配给我们的`*`参数。

让我们看一个例子，这样我们就可以看到这一点。

我们将定义一个名为`mul`的函数，它将接受任意两个数字作为参数(有两个参数)，并将它们相乘。然后我们将使用`*args`重新定义它。

```
`def mul(x, y):
    print(x * y)` 
```

这里我们可以这样调用函数，传递两个位置参数。记住这些是位置变量，因为在给参数赋值时，变量的位置很重要。

由于`5`在第一个位置，它被分配给`x`参数。

现在我们将使用`*args`来定义函数。该参数将收集任何没有分配给任何其他对象的位置参数，并将它们转换成一个值元组。现在我们必须这样做:

```
`def mul(*args):
    print(args[0] * args[1])

mul(5, 10)` 
```

很少创建像上面这样的函数，因为`*args`的目的是接受任意数量的参数。因为我们正在访问`args[0]`和`args[1]`，这意味着我们真的想要*两个*参数。所以在那里用`*args`并不是正确的选择。

但是看看另一个函数，它接受任意数量的朋友名字并问候他们:

```
`def multigreet(*args):
    for name in args:
        print(f"Hello, {name}!")

multigreet("Rolf", "Bob", "Anne")` 
```

试着自己调用这个函数，传递尽可能多的名字作为参数。

需要注意的一点是，当我们在循环中引用我们的参数时，我们使用名称`args`，而不是`*args`。永远记住这个`*`是一个运算符。

### 风格注释

正如我之前提到的，当我们想要收集多余的位置参数时，使用参数名`args`是一个惯例；然而，这并不总是正确的选择。

当参数可以是任何东西时，编写`*args`非常有用，但是在上面的函数中，我们非常清楚值代表什么:值应该都是名称。

在`multigreet`的情况下，因此用`*names`代替更合适，就像这样:

```
`def multigreet(*names):
    for name in names:
        print(f"Hello, {name}!")` 
```

有更好的名字就不要用`*args`。请记住，变量名是帮助提高可读性的极好工具，所以请尽可能使用最具描述性的名称。

## 参数顺序同`*args`

当我们使用像`*args`这样的参数时，我们必须非常清楚参数的顺序。这是因为我们在`*args`之后定义的任何参数都不能接受位置参数。

这有些道理。如果我们用我们的`*args`参数收集所有多余的位置参数，那么当我们到达它后面的参数时，什么都没有了。我们剩下的只有关键字参数。

例如，如果我们将我们的`multigreet`函数更改为下面的代码，并尝试调用它，

```
`def multigreet(*names, other):
    for name in names:
        print(f"Hello, {name}!")

multigreet("Jose", "Phil")` 
```

我们得到一个`TypeError`。

```
`Traceback (most recent call last):
  File "main.py", line 6, in <module>
    multigreet("Jose", "Phil")
TypeError: multigreet() missing 1 required keyword-only argument: 'other'` 
```

在这种情况下，我们得到一个非常明确的信息。我们缺少了一个仅包含关键字的参数。

如果我们将`other`参数放在第一位，这个异常就会消失。Python 非常乐意按顺序分配位置参数，然后为我们的`*names`参数收集剩下的内容。

在`*`参数后放置参数也非常有用，因为我们可以强迫用户对某些参数使用关键字参数。这对于我们不希望用户意外更改的内容，或者我们希望建立默认设置的内容非常有用。

`print`实际上利用了仅接受关键字参数的`sep`和`end`。

## 接受任意数量的关键字参数

我们已经讨论了位置参数，现在是时候讨论如何处理任意数量的关键字参数了。如果你想要一个内置函数的例子，我们可以看看`dict`。

我们已经看到了一些调用`dict`的不同方法，但是我们还没有看到的一种方法是使用`dict`和关键字参数创建一个字典。

例如，我们可以这样写:

```
`dict(name="Phil", age=29, city="Budapest", nationality="British")` 
```

在这里，关键字变成了键，我们与每个关键字匹配的值变成了与这些键相关联的值。因此，上面的代码与下面的代码相同:

```
`{
    "name": "Phil",
    "age": 29,
    "city": "Budapest",
    "nationality": "British"
}` 
```

不知道我们想要用多少键和值来创建我们的字典，所以它必须是灵活的，并且接受任意数量的关键字参数。

我们可以用另一个特殊的参数实现同样的事情，这次是在它的名字前面加上`**`。这个参数的常规名称是`**kwargs`，这是关键字参数的缩写。

看一下这个例子，它以一种很好的格式打印出关键字参数:

```
`def pretty_print(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

pretty_print(title="The Matrix", director="Wachowski", year=1999)

# title: The Matrix
# director: Wachowski
# year: 1999` 
```

注意`**kwargs`收集所有未赋值的关键字参数，并用它们创建一个字典，然后可以在函数中使用。这就是为什么我们可以访问那里的`.items()`，因为`kwargs`是一本字典。

如果我们为一个给定的函数定义了`*args`和`**kwargs`，`**kwargs`必须排在第二位。如果顺序颠倒了，Python 认为这是无效的语法。

实际上，`**kwargs`必须是您定义的最后一个参数，因为它必须跟在`*args`以及任何其他关键字参数之后。毕竟，它会收集所有剩余的关键字参数，所以它后面的任何参数都不会被赋值。

### 风格注释

就像用`*args`一样，我们在使用`**kwargs`的时候也不应该被约定俗成所束缚。如果我们知道预期的数据，并且我们可以用另一个名称更有效地描述它，那么您肯定应该这样做。

这种约定存在于定义函数时参数完全未知的情况下。

## `*`和`**`的其他用途

`*`和`**`操作符非常通用，因为我们不仅可以用它们将值收集到一个集合中，我们还可以用它们来做相反的事情:将一个 iterable 解包为单个值。

我们已经在第 9 天看到了解包(也称为析构)，但是我们总是在给固定数量的变量赋值的上下文中这样做。在这些情况下，我们只需将 iterable 提供的值与相同数量的变量进行匹配，剩下的由 Python 处理。

然而，有时这种方法并不总是可行的。例如，如果我们想要析构一个 iterable，这样我们就可以将许多值传递给`*args`？

答案是我们在作为参数传递的 iterable 前面加了一个`*`。

例如，假设我想获取这个数字列表，并在一行中打印这些数字，在每个数字之间使用竖线(`|`)字符。我们可以这样做:

```
`numbers = [1, 2, 3, 4, 5]

print(*numbers, sep=" | ")` 
```

如果我们运行这段代码，我们会得到以下输出:

如果我们想析构一个字典，我们可以做同样的事情，但是记住默认情况下，当我们迭代一个字典时，Python 会给我们键。因此，在使用`*`时，你经常需要使用`values`或`items`方法。

例如，此函数打印一部电影的详细信息:

```
`def print_movie(*args):
    for value in args:
        print(value)

movie = {
    "title": "The Matrix",
    "director": "Wachowski",
    "year": 1999
}

print_movie(*movie.values())

# The Matrix
# Wachowski
# 1999` 
```

这个函数实际上是打印它接收到的参数，每行一个。例如，如果我们传入`movie.keys()`,它将打印键:

```
`print_movie(*movie.keys())

# title
# director
# year` 
```

没那么有用！

我们还可以使用`**`将字典转换成一系列关键字参数。

将上面的函数与`**kwargs`和字典析构一起使用，我们得到:

```
`def print_movie(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

movie = {
    "title": "The Matrix",
    "director": "Wachowski",
    "year": 1999
}

print_movie(**movie)

# title: The Matrix
# director: Wachowski
# year: 1999` 
```

这里，当我们调用函数时，它把字典变成关键字参数。这些被传递给`print_movie`,`**kwargs`参数将它们收集回一个字典中。

你也可以这样做:

```
`def print_movie(movie_details):
    for key, value in movie_details.items():
        print(f"{key}: {value}")

movie = {
    "title": "The Matrix",
    "director": "Wachowski",
    "year": 1999
}

print_movie(movie)

# title: The Matrix
# director: Wachowski
# year: 1999` 
```

这是有效的，但是在收集未赋值的关键字参数时,`**kwargs`可以给我们更多的灵活性，而不仅仅是那些来自字典的参数。

例如，我们可以这样做:

```
`def print_movie(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

movie = {
    "title": "The Matrix",
    "director": "Wachowski",
    "year": 1999
}

print_movie(studio="Warner Bros", **movie)

# studio: Warner Bros
# title: The Matrix
# director: Wachowski
# year: 1999` 
```

在那里，我们使用了`**kwargs`来收集`studio`参数以及所有的`movie`参数！这不是我们只用一个参数就能轻易做到的。

像这样的技术还有很多其他用途，包括合并字典，以及在使用`format`时节省我们大量的打字时间。

例如，让我们回头看一个来自 [day 14 project](/30-days-of-python/python-30-day-14-project-regular/) 的例子，我们需要从字典列表中打印一些关于我们的书籍的输出。

```
`def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print(f"{book['title']}, by {book['author']} ({book['year']})")

    print()` 
```

我们可以使用带有命名占位符的`format`方法，而不是在这里使用 f 字符串。然后我们可以将`**book`传递给`format`。

```
`def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print("{title}, by {author} ({year})".format(**book))

    print()` 
```

这将为每本书创建一系列关键字参数，如下所示:

```
`title="1Q84", author="Haruki Murakami", year="2004"` 
```

这种方法的好处之一是我们可以在其他地方定义模板，这也意味着我们可以在代码的多个地方引用它。

```
`book_template = "{title}, by {author} ({year})"

def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print(book_template.format(**book))

    print()` 
```

您需要在字典中访问的键越多，它也会变得越来越有效。

## 练习

1)创建一个函数，该函数接受任意数量的数字作为位置参数，并打印这些数字的和。记住，我们可以使用`sum`函数将 iterable 中的值相加。

2)创建一个函数，它接受任意数量的位置和关键字参数，并将它们打印回给用户。您的输出应该指出哪些值是作为位置参数提供的，哪些是作为关键字参数提供的。

3)使用`format`方法和`**`拆包打印以下字典。

```
 `country = {
    "name": "Germany",
    "population": "83 million",
    "capital": "Berlin",
    "currency": "Euro"
 }` 
```

4)使用`*`拆包和`range`，打印数字`1`到`20`，用逗号分隔。在这个练习中，你必须为`print`函数的`sep`参数提供一个参数。

5)修改练习`4`中的代码，使每个数字打印在不同的行上。您只能使用一次`print`通话。

## 额外资源

我们有一个关于析构的[专用帖子，在那里我们讨论`*`的另一个用途，这次是在给变量赋值时收集值。](https://blog.teclado.com/destructuring-in-python/)