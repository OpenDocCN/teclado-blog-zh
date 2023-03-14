# 第 12 天:功能| Teclado

> 原文：<https://blog.teclado.com/python-30-day-12-functions/>

欢迎来到 Python 系列 [30 天的第 12 天！在这篇文章中，我们将看看函数。](https://blog.teclado.com/30-days-of-python)

我们在整个课程中使用了许多内置函数，例如`print`和`input`，但是现在我们将学习如何创建一些我们自己的函数。

此外，我们还制作了第 10 天和第 11 天的简短回顾，您可以在此处查看来刷新您的记忆。

## 为什么要使用函数？

在我们开始编写函数之前，我们需要理解它们为什么有用。

我们暂时以`print`为例。打印是我们一直在做的事情，无论是测试我们的代码还是向程序用户提供信息。

虽然这听起来是一个相当简单的操作，但是`print`的代码实际上有 80 多行长！`print`也调用其他几个函数来完成它的工作，所以`print`提供的完整功能实际上需要几百行代码。

这提供了使用函数的几个主要好处:

1.  它们允许我们减少重复的长而复杂的代码来执行我们想要多次执行的操作。
2.  它们使我们的代码更具可读性。理解`print("Hello, world!")`比理解`print`函数的冗长实现要容易得多。

这给我们带来了函数的第三个主要好处:我们不需要知道函数的底层代码是如何工作的。从这个系列的第一天开始，我们就一直在使用`print`，但是我们还不知道任何关于函数的事情，我们当然也不知道当我们调用它的时候`print`在做什么。

我们知道如何使用函数，以及函数将如何处理我们提供的值，这就足够了。实现细节通常可以被安全地忽略。

## 定义我们的第一个功能

首先，我们将定义一个名为`get_even_numbers`的简单函数。这个函数将打印出前十个偶数，从`2`开始。

如果你不清楚如何得到前十个偶数，我们可以像这样使用`for`循环和`range`:

```
`for number in range(1, 11):
    print(number * 2)` 
```

我们还可以向名为`step`的`range`传递第三个值，这允许我们创建具有不同模式的序列。例如，指定一个`2`的`step`将允许我们获得指定范围内的每秒钟的数字。

```
`for number in range(2, 21, 2):
    print(number)` 
```

这两种方法都可以。

那么，我们如何把它变成一个函数呢？

第一步是写`def`关键字，是“define”的简称。这对 Python 来说是一个函数定义。在同一行中，我们需要写出函数的名字。这个名字将作为函数的标识符，就像变量名一样。

直接在函数名之后，我们需要一对括号，暂时保留为空。最后，我们需要用冒号结束这一行。因此，我们的函数定义的第一行如下所示:

然而，这不是合法的语法，因为所有的函数都需要一个函数体。这个主体包含了当我们调用函数时应该运行的代码。在我们例子中，我们想要运行上面指定的 for 循环，所以我们需要将它包含在我们的函数体中。

函数体直接写在定义函数名的那一行的下面，必须缩进，就像我们使用 for 循环或条件语句一样。

```
`def get_even_numbers():
    for number in range(1, 11):
        print(number * 2)` 
```

这样，我们就有了一个完全可用的函数，我们可以像调用任何内置函数一样调用它:

```
`def get_even_numbers():
    for number in range(1, 11):
        print(number * 2)

get_even_numbers()` 
```

自己运行代码看看！

### 风格注释

随着我们开始编写越来越复杂的函数，函数体中出现空行来分割代码将会很常见。出于这个原因，我们所有的函数定义后面都应该跟有**两个**空行。

这将有助于很容易地看到我们的函数在哪里结束。

## 函数参数和自变量

`get_even_numbers`函数工作得很好，但是我们在本系列中使用的许多函数在调用时都能够接受值。例如，`print`接受我们想要输出的值，而`input`接受一个提示。

甚至有些函数我们必须向 T2 传递一个值。例如，我们不能在不传递集合的情况下调用`len`。这是无效的:

```
`len([1, 3, 5])  # 3
len()  # Error!` 
```

在第二行中我们没有传递给它一个集合，所以我们得到一个`TypeError`:

```
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    len()
TypeError: len() takes exactly one argument (0 given)` 
```

如果我们想在自己的函数中接受值，我们需要告诉 Python 期待值。我们通过定义*参数*来做到这一点。

还记得我们函数定义中那些看起来没怎么用的括号吗？这是我们指定参数的地方。在我们最初的`get_even_numbers`函数中，我们没有任何参数，所以括号是空的:

让我们更新一下我们的`get_even_numbers`函数，这样用户就可以指定打印出多少个数字。我们希望接受单个值，因此需要提供单个参数。我们传入的值叫做*参数*。

参数实际上只是一个变量，它将为我们提供一种方法来访问用户传入的参数。它们充当参数值的名称。

在`get_even_numbers`的情况下，我将通过在括号之间写`amount`来定义一个名为`amount`的参数。然后我将在函数体中使用这个参数来修改`range`。

```
`def get_even_numbers(amount):
    for number in range(1, amount + 1):
        print(number * 2)` 
```

不要忘记`range`的停止值是不包含的，所以如果我们想在这种情况下生成正确数量的数字，我们必须加上`1`。

现在，如果我们调用我们的`get_even_numbers`函数并传入一个整数作为参数，我们将能够改变我们输出的数字数量。

```
`def get_even_numbers(amount):
    for number in range(1, amount + 1):
        print(number * 2)

get_even_numbers(5)` 
```

试试看！

您可能会注意到，我们不再能够在不传入任何参数的情况下调用函数。如果我们尝试，我们将得到一个`TypeError`，就像我们在没有传入参数的情况下试图调用`len`一样。

```
`Traceback (most recent call last):
  File "main.py", line 6, in <module>
    get_even_numbers()
TypeError: get_even_numbers() missing 1 required positional argument: 'amount'` 
```

在这种情况下，异常消息非常有用。我们被告知缺少一个必需的参数，缺少值的参数称为`amount`。

我们有办法让某些参数可选，但这是我们在本系列后面要研究的东西。现在，重要的是要认识到，如果我们指定了一个参数，我们需要在调用函数时为该参数提供一个参数。

## 指定多个参数

让我们定义一个名为`x_print`的新函数。`x_print`将接受两个参数:一个要打印的字符串，以及打印该字符串的次数。

所以如果我们这样称呼`x_print`:

我们预计`Hello!`会打印到控制台 5 次，如下所示:

```
`Hello!
Hello!
Hello!
Hello!
Hello!` 
```

定义这个函数并不比`get_even_numbers`函数更复杂，但是我想让你注意 Python 默认情况下是如何给参数赋值的。

下面是`x_print`的一个可能的实现:

```
`def x_print(requested_output, quantity):
    for _ in range(quantity):
        print(requested_output)` 
```

这里我们有一个`for`循环，我们从一个`range`序列中获取项目。然而，我们实际上并不关心这个`range`中的值:我们只是使用`range`来确保某件事情发生一定的次数。为了表明循环变量不重要，我们使用了一个`_`作为变量名，这是这种情况下的常见惯例。

对于循环的每一次迭代，我们打印出用户提供的任何内容，作为他们请求的输出。

我们需要明确的一点是，Python 不理解我们定义的这些名字的意思。像变量一样，参数只是为了方便开发人员而使用的名称，这样我们就可以很容易地推断出我们正在处理的值的种类。

当我们有多个参数时，默认情况下 Python 会按顺序给这些参数赋值。

你可能已经注意到我们读到的一些错误消息提到了*位置参数*。这是因为这些参数是根据它们在参数列表中的位置分配给参数的。

这个事实意味着我们为函数提供参数的顺序可能非常重要，如果我们不小心，可能会出现一些错误。例如，如果我们写这样的东西，

```
`def x_print(requested_output, quantity):
    for _ in range(quantity):
        print(requested_output)

x_print(5, "Hello")` 
```

我们会有一些问题。现在`5`被分到`requested_output`，而`"Hello"`被分到`quantity`。这将给我们一个错误，因为我们提供了`quantity`到`range`，而`range`期望一个整数。

在这种情况下，我们得到的是一个`TypeError`:

```
`Traceback (most recent call last):
  File "main.py", line 6, in <module>
    x_print(5, "Hello")
  File "main.py", line 2, in x_print
    for _ in range(quantity):
TypeError: 'str' object cannot be interpreted as an integer` 
```

## 关键字参数

我们在本系列中已经看到过几次关键字参数，但是我们并没有真正地直接谈到它们。关键字实参是位置实参的一种替代方法，在位置实参中，我们将实参的值与一个参数名联系起来。

例如，我们可以这样调用我们的`x_print`函数:

```
`def x_print(requested_output, quantity):
    for _ in range(quantity):
        print(requested_output)

x_print(requested_output="Hello", quantity=5)` 
```

这里我们指定了`"Hello"`应该是`requested_output`的值，`5`应该是`quantity`的值。因为我们已经指定了哪个值属于哪个参数，所以我们现在不必担心我们指定这些值的顺序。

例如，这样就可以了:

```
`x_print(quantity=5, requested_output="Hello")` 
```

我们也可以混合位置参数和关键字参数，但是我们必须小心一点，因为有一些限制。

我们不能在关键字参数后提供位置参数。这在 Python 中是无效的语法:

```
 `x_print(requested_output="Hello", 5)` 
```

如果我们提供一个映射到给定参数的位置参数，我们就不能为同一个参数提供关键字参数。例如，我们不能这样做:

```
 `x_print(5, requested_output="Hello")` 
```

这里的问题是`5`是一个位置参数，所以被赋给了`requested_output`(第一个参数)。当我们为同一个参数指定一个关键字参数时，Python 意识到有问题，并引发了一个`TypeError`。

如果我们想为我们的`x_print`函数混合位置和关键字参数，我们只有一个可行的选择:

```
`x_print("Hello", quantity=5)` 
```

这满足了所有要求。关键字 arguments 跟在位置参数之后，我们没有因为值的顺序而导致任何重复的赋值。

通常，尽可能使用关键字参数是一个好主意，因为它们提供了很多可读性好处。然而，为参数提供关键字参数并不总是可能的。许多内置函数不接受某些参数的关键字实参，在 Python 3.8 中，我们还获得了定义仅位置形参的能力。

这些参数不接受关键字参数。如果你尝试，你只会得到一个`TypeError`。

关于函数，我们还需要学习很多东西，但是现在，让我们通过一些练习来测试我们所学的内容。

## 练习

1)定义四个函数:`add`、`subtract`、`divide`和`multiply`。每个函数应该有两个参数，它们应该打印由函数名指示的算术运算的结果。

