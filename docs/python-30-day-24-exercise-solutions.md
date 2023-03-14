# 第 24 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-24-exercise-solutions/>

以下是我们对 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 24 天](/30-days-of-python/python-30-day-24-exceptions-advanced)练习的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)要求用户输入一个介于 1 和 10(包括 1 和 10)之间的整数。如果他们给出的数字超出了指定的范围，则引发`ValueError`并通知他们他们的选择无效。

首先，我们需要实际获取一些用户输入，并将其转换为整数。我们没有被告知要处理在这一步出现的任何异常，但是如果您愿意，您可以这样做。

```py
`number = int(input("Please enter a number between 1 and 10: "))` 
```

为了检查数量是否在指定的范围内，我们可以使用这样的条件:

```py
`number = int(input("Please enter a number between 1 and 10: "))

if number > 10 or number < 1:
    pass` 
```

或者，我们可以定义一个范围，并查看该数字是否在该范围内:

```py
`number = int(input("Please enter a number between 1 and 10: "))

if number not in range(1, 11):
    pass` 
```

在这两种情况下，我们都希望在条件语句体中引发一个`ValueError`,如下所示:

```py
`number = int(input("Please enter a number between 1 and 10: "))

if number not in range(1, 11):
    raise ValueError(f"The number must be between 1 and 10\. You entered {number}.")` 
```

### 2)下面你会发现一个`divide`函数。编写异常处理，这样我们就可以捕获`ZeroDivisionError`异常、`TypeError`异常和其他种类的`ArithmeticError`。

我们得到的函数是这样的:

```py
`divide(a, b)
    print(a / b)` 
```

首先，我们需要处理任何`ZeroDivisionError`异常，我们可以这样做:

```py
`divide(a, b)
    try:
        print(a / b)
    except ZeroDivisionError:
        print("Cannot divide by zero")` 
```

接下来，我们需要捕捉更一般的`TypeError`异常，这可以通过添加另一个`except`子句来实现。

```py
`divide(a, b)
    try:
        print(a / b)
    except ZeroDivisionError:
        print("Cannot divide by zero")
    except TypeError:
        print("Both values must be numbers. Cannot divide {a} and {b}")` 
```

最后，我们需要捕捉一般的`ArithmeticError`异常。

这最后的另一个条款很重要。确保不要把它放在`ZeroDivisionError`分支之前，因为`ZeroDivisionError`的所有实例都是`ArithmeticError`的实例。记住 Python 将按顺序一次检查一个`except`子句，并且它将使用它首先找到的匹配分支。

```py
`divide(a, b)
    try:
        print(a / b)
    except ZeroDivisionError:
        print("Cannot divide by zero")
    except TypeError:
        print("Both values must be numbers. Cannot divide {a} and {b}")
    except ArithmeticError:
        print("Could not complete the division. The numbers were likely too large")` 
```

### 3)下面你会发现一个接受集合的`itemgetter`函数，以及一个键或索引。捕捉任何`KeyError`或`IndexError`的实例，并将异常写入一个名为`log.txt`的文件，同时附上导致这个问题的参数。写入日志文件后，重新引发原始异常。

```py
`def itemgetter(collection, identifier):
    return collection[identifier]` 
```

首先，我将编写另一个名为`log_exception`的函数，它将负责写入文件。这将使我们的异常处理更加简洁，因为我们不会在`except`子句中嵌入上下文管理器，等等。

```py
`def log_exception(exception, fn, **kwargs):
    values = ", ".join(f"{key}={value!r}" for key, value in kwargs.items())

    with open("log.txt", "a") as log_file:
        log_file.write(f"Exception: {exception}, Function: {fn}, Values: {values}\n")` 
```

这里的一切我们以前都见过，但让我们快速浏览一遍。

我们的函数有三个参数:`exception`、`fn`和`**kwargs`。`exception`将接收一个异常对象，我们将通过命名我们捕获的特定异常来获取该异常对象。`fn`将是一个代表函数名的字符串，这样我们就可以确定异常发生在哪里。`**kwargs`将收集传递给该函数的所有参数，以及赋值给这些参数的值。

在函数体的第一行，我们遍历了`**kwargs`中的值，并生成了一个字符串，将键和值表示为关键字参数。对于`value`,我们有这个特殊的`!r`,这是一种确保我们得到的表示与代码中值的出现方式相匹配的方式，而不是当我们打印它们时用户看到的样子。这意味着我们可以在日志中区分`4`和`"4"`。

一旦我们有了值，我们就用逗号将它们连接在一起，然后在 append 模式(`"a"`)下向日志文件中写入一行。

现在让我们来看看实际的异常处理。在这种情况下，我们希望对两个异常做同样的事情，所以我们有一些选择。我们要么抓住更宽泛的`LookupError`，要么同时抓住`IndexError`和`KeyError`。在这种情况下，我会选择后者。

```py
`def log_exception(exception, fn, **kwargs):
    values = ", ".join(f"{key}={value!r}" for key, value in kwargs.items())

    with open("log.txt", "a") as log_file:
        log_file.write(f"Exception: {exception}, Function: {fn}, Values: {values}\n")

def itemgetter(collection, identifier):
    try:
        return collection[identifier]
    except (IndexError, KeyError) as ex:
        log_exception(ex, "itemgetter", collection=collection, identifier=identifier)` 
```

现在我们只需要使用`raise`关键字重新引发异常。

```py
`def log_exception(exception, fn, **kwargs):
    values = ", ".join(f"{key}={value!r}" for key, value in kwargs.items())

    with open("log.txt", "a") as log_file:
        log_file.write(f"Exception: {exception}, Function: {fn}, Values: {values}\n")

def itemgetter(collection, identifier):
    try:
        return collection[identifier]
    except (IndexError, KeyError) as ex:
        log_exception(ex, "itemgetter", collection=collection, identifier=identifier)
        raise` 
```

我们可以做得更好的一件事是处理`TypeError`异常，因为当我们将一个非整数作为一个序列的索引传入时会发生这种情况。

```py
`def log_exception(exception, fn, **kwargs):
    values = ", ".join(f"{key}={value!r}" for key, value in kwargs.items())

    with open("log.txt", "a") as log_file:
        log_file.write(f"Exception: {exception}, Function: {fn}, Values: {values}\n")

def itemgetter(collection, identifier):
    try:
        return collection[identifier]
    except (IndexError, KeyError, TypeError) as ex:
        log_exception(ex, "itemgetter", collection=collection, identifier=identifier)
        raise` 
```

### 注意

我们实际上并不像我在这个练习中展示的那样进行日志记录，因为我们不需要这样做。Python 有一个`logging`模块，它为我们做了很多工作，并生成更多信息日志。

你可以在这里找到关于日志记录的信息和入门教程。