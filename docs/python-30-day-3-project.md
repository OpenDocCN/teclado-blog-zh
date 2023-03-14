# 第三天项目:一个简单的收入计算器

> 原文：<https://blog.teclado.com/python-30-day-3-project/>

欢迎来到 Python 系列 [30 天的第一个迷你项目。对于这个项目，我们将创建一个简单的控制台应用程序，它将帮助雇主计算雇员在给定的一周内的收入。](https://blog.teclado.com/30-days-of-python/)

如果你还没有完成第三天剩余的内容，我建议你在尝试这个项目之前完成。

下面你会发现一个项目简介，以及一个带有解释的模型解决方案。我真的建议你在寻找解决方案之前先自己尝试一下；我保证你会得到更多。如果你被卡住了，参考本系列之前的帖子，或者看看你写的任何笔记都没有错。这不是考试！

一旦你写了你的程序，你不应该担心它看起来和我们的有一点点不同。您可能选择了不同的变量名或提示，或者您可能对我们使用了稍微不同的方法。这绝对没问题。通常有许多不同的方法来编写像这样非常短的程序。

## 案情摘要

我们的应用程序应该如下工作:

1.  应该给用户三个提示，要求他们提供关于雇员的不同信息。一个提示应该询问雇员的姓名，另一个应该询问他们的时薪，最后一个应该询问雇员这个星期工作了多少小时。
2.  应该对雇员姓名进行处理，以确保它采用特定的格式。所有员工的名字都应该去掉多余的空格，并且应该是大写的。这意味着每个单词都是大写的，其他字母都是小写的。例如，如果雇主无意中按下了 caps lock 键，他们写了`"rEGINA gEORGE"`，姓名仍然会正确地存储为`"Regina George"`。
3.  雇员一周的总收入应该通过工作时间乘以小时工资来计算。
4.  请记住，我们收到的任何用户输入都将是一个字符串。虽然我们可以对字符串进行乘法运算，但在这种情况下，这并不完全符合您的要求。同样值得记住的是，员工的工资，或者他们工作的小时数，可能不是一个整数。
5.  在处理了雇员的姓名并计算了他们一周的收入之后，程序应该将这些信息作为一个字符串输出。例如，类似这样的输出是合适的:

```py
`Regina George earned $800 this week.` 
```

## 解决方案演练

下面你会发现我们将如何处理这个特殊问题的演练。再说一次，如果你的有点不同，不要担心！

对于这个项目，我们将一步一步地解决问题。将任务分解成简单的块总是一个好主意，因为这会让项目不那么令人生畏。

首先，我们将向用户询问一些信息。我们不会处理任何字符串，也不会担心类型。我们只想从用户那里获取信息，并将这些信息存储在一些变量中。

```py
`name = input("Please enter the employee's name: ")
hourly_wage = input("What is their hourly wage? ")
hours_worked = input("How many hours have they worked this week? ")` 
```

如果我们想测试事情是否正常，我们可以打印变量:

```py
`print(name)
print(hourly_wage)
print(hours_worked)` 
```

在这个阶段我们应该不会有任何问题，所以我们可以继续纠正`name`输入。我们有几个写这个的选择，但是基本上方法是一样的。我们需要调用`strip`方法来删除字符串两端多余的空白，我们需要调用`title`来将字符串转换成标题大小写。

一种方法是创建三个不同的变量，并对每一行执行不同的操作:

```py
`name = input("Please enter the employee's name: ")
stripped_name = name.strip()
title_name = stripped_name.title()` 
```

这很好，但是我们不需要在这里使用不同的变量名。我们不会使用原始形式的`name`,所以我们不需要保存它。我们去掉了空格的字符串也是如此。我们只关心最终产品，所以我们可以在每个阶段安全地覆盖`name`:

```py
`name = input("Please enter the employee's name: ")
name = name.strip()
name = name.title()` 
```

如果我们想真正简洁，我们可以在`input`调用的末尾链接各种操作，如下所示:

```py
`name = input("Please enter the employee's name: ").strip().title()` 
```

这样做的原因是，Python 将从左边开始，一次一个地评估这些调用(都是表达式)。

所以首先我们有了`input`呼叫。这将在调用`strip`之前计算一个字符串。

让我们想象一下，用户输入`" rEGINA gEORGE "`，实际姓名周围都是空白。这是我们从`input`得到的结果，所以在`input`运行完毕后，我们的代码大致相当于这样:

```py
`name = " rEGINA gEORGE  ".strip().title()` 
```

现在要执行的下一个操作是`strip`调用。`strip`也计算出一个新的字符串，所以我们最终得到这样的结果:

```py
`name = "rEGINA gEORGE".title()` 
```

现在空白区域消失了，我们还有一个操作要执行:调用`title`。`title`，很像`strip`和`input`，计算出另一个字符串，留给我们这个:

既然我们的员工姓名已经被正确处理，我们可以开始考虑计算他们的收入了。不幸的是，我们不能这样做:

```py
`name = input("Please enter the employee's name: ").strip().title()
hourly_wage = input("What is their hourly wage? ")
hours_worked = input("How many hours have they worked this week? ")

earnings = hourly_wage * hours_worked` 
```

这将给我们一个`TypeError`:

```py
`Traceback (most recent call last):
    File "main.py", line 5, in <module>
    earnings = hourly_wage * hours_worked
TypeError: can't multiply sequence by non-int of type 'str'` 
```

这里的信息相当清楚，我们将一个字符串乘以一个字符串，但其中至少有一个需要是整数。

好吧，让我们把工作时间改成一个整数，看看会发生什么:

```py
`name = input("Please enter the employee's name: ").strip().title()
hourly_wage = input("What is their hourly wage? ")
hours_worked = input("How many hours have they worked this week? ")

earnings = hourly_wage * int(hours_worked)

print(earnings)` 
```

如果您尝试运行这个代码，您将得到您输入的小时工资值，它会重复多次，等于您输入的工作小时数。这是因为字符串乘以整数是执行重复字符串连接的一种简化方式。`"-" * 5`与`"-" + "-" + "-" + "-" + "-"`相同。

考虑到这一点，我们需要将这两个值都转换成数字。然而，我们不是转换成整数，而是要转换成浮点数，因为这将允许我们接受用户工资或工作时间的非整数值。例如，我们希望能够以每小时 13.50 英镑的工资计算 32.5 小时的工作时间。

```py
`name = input("Please enter the employee's name: ").strip().title()
hourly_wage = input("What is their hourly wage? ")
hours_worked = input("How many hours have they worked this week? ")

earnings = float(hourly_wage) * float(hours_worked)` 
```

确保您打印收入以检查您的代码。如果 Python 引发了一些异常，尝试使用错误消息和回溯来找出它们发生的原因。

我们不能做的一件事是接受带有货币符号的工资。如果你想要一个额外的挑战，你可以尝试找出如何为你的当地货币做到这一点。你可能想看看[剥离](https://docs.python.org/3/library/stdtypes.html#str.strip)、 [lstrip](https://docs.python.org/3/library/stdtypes.html#str.lstrip) 和 [rstrip](https://docs.python.org/3/library/stdtypes.html#str.rstrip) 的文档。

既然我们已经编写了所有的程序逻辑，我们只需要向用户输出信息。这里我将使用 f 字符串，但是如果您愿意，也可以使用`format`或字符串连接。如果你使用连接，记住你只能连接字符串！

```py
`name = input("Please enter the employee's name: ").strip().title()
hourly_wage = input("What is their hourly wage? ")
hours_worked = input("How many hours have they worked this week? ")

earnings = float(hourly_wage) * float(hours_worked)

print(f"{name} earned ${earnings} this week.")` 
```

我们完成了！

希望你自己能够做到这一点，但如果没有，不要担心！编程很难，真正吸收这些新概念需要时间。坚持下去，如果需要的话，一定要复习最近几天的内容。

一如既往，如果你需要任何帮助，你可以在 [Discord](https://discord.gg/BBWwyMq) 上找到我们。

## 奖励材料

您可能注意到的一个小问题是，在打印员工的收入时，我们在小数点后有一个可变的精度。有几种方法可以解决这个问题，如果你有兴趣更深入地研究这个问题，你可以在下面找到一些方法。

一种选择是将我们的计算结果转换成整数。这将“截断”浮点，实质上是丢弃小数点后的所有内容。这当然看起来好多了，但我们丢掉了一些精度。

一个稍微好一点的选择是对浮点取整，而不是只修剪小数点后的值。在 Python 中舍入时，你需要注意一些微妙之处，但是我们有[一篇关于这个主题的文章，你可以阅读](https://blog.teclado.com/rounding-in-python/)。

这个问题的最终解决方案涉及到使用某种特殊的格式化语言，它是 Python 的一部分。它的一个特性是允许我们指定我们想要为我们的浮动显示多少个小数位。我们也有一个关于这个话题的帖子。

使用这些特殊的格式选项，我们的解决方案看起来是这样的:

```py
`name = input("Please enter the employee's name: ").strip().title()
hourly_wage = input("What is their hourly wage? ")
hours_worked = input("How many hours have they worked this week? ")

earnings = float(hourly_wage) * float(hours_worked)

print(f"{name} earned ${earnings:.2f} this week.")` 
```