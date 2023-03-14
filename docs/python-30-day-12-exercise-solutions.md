# 第 12 天锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-12-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 12 天练习](/30-days-of-python/python-30-day-12-functions/)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)定义四个函数:`add`、`subtract`、`divide`和`multiply`。每个函数应该有两个参数，它们应该打印由函数名指示的算术运算的结果。

对于这个问题，我们必须记住几个额外的细节。

首先，对于某些运算，操作数的顺序很重要。毕竟`12 / 4`和`4 / 12`不是一回事，而`5 + 7`和`7 + 5`是完全相同的操作。在操作数的顺序很重要的情况下，我们应该将函数的第一个参数视为左操作数，将第二个参数视为右操作数。

这意味着`12 / 4`将被称为`divide(12, 4)`。

我们必须记住的另一件事是，除以`0`会产生一个异常。如果我们尝试，我们会得到一个`ZeroDivisionError`:

```
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    8 / 0
ZeroDivisionError: division by zero` 
```

让这个错误浮出水面没有错，但是在我们的例子中，我们将检查除法的第二个操作数是否是`0`，我们将向用户打印一条消息，而不是让程序崩溃。

让我们从定义`add`函数开始。您可以为这些函数使用您喜欢的任何参数名称。我将调用操作数`a`和`b`。

```
`def add(a, b):
    print(a + b)` 
```

这里我们定义了两个参数，这意味着`add`需要两个参数。当我们调用函数时，这些参数值被赋给我们的参数，我们可以在函数体中使用参数名来引用这些值。

我们将假设用户传递给我们数字，所以我们可以打印表达式的结果，`a + b`。

`subtract`和`multiply`非常相似，但是我们必须小心保持`subtract`的操作数顺序正确:

```
`def add(a, b):
    print(a + b)

def subtract(a, b):
    print(a - b)

def multiply(a, b):
    print(a * b)` 
```

不要忘记在函数定义之间放两行空行！

`divide`稍微复杂一点，因为我们必须检查第二个参数不是`0`。我们可以用一个简单的条件语句来实现这一点:

```
`def add(a, b):
    print(a + b)

def subtract(a, b):
    print(a - b)

def multiply(a, b):
    print(a * b)

def divide(a, b):
    if b == 0:
        print("You can't divide by 0!")
    else:
        print(a / b)` 
```

你可能在想，如果`b`是`0.0`会怎么样？在这种情况下，条件仍然有效。Python 足够聪明，知道`0.0`和`0`是一回事。

确保测试所有的函数都工作正常，并确保测试了`divide`的零值和非零值。

### 2)定义一个名为`print_show_info`的函数，它只有一个参数。传递给它的参数将是一个字典，其中包含一些关于电视节目的信息。`print_show_info`函数应该以一种很好的方式打印存储在字典中的信息。

本练习的目标是定义一个名为`print_show_info`的函数，它可以接受这样一个字典:

```
`tv_show = {
    "title": "Breaking Bad",
    "seasons": 5,
    "initial_release": 2008
}` 
```

并输出如下字符串:

```
`Breaking Bad (2008) - 5 seasons` 
```

我们的`print_show_info`，它有一个参数，我称之为`show`。`show`想要一个以上格式的字典。

```
`def print_show_info(show):
    pass` 
```

我在这里写了`pass`，意思就是“什么都不做”。当我们在代码中创建需要主体的结构，但是我们还不知道主体应该是什么的时候，这是很有用的。如果我们把它留空，Python 会抱怨，所以`pass`是一个非常有用的占位符。

在这种情况下，我们希望我们的函数以良好的格式打印显示数据，我将使用上面的示例作为模板。我们可以通过访问`show`的键来访问提供的字典中的值。

```
`def print_show_info(show):
    print(f"{show['title']} ({show['initial_release']}) - {show['seasons']} season(s)")` 
```

现在我们只需要调用我们的函数！

```
`def print_show_info(show):
    print(f"{show['title']} ({show['initial_release']}) - {show['seasons']} season(s)")

tv_show = {
    "title": "Breaking Bad",
    "seasons": 5,
    "initial_release": 2008
}

print_show_info(tv_show)` 
```

### 3)以下是包含多部电视连续剧详细信息的列表。使用函数 print_show_info 和一个 for 循环来迭代序列列表，并在每次迭代中调用一次函数，传入每个字典。

```
`series_data = [
    {"title": "Breaking Bad", "seasons": 5, "initial_release": 2008},
    {"title": "Fargo", "seasons": 4, "initial_release": 2014},
    {"title": "Firefly", "seasons": 1, "initial_release": 2002},
    {"title": "Rick and Morty", "seasons": 4, "initial_release": 2013},
    {"title": "True Detective", "seasons": 3, "initial_release": 2014},
    {"title": "Westworld", "seasons": 3, "initial_release": 2016},
]` 
```

因为我们已经定义了函数，所以除了编写实际的循环，我们没有什么要做的。我将使用`show`作为循环变量，因此我的循环定义将如下所示:

```
`for show in series_data:
    pass` 
```

现在我们只需要在循环体中调用我们的函数，将`show`作为参数传入，因为对于循环的每次迭代，我们的一个字典将被分配给`show`:

```
`for show in series_data:
    print_show_info(show)` 
