# 第 19 天锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-19-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 19 天练习](/30-days-of-python/python-30-day-19-exception-handling)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)创建一个短程序，提示用户输入用逗号分隔的成绩列表。将字符串拆分为单独的等级，并使用列表理解将每个字符串转换为整数。当用户输入的值不能被转换时，您应该使用一个`try`语句来通知用户。

让我们从编写获取成绩列表的`input`调用开始。我们也可以使用`split`方法来分割从`input`返回的字符串，所有的都在同一行上。

```py
`grades = input("Please enter your grades, separated by commas: ").split(",")` 
```

在我们继续之前，让我们考虑一下，如果用户输入如下内容来响应我们的提示，会发生什么情况:

在这里，我们在所有逗号后面都有空格，所以我们得到的`grades`列表应该是:

```py
`['76 ', '93 ', '84 ', '81']` 
```

不过这不是问题，因为 Python 乐意接受字符串`'76 '`作为`int`或`float`的有效参数，它会为我们去掉空格。

因此，没有必要这样做:

```py
`grades = input("Please enter your grades, separated by commas: ").split(",")
grades = [grade.strip() for grade in grades]` 
```

现在我们开始练习的核心部分。我们需要尝试将每个字符串转换成一个整数，然后我们需要捕捉用户提供无效值时出现的任何异常。

在用户输入无效值的情况下，我们将得到一个`ValueError`，所以这是我们需要用`except`子句捕捉的异常。

```py
`grades = input("Please enter your grades, separated by commas: ").split(",")

try:
    pass
except ValueError:
    pass` 
```

在`try`子句中，我将创建一个 list comprehension 来将每个字符串转换成一个整数。然后我将把结果赋给`grades`。如果你需要一个关于列表理解如何工作的提示，再看看[第 15 天](/30-days-of-python/python-30-day-15-comprehensions/)。

在`except`子句中，我将写一条消息通知用户他们的数据格式无效。

在一个更完整的应用程序中，使用`grades`的其余代码可能会放在一个`else`子句中，但是我们在这里不必担心这个。

```py
`grades = input("Please enter your grades, separated by commas: ").split(",")

try:
    grades = [int(grade) for grade in grades]
except ValueError:
    print("The grades you entered were in an invalid format.")` 
```

### 2)调查当`try`语句的`try`子句和`finally`子句中都有`return`语句时会发生什么。

为了完成这个练习，我们需要写一个函数，因为我们需要一个从返回*的函数。*

在这里我们不必太花哨。这将会:

现在让我们把我们的`try`语句放在函数体中。我建议你对两个返回值使用不同的东西，这样我们就知道我们的值来自哪里。

类似下面的函数可能是合适的:

```py
`def func():
    try:
        return "Returned from the try clause!"
    finally:
        return "Returned from the finally clause!"` 
```

现在让我们调用我们的函数，看看会发生什么。

```py
`def func():
    try:
        return "Returned from the try clause!"
    finally:
        return "Returned from the finally clause!"

print(func())  # "Returned from the finally clause!"` 
```

可能有点令人惊讶的是，我们在`finally`子句中获得了返回值。这也有一定的道理，因为`finally`子句*总是*运行，并且它会中断`try`子句中的`return`。它有效地暂停了那个`return`，同时执行自己的`return`。

问题是，如果我们在 finally 子句中执行`return`,我们就结束了函数的执行，所以我们再也不会回来执行`try`子句中的函数。它只是被遗忘了。

### 3)假设您有一个名为`data.txt`的文件。使用 Python 打开它进行读取，但是要确保使用 try 块来捕获文件不存在时出现的异常。一旦您验证了您的解决方案可以处理一个实际的文件，请删除该文件，看看您的 try 块是否能够处理它。

我们的`data.txt`文件的内容很简单，如下所示:

这并不十分重要。

我们还被告知，我们需要注意的异常是`FileNotFoundError`。

记住这一点，让我们构建我们的`try`语句:

```py
`try:
    pass
except FileNotFoundError:
    pass` 
```

在我们的`try`子句中，我们将编写访问文件的代码。我将把文件内容打印到控制台上。在`except`子句中，我将向用户写一条消息，指出找不到该文件。

在实际的应用程序中，我们可能会利用这个机会为用户创建文件。

```py
`try:
    with open("data.txt", "r") as text_file:
        print(text_file.read())
except FileNotFoundError:
    print("Error: Couldn't find data.txt")` 
```

就这样，我们结束了！