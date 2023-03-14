# 第 24 天:高级异常处理和引发异常| Teclado

> 原文：<https://blog.teclado.com/python-30-day-24-exceptions-advanced/>

大家好，欢迎来到 Python 系列 [30 天的第 24 天！今天，我们将扩展我们的异常处理工具箱，我们将学习如何实际引发异常。](https://blog.teclado.com/30-days-of-python/)

我们还将研究异常的层次结构，这将有助于我们更加智能地处理异常。

## 异常层次结构

在标准库中有许多不同类型的异常，我们已经见过很多了。

我们实际上可以在文档中找到内置异常的完整列表，如下所示:

```py
`BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
      |    +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- ResourceWarning` 
```

您很快会注意到，有些异常被列为另一种异常的子类。比如我们来看看`SyntaxError`。

```py
`+-- SyntaxError
|    +-- IndentationError
|         +-- TabError` 
```

我们可能都见过`SyntaxError`很多次，它会告诉我们什么时候我们的 Python 代码不是有效的格式。我们做了违反语言规则的事情。

但是我们也可以看到，还有两个例外是更具体的`SyntaxError`类型。

其中第一个是`IndentationError`。每当我们缺少所需的缩进，或者我们使用的缩进模式有问题时，我们都会看到这种情况。

第二个是`TabError`，实际上是一个更具体的类型`IndentationError`，当我们有制表符和空格的组合来缩进时使用。这可能会有问题，因为许多现代编辑器和格式化工具会将制表符转换为空格，所以如果制表符后来被转换，我们最终得到的缩进量可能会与我们预期的不同。

了解这种层次结构是很有用的，因为所有这些更具体的异常都被认为是派生它们的不太具体的异常的实例。

我们再来看另一个例子，叫做`LookupError`。

```py
`+-- LookupError
|    +-- IndexError
|    +-- KeyError` 
```

`LookupError`在没有找到键或索引时使用，它有两个“孩子”:`IndexError`和`KeyError`。后两个对我们来说比`LookupError`更熟悉，因为它们实际上是由列表、字典和元组之类的东西引发的异常。

然而，`IndexError`和`KeyError`都被认为是`LoopkupError`，所以我们这样处理这些异常是完全合法的:

main.py

```py
`numbers = [1, 2, 3, 4, 5]

try:
    print(numbers[100])  # <- Out of range index
except LookupError:
    print("Could not retrieve that value.")` 
```

这里实际引发的是一个`IndexError`，但是我们的`except`子句仍然捕获了这个异常。

很容易看出这种模式在我们有一些灵活的代码可以处理序列和映射类型(如字典)的情况下是如何有用的。

它还让我们能够捕捉一般的异常，作为更具体的异常的后备。

main.py

```py
`numbers = [1, 2, 3, 4, 5]

try:
    print(numbers[100])  # <- Out of range index
except IndexError:
    print("The requested index is out of range")
except LookupError:
    print("Could not retrieve that value.")` 
```

当在`try`子句中发现异常时，Python 将检查`except`子句以查看是否有匹配。一旦发现匹配的异常，它将停止查找。这很像条件语句的工作方式。

在上面的例子中，我们现在将触发第一个`except`子句，因为我们有一个`IndexError`，但是其他类型的`LookupError`将触发第二个`except`子句，比如一个`KeyError`。

main.py

```py
`person = {
    "name": "Phil",
    "city": "Budapest"
}

try:
    print(person["age"])  # <- Referencing an undefined key
except IndexError:
    print("The requested index is out of range")
except LookupError:
    print("Could not retrieve that value.")` 
```

在上面的例子中，我们只是将一些东西打印到控制台，这不是一个非常现实的场景，但是我们可以用我们喜欢的任何东西来替换这些`print`调用，以适当地处理不同类型的异常。

## 访问原始异常

有时候，获得原始异常消息是很有用的，例如，如果我们想用它来记录日志。

在这些情况下，我们可以使用`as`关键字作为`except`子句的一部分，将变量名直接放在`as`关键字之后。这个变量名给了我们一个句柄，我们可以用它来访问与原始异常相关的信息。

main.py

```py
`numbers = [1, 2, 3, 4, 5]

try:
    print(numbers[100])  # <- Out of range index
except LookupError as ex:
    print(f"Error: {ex}")` 
```

现在我们的输出看起来像这样:

```py
`Error: list index out of range` 
```

### 注意

虽然看起来不像，但我们捕获并命名为`ex`的这个异常实际上不仅仅是一个简单的字符串。它包含许多其他信息，如原始回溯、错误消息、异常类型等。

我们不会在这里详细讨论这个问题，但是一旦你在 Python 学习之旅中走得更远，了解这个问题是很有用的。

## 引发异常

在构建我们的应用程序时，我们可能会遇到这样的情况，我们无法从我们试图处理的异常中恢复，或者我们事先知道我们想要执行的操作将是不可能的。

在这种情况下，我们知道如何自己引发异常是很重要的，这样我们就可以用一个有意义的错误消息来终止程序。

引发异常实际上非常简单。我们只需要键入单词`raise`,后跟我们想要引发的异常的名称。

例如，我们可以这样提出一个`ValueError`:

在控制台中，我们像往常一样得到一个回溯，以及导致程序终止的异常的名称。

```py
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    raise ValueError
ValueError` 
```

这里缺少了一个信息。我们可以通过在异常名称后添加一组括号，并将我们的消息放入其中，从而非常容易地添加一个。

main.py

```py
`raise ValueError("I raised this ValueError for no reason!")` 
```

现在，该消息反映在控制台输出中:

```py
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    raise ValueError("I raised this ValueError for no reason!")
ValueError: I raised this ValueError for no reason!` 
```

## 重新引发异常

我们还可以在`except`子句中以另一种方式使用`raise`。通过编写没有指定异常的`raise`,我们可以重新引发我们在`except`子句中捕获的异常。

这在我们实际上不想阻止异常发生，只想在异常终止应用程序之前对异常做些什么的情况下非常有用。

同样，日志记录是一个很好的例子。我们可能希望将原始异常中的信息用于我们的日志，这样我们就有了出错的永久记录，然后我们可以允许异常终止程序。

## 嵌套的`try`语句

你可能没有意识到的一点是，我们可以将`try`语句放在其他`try`语句中。如果我们想尝试从其他异常中恢复，这可能非常有用，但是我们不确定我们的修复是否有效。

假设我要从一个文件中读取大量的数字，我想把每个数字的字符串表示转换成一个整数。我相当确定几乎所有的数字都将是整数，但有时我可能会遇到浮点数的字符串表示。

为了简单起见，让我们假设所有的数字都在文件的不同行上，所以我们可以遍历文件来得到我们需要的。

因为我可能会在文件中遇到一个浮点，所以我不能这样做:

main.py

```py
`with open("numbers.txt", "r") as numbers_file:
    numbers = [int(number) for number in numbers_file]` 
```

如果我们试图将一个浮点数的字符串表示传递给`int`函数，例如“93.2”，程序将终止，因为`ValueError`将被`int`引发。

为了解决这个问题，我将定义一个函数来为我们进行转换。在这个函数中，我首先尝试使用`int`将数字转换为整数，如果失败，我将尝试将其转换为浮点数。

如果整数转换顺利，我将返回新的整数。如果我们得到一个`float`，我将使用`round`函数对该浮点数进行舍入，然后我将返回结果整数。

如果所有这些都失败了，那么我们就无能为力了，所以我将使用我自己的自定义消息来引发一个`ValueError`。

main.py

```py
`def intify(number):
    try:
        return int(number)
    except ValueError:
        try:
            f_number = float(number)
        except ValueError:
            raise ValueError(f"could not convert string to an integer: {number}")
        else:
            return round(f_number)

with open("numbers.txt", "r") as numbers_file:
    numbers = [int(number) for number in numbers_file]` 
```

## 控制追溯

上述方法的一个问题是我的回溯非常大，并且不是很有帮助:

```py
`Traceback (most recent call last):
  File "main.py", line 3, in intify
    return int(number)
ValueError: invalid literal for int() with base 10: '"f"'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "main.py", line 6, in intify
    f_number = float(number)
ValueError: could not convert string to float: '"f"'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "main.py", line 14, in <module>
    numbers = [intify(number) for number in numbers_file]
  File "main.py", line 14, in <listcomp>
    numbers = [intify(number) for number in numbers_file]
  File "main.py", line 8, in intify
    raise ValueError(f"could not convert string to an integer: {number}")
ValueError: could not convert string to an integer: "f"` 
```

这里有很多我不需要用户看到的信息，因为它主要描述了实现细节。我还定义了自定义的错误消息，它解释了用户需要了解的所有情况。

为了去掉所有这些额外的回溯信息，我们可以结合使用另一个叫做`from`的关键字`raise`。

`from`将让我们指定一个我们想要开始回溯信息的点。我们可以通过按名称引用异常来做到这一点(记住，我们可以使用`as`来命名异常)。或者，我们可以写`None`来代替异常名。

`None`将从这个`try`语句中去掉所有多余的回溯信息，只给我们留下最近的异常。

main.py

```py
`def intify(number):
    try:
        return int(number)
    except ValueError:
        try:
            f_number = float(number)
        except ValueError:
            raise ValueError(f"could not convert string to an integer: {number}") from None
        else:
            return round(f_number)

with open("numbers.txt", "r") as numbers_file:
    numbers = [int(number) for number in numbers_file]` 
```

现在我们的回溯可读性更强了。

```py
`Traceback (most recent call last):
  File "main.py", line 14, in <module>
    numbers = [intify(number) for number in numbers_file]
  File "main.py", line 14, in <listcomp>
    numbers = [intify(number) for number in numbers_file]
  File "main.py", line 8, in intify
    raise ValueError(f'could not convert string to an integer: {number}') from None
ValueError: could not convert string to an integer: "f"` 
```

## 练习

1)要求用户输入一个介于 1 和 10(包括 1 和 10)之间的整数。如果他们给出的数字超出了指定的范围，则引发`ValueError`并通知他们他们的选择无效。

2)下面你会发现一个`divide`函数。编写异常处理，这样我们就可以捕获`ZeroDivisionError`异常、`TypeError`异常和其他种类的`ArithmeticError`。

```py
`divide(a, b)
    print(a / b)` 
```

3)下面你会发现一个接受集合的`itemgetter`函数，以及一个键或索引。捕捉任何`KeyError`或`IndexError`的实例，并将异常写入一个名为`log.txt`的文件，同时附上导致这个问题的参数。写入日志文件后，重新引发原始异常。

```py
`def itemgetter(collection, identifier):
    return collection[identifier]` 
```

## 项目

一旦你完成了今天的练习，我们有[另一个项目给你](/30-days-of-python/python-30-day-24-project/)！这次我们将创建一个命令行程序来滚动各种骰子的组合。为了实现这一点，我们将在用户运行程序之前接受用户的各种配置！