```

因此，完整的解决方案如下所示:

```
`def print_show_info(show):
    print(f"{show['title']} ({show['initial_release']}) - {show['seasons']} season(s)")

series_data = [
    {"title": "Breaking Bad", "seasons": 5, "initial_release": 2008},
    {"title": "Fargo", "seasons": 4, "initial_release": 2014},
    {"title": "Firefly", "seasons": 1, "initial_release": 2002},
    {"title": "Rick and Morty", "seasons": 4, "initial_release": 2013},
    {"title": "True Detective", "seasons": 3, "initial_release": 2014},
    {"title": "Westworld", "seasons": 3, "initial_release": 2016},
]

for show in series_data:
    print_show_info(show)` 
```

这应该会给我们这样的输出:

```
`Breaking Bad (2008) - 5 season(s)
Fargo (2014) - 4 season(s)
Firefly (2002) - 1 season(s)
Rick and Morty (2013) - 4 season(s)
True Detective (2014) - 3 season(s)
Westworld (2016) - 3 season(s)` 
```

### 4)创建一个函数来测试一个单词是否是回文。回文是一串向前或向后阅读都相同的字符。

这个练习有点棘手，实际上是一个相当常见的面试问题。我们有几种不同的方法，我将展示几种不同的方法。

首先，我们需要定义我们的函数，它将接收一个`word`。我们要清理一下这个字符串，去掉所有的空格，把所有的字符都改成相同的大小写。

```
`def is_palindrome(word):
    word = word.strip().lower()` 
```

从这里，我们可以采取一些方法。我想展示的第一种方法是使用列表和`reversed`函数。

我们必须对`reversed`小心一点，因为它会给我们一个惰性类型，并且它的值在任何情况下都不会等同于一个字符串。我们将从`reversed`对象中获取字符。在这种情况下，我们将把它转换成一个列表。

```
`def is_palindrome(word):
    word = word.strip().lower()
    reversed_word = reversed(word)

    if list(word) == list(reversed_word):
        print(True)
    else:
        print(False)` 
```

注意，为了进行比较，我们还必须将`word`转换成一个列表。那是因为 Python 不认为像`"hannah"`这样的东西和`["h", "a", "n", "n", "a", "h"]`是一样的。

我们可以用如下几个例子来测试我们的功能:

```
`def is_palindrome(word):
    word = word.strip().lower()
    reversed_word = reversed(word)

    if list(word) == list(reversed_word):
        print(True)
    else:
        print(False)

is_palindrome("Hannah")  # True
is_palindrome("Fred")    # False` 
```

这一切似乎都在起作用。

除了使用列表，我们还可以使用`join`从`reversed`对象创建一个字符串:

```
`def is_palindrome(word):
    word = word.strip().lower()
    reversed_word = reversed(word)

    if word == "".join(reversed_word):
        print(True)
    else:
        print(False)

is_palindrome("Hannah")  # True
is_palindrome("Fred")    # False` 
```

这里我们的`join`字符串是空的，这意味着当我们创建新的字符串时，我们不希望在`word`中的条目之间有任何东西。

如你所见，结果是一样的。

一种类似的方法是对 list 使用`reverse`方法。在这种情况下，我们将`word`转换为一个列表，并创建它的副本。然后我们可以反转这个副本并比较列表。

但是我们必须小心使用这种方法，因为我们只是想要一个值的副本。如果我们尝试这样做:

```
`def is_palindrome(word):
    word = list(word.strip().lower())
    reversed_word = word
    reversed_word.reverse()` 
```

两个名称指的是同一个列表。如果我们颠倒这个列表，两个名字现在指的是同一个颠倒的列表。

有几种方法可以解决这个问题。首先，我们可以在执行`reversed_word`赋值时传递`word`到`list`:

```
`def is_palindrome(word):
    word = list(word.strip().lower())
    reversed_word = list(word)
    reversed_word.reverse()` 
```

创建一个新列表，所以我们创建第二个具有相同值的列表。列表也有一个名为`copy`的方法来做这件事:

```
`def is_palindrome(word):
    word = list(word.strip().lower()):
    reversed_word = word.copy()
    reversed_word.reverse()` 
```

这两个都可以。然后，我们可以对列表进行直接比较:

```
`def is_palindrome(word):
    word = list(word.strip().lower())
    reversed_word = word.copy()
    reversed_word.reverse()

    if word == reversed_word:
        print(True)
    else:
        print(False)

is_palindrome("Hannah")  # True
is_palindrome("Fred")    # False` 
```

我想向您展示的最后一种方法是使用切片，因为在这种情况下，它们提供了一种极其优雅的解决方案:

```
`def is_palindrome(word):
    word = word.strip().lower()

    if word == word[::-1]:
        print(True)
    else:
        print(False)

is_palindrome("Hannah")  # True
is_palindrome("Fred")    # False` 
```

另一篇博文的[中提到了`[::-1]`，但它的基本意思是，“把整个序列倒过来给我”。](https://blog.teclado.com/python-quick-sequence-reversal/)

这意味着我们可以对照`word`来检查`word`，而不需要做任何类型转换，或者使用`join`。

如果您愿意，我们可以使用变量名来更好地描述这个值的含义:

```
`def is_palindrome(word):
    word = word.strip().lower()
    reversed_word = word[::-1]

    if word == reversed_word:
        print(True)
    else:
        print(False)

is_palindrome("Hannah")  # True
is_palindrome("Fred")    # False` 
```

如果你想试试这个练习的更复杂的版本，看看我们关于寻找回文的帖子。我们会更详细地讨论这些解决方案，当涉及到标点符号时，或者当我们处理完整句子的回文时，我们会讨论寻找回文。