当操作的顺序很重要时，第一个参数应该被视为左操作数，第二个参数应该被视为右操作数。比如用户传入`6`和`2`做减法，结果应该是`4`，而不是`-4`。

您还应该确保用户不能将`0`作为`divide`的第二个参数传入。如果用户提供了`0`，你应该打印一个警告而不是计算他们的除法。

2)定义一个名为`print_show_info`的函数，它只有一个参数。传递给它的参数将是一个字典，其中包含一些关于电视节目的信息。例如:

```
 `tv_show = {
    "title": "Breaking Bad",
    "seasons": 5,
    "initial_release": 2008
 }

 print_show_info(tv_show)` 
```

函数应该以一种很好的方式打印存储在字典中的信息。例如:

```
 `Breaking Bad (2008) - 5 seasons` 
```

记住你必须在调用函数*之前定义它！*

3)以下是包含多部电视连续剧详细信息的列表。

```
 `series = [
    {"title": "Breaking Bad", "seasons": 5, "initial_release": 2008},
    {"title": "Fargo", "seasons": 4, "initial_release": 2014},
    {"title": "Firefly", "seasons": 1, "initial_release": 2002},
    {"title": "Rick and Morty", "seasons": 4, "initial_release": 2013},
    {"title": "True Detective", "seasons": 3, "initial_release": 2014},
    {"title": "Westworld", "seasons": 3, "initial_release": 2016},
 ]` 
```

使用您的函数`print_show_info`和一个`for`循环来遍历`series`列表，并在每次迭代中调用一次您的函数，传入每个字典。您应该以适当的格式打印每个系列。

4)创建一个函数来测试一个单词是否是回文。回文是一串向前或向后阅读都相同的字符。例如，“我看到的是一辆车还是一只猫”是一个回文。

在 day 7 项目中，我们看到了许多颠倒序列的方法，你可以用这些方法来验证一个字符串是否和它原来的顺序一样。你也可以使用[切片方法](https://blog.teclado.com/python-quick-sequence-reversal/)。一旦发现一个单词是否是回文，就应该将结果打印给用户。

确保清理提供给函数的参数。我们应该去掉字符串两端的空格，我们应该把它们都转换成相同的大小写，以防我们处理一个名字，比如`“Hannah”`。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-12-exercise-solutions)

## 项目

一旦你完成了练习，就该处理今天的[项目](/30-days-of-python/python-30-day-12-project)了！

## 额外资源

如果您想了解仅位置参数，在描述 Python 3.8 新特性的 [Python 文档](https://docs.python.org/3/whatsnew/3.8.html#positional-only-parameters)中有一个简短的总结。仅位置参数一节还描述了该特性的几个用例。

如果你有兴趣扩展你的回文解决方案来处理完整的句子和带标点的单词，我们在博客上有一个更详细的演练[。](https://blog.teclado.com/coding-interview-problems-palindromes/)