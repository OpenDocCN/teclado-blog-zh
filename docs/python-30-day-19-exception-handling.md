# 第 19 天:异常处理

> 原文：<https://blog.teclado.com/python-30-day-19-exception-handling/>

欢迎来到 Python 系列 [30 天的第 19 天！今天我们将探讨异常处理这个非常重要的话题。](https://blog.teclado.com/30-days-of-python/)

我们还将讨论一下 Python 中如何使用异常，以及在我们的代码中请求原谅和请求许可的概念。

## 什么是例外？

到目前为止，我相信我们都遇到过一些例外。`SyntaxError`、`NameError`和`TypeError`可能开始感觉像是你每次坐下来写代码时看到的同事。

目前，在运行我们的代码时遇到这些异常之一对应用程序来说是致命的。如果我们试图将用户输入转换成一个整数，并且用户输入了`"Five"`，程序将会终止，我们将会在控制台中看到一些可爱的红色文本，显示`"Five"`不是一个基数为 10 的整数。

```py
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    int("Five")
ValueError: invalid literal for int() with base 10: 'Five'` 
```

看看这样一个例子，假设异常仅仅是错误是合理的，而且在大多数情况下，这个假设是正确的。大多数异常确实是错误。然而，也有一些例外，它们并不真的表明出了问题。它们更像是给定事件发生的通知。

一个例子是`StopIteration`，这可能是目前你的应用程序中最常见的异常:你只是从未见过它，因为它从未终止你的程序！稍后会有更多的介绍。

我们看到`StopIteration`的一个地方是当我们在一个`for`循环中迭代一些 iterable 时。它用于指示 iterable 的所有值都已被使用，并且没有新的值提供给我们。这就是 Python 知道何时终止`for`循环的方式，当我们使用[析构](/30-days-of-python/python-30-day-9-enumerate-zip)来给几个变量赋值时，也会发生同样的情况。

像这样使用异常作为信号在 Python 中是很常见的，这也是我们将在本系列中再次讨论的内容。

## 请求允许还是请求原谅

让我们回到前面的例子，我们试图从用户那里得到一个整数，他们输入了`"Five"`而不是我们期望的数字。

我们的代码可能看起来像这样:

```py
`number = int(input("Please enter a whole number: "))` 
```

这种类型的代码对我们来说并不陌生，但对我们来说，清楚我们在讨论什么是有好处的。

也许您不同意，但我认为在这种情况下终止应用程序是对用户输入无效值的强烈回应。即使我们真的想结束程序，给用户一个大大的红色错误信息可能也不是最好的方法。这听起来像是什么东西坏了，而不是他们的输入有问题。毕竟，我们从来没有打算接受数字写成文字。

让我们用目前已知的东西来思考如何解决这个问题。

我认为一个明智的方法是将提示放在一个`while`循环中。然后，我们可以检查用户输入是否是一个有效的整数，如果是，我们将使用该值赋给一个变量，并退出循环。如果无效，我们将进入下一个迭代，用户将再次被提示。

问题是，当我们只有一个字符串时，我们如何检查某个东西是否是有效的整数值？

这对于我们手动来说有点棘手，但是如果你一直在寻找我们对字符串可用的方法，你可能已经找到了`isnumeric`方法。因此，我们可以这样写:

```py
`while  True:
    user_number = input("Please enter a whole number: ")

    if  user_number.isnumeric():
        number = int(user_number)
        break
    else:
        print("You didn't enter a valid integer!")` 
```

如果我们试一试，它似乎是有效的。我们可以输入`4`或`4834854`，它不接受`3.141592`，这很好，因为这会给我们的`int`转换带来问题。

但是，如果您测试了任何负数，您将会发现一个问题。`isnumeric`返回负数的`False`，因为不是所有的数字都是数字。

这是个问题，但不是不可克服的。我们可以去掉任何初始的`-`符号，因为我们知道`int`可以处理这些符号。为此，我们可以使用`lstrip`，而不是普通的`strip`，因为`lstrip`只是从字符串的左边移除字符。

```py
`while  True:
    user_number = input("Please enter a whole number: ")

    if user_number.lstrip("-").isnumeric():
        number = int(user_number)
        break
    else:
        print("You didn't enter a valid integer!")` 
```

让我们再试一次！

现在没问题了，那太好了。正数仍然有效，所以我们没有破坏任何东西。然而，我们确实有一个新问题:`lstrip`有点太好了，如果它找到了许多`-`字符，它会将其剥离。这对我们来说是一个问题，因为对于`int`来说`-3`是一个有效的数字，而`--3`不是。

这意味着我们的 if 语句将接受`--3`为有效，但是随后将它转换为整数会给我们一个错误。

如果我们尝试`--3`，我们得到一个`ValueError`。

```py
`Traceback (most recent call last):
  File "main.py", line 5, in <module>
    number = int(user_number)
ValueError: invalid literal for int() with base 10: '--3'` 
```

在这一点上，我认为它开始变得清晰，我们在错误的道路上。即使对于这种非常简单的情况，我们也不得不手动处理许多边缘情况，并且很难知道我们是否遗漏了什么。

这种做法被称为“征求许可”。我们正在检查是否可以提前做一些事情，如果我们确定不会有任何问题，我们就继续进行。正如我们已经看到的，这种方法可能会非常混乱，并且会变得非常复杂。

这不是我们在 Python 中采用的异常处理方法。在 Python 中，首选的方法是简单地尝试我们认为可能失败的事情，然后在发生异常时从异常中恢复。这使问题变得简单得多:知道可能会发生什么异常。在上面的案例中，我们只需要担心一个异常:`ValueError`。

这种替代模式被称为“请求原谅”，因为我们正在尝试一些可能出错的事情，然后如果某些事情*出错了*，我们会做一些事情来弥补。

让我们来看看一个新的语法，它将允许我们使用这种请求原谅的模式:`try`语句。

## `try`声明

一个陈述可能会很长很详细，但是我们实际上只需要两部分就可以开始了。我们需要一个`try`子句，包含我们预计会失败的代码，然后通常我们至少需要一个`except`子句，描述当某种类型的失败发生时该做什么。

让我们再来看看我们的数字示例，看看一个`try`语句在起作用:

```py
`while True:
    try:
        number = int(input("Please enter a whole number: "))
        break
    except ValueError:
        print("You didn't enter a valid integer!")` 
```

这里我们有两行想要尝试的代码。我们希望接收一些用户输入，并将其转换为整数，然后如果没有出错，我们希望跳出`while`循环。

当我们在`try`子句中运行这些操作时，我们的`except`子句正在等待查看是否有任何`ValueError`被引发。如果引发了`ValueError`，我们将放弃`try`子句中的代码，转而执行`except`子句中列出的操作。

在这个例子中，我们简单地打印了，`"You didn't enter a valid integer!"`，然后我们用完了循环体中的代码，因此循环的新迭代开始了。

请注意，当`ValueError`发生时，我们不会在控制台中得到错误消息。

我们用我们的`except`子句“处理”了这个异常，并且我们提供了一个可供选择的行动过程。只有未处理的异常会终止应用程序，因为在这些情况下，我们没有提供可行的替代方案。终止应用程序实际上是 Python 的最后一招。

需要记住的另一件事是，我们在这里只处理了一种类型的异常。如果我们以某种方式得到一个`TypeError`会发生什么？

我们的`except`子句对处理`TypeError`没有任何作用，所以在这个实例中我们没有提供一些替代的操作过程。这意味着`TypeError`仍然会终止我们的程序，而这有时正是我们想要的。有时我们无法纠正一个问题，在这种情况下，让一个异常终止程序是完全可以接受的。

### 重要的

需要记住的一件重要事情是，一旦出现异常，`try`块将停止运行。如果出现问题，就好像`try`块中的代码都没有运行过。

## 处理多个可能的异常

我们有两种方法可以使用一条`try`语句处理多个异常，它们有不同的用例。

让我们首先想象我们有两个不同的可能发生的异常，我们希望根据发生的情况做不同的事情。在这种情况下，我们应该有两个`except`子句，每个`except`子句应该描述当异常发生时我们想要采取的行动。

作为一个例子，让我们创建一个从一些数字集合中计算平均值的函数。我们不知道用户要传递给这个函数什么，所以有些事情可能会出错。

我们将从这里开始:

```py
`import math

def average(numbers):
    mean = math.fsum(numbers) / len(numbers)
    print(mean)` 
```

我在这里使用`fsum`，只是因为数字在很多情况下很可能是浮点数而不是整数。

好吧，让我们考虑一下潜在的问题:

1.  用户可能会传入一个空的集合，所以我们将从`len`返回`0`。这将导致被`0`分裂，这是不允许的。在这种情况下，我们得到一个`ZeroDivisionError`。
2.  用户可能会传入不是集合的东西。这将给我们一个`TypeError`，因为`fsum`期望一个可迭代的，而`len`期望一个集合。
3.  用户可以传入一个包含非数字内容的集合。这也将是一场`TypeError`。

这给了我们两个需要注意的例外:`ZeroDivisionError`和`TypeError`。

如果你不确定会发生什么样的错误，一个好的方法就是尝试测试用例，看看会发生什么。文档也很好地描述了各种操作和功能的异常情况。

既然我们知道什么可能出错，我们可以写下我们的`try`语句。

```py
`import math

def average(numbers):
    try:
        mean = math.fsum(numbers) / len(numbers)
        print(mean)
    except ZeroDivisionError:
        print(0)
    except TypeError:
        print("You provided invalid values!")` 
```

现在我们正在处理得到`ZeroDivisionError`和得到`TypeError`不同的情况，所以我们能够提供关于哪里出错的更具体的反馈。在`ZeroDivisionError`的例子中，我们甚至没有通知用户一个问题:我们已经决定，对于我们的函数来说，没有任何东西的平均值是`0`。

如果我们想捕捉两个异常并做同样的事情，我们不需要两个`except`子句。我们可以用同一个`except`子句捕获多个异常，如下所示:

```py
`import math

def average(numbers):
    try:
        mean = math.fsum(numbers) / len(numbers)
        print(mean)
    except (ZeroDivisionError, TypeError):
        print("An average cannot be calculated for the values you provided.")` 
```

### 重要的

在某些时候，您可能会遇到这样的代码:

```py
`import math

def average(numbers):
    try:
        mean = math.fsum(numbers) / len(numbers)
        print(mean)
    except:
        print("An average cannot be calculated for the values you provided.")` 
```

您会注意到我们没有在`except`子句后列出任何例外。这被称为裸`except`子句，它将捕获任何发生的异常。

虽然这有它的用处，但在您的代码中使用它通常是非常不好的。它提供了捕捉许多我们没有预料到的异常的可能性，并且它可以掩盖严重的实现问题。

## `else`条款

除了`try`和`except`子句，我们还可以在`try`语句中使用`else`子句。只有在执行`try`子句中的代码时没有出现异常，才会运行`else`子句下的代码。

当我发现`else`的时候，我知道很多学生认为它完全没用。我们就不能在`try`块中放更多的代码吗？

我们可以，但是这里有几个原因可以解释为什么这是一个非常糟糕的主意。

1.  我们可能会意外地捕捉到我们没有预料到的异常。出现在`try`子句中的代码越多，这种情况就越有可能发生。这可能会使我们对异常的处理不恰当，因为我们要处理的是一个实际上并没有发生的情况。
2.  它损害了可读性。`try`子句表达了我们预期会失败的内容，而`except`子句表达了我们计划处理代码中特定失败的方式。添加到`try`子句中的代码越多，我们实际尝试的东西就越不清楚，这会使整个结构更难理解。

这是否意味着我们总是需要一个`else`条款？号码

运用你的判断力。对于非常简单的例子来说，这可能有些过分，就像下面的例子:

```py
`import math

def average(numbers):
    try:
        mean = math.fsum(numbers) / len(numbers)
    except ZeroDivisionError:
        print(0)
    except TypeError:
        print("You provided invalid values!")
    else:
        print(mean)` 
```

我们不太可能在打印数字时遇到任何问题，很明显这不是我们关心的测试问题。

只要确保在进行更复杂的操作时不要忘记`else`就行了。

## `finally`条款

除了`else`之外，对于`try`语句，我们还有一个更重要的子句:`finally`。

`finally`很特别，因为它会*一直*跑。

如果出现未处理的异常，也没关系。在异常终止程序之前，仍然会运行它的代码。

如果我们从`try`语句中的一个函数返回，`finally`将中断这个返回，首先运行它自己的代码。通过运行以下代码，您可以看到一个示例:

```py
`def finally_flex():
    try:
        return
    finally:
        print("You return when I say you can return...")

finally_flex()` 
```

该属性对于任何需要在操作后进行重要清理的情况都非常有用。一个例子是当处理文件时。如果我们在处理文件中的数据时遇到一些问题，会发生什么？我们仍然希望在完成后关闭文件，使用`finally`我们可以确保这一点。

我们用来处理文件的上下文管理器实际上在幕后使用了与这个`finally`子句非常相似的东西，以确保文件无论如何都是关闭的。事实上，这是如此的相似，以至于我们可以使用带有`finally`子句的`try`语句来创建自己的上下文管理器。

`finally`的主要用例通常出现在更高级的代码中，但是在这个阶段它仍然是一个需要了解的重要工具。

## 练习

1)创建一个短程序，提示用户输入用逗号分隔的成绩列表。将字符串拆分为单独的等级，并使用列表理解将每个字符串转换为整数。当用户输入的值不能被转换时，您应该使用一个`try`语句来通知用户。

2)调查当`try`语句的`try`子句和`finally`子句中都有`return`语句时会发生什么。

3)假设您有一个名为`data.txt`的文件，其内容如下:

使用 Python 打开它进行读取，但是要确保使用一个`try`块来捕捉文件不存在时出现的异常。一旦你验证了你的解决方案可以处理一个实际的文件，删除这个文件，看看你的`try`块是否能够处理它。

当您尝试打开文件时，如果文件不存在，则会引发异常`FileNotFoundError`。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-19-exercise-solutions)

## 额外资源

你可以在这里找到更多关于所有内置异常的信息。