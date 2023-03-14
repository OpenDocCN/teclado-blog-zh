# 仔细看看 Python 的打印功能

> 原文：<https://blog.teclado.com/python-a-closer-look-at-print/>

在这篇 [Python 片段帖子](https://blog.teclado.com/tag/python-snippets/)中，我们将仔细研究一个自从我们写下第一个“Hello，World！”以来一直在使用的函数在 Python 中。当然，我说的是`print`。

实际上有很多被忽略的功能，当然也不是大多数 Python 课程所涵盖的。例如，`print`接受任意数量的参数，允许我们一次传递几个值进行打印。

```
print(1, 2, 3, 4, 5)  # 1 2 3 4 5 
```

请注意，这与我们像这样传入单个值元组是不同的:

```
print((1, 2, 3, 4, 5))  # (1, 2, 3, 4, 5) 
```

相反，每个值都是一个单独的参数，各项用逗号分隔。

默认情况下，`print`将在打印输出中的每一项之间放置一个空格，但是我们可以使用`sep`关键字 only 参数来配置它。`sep`接受一个字符串作为值，这个字符串将被放在打印结果的每一项之间。

```
print(1, 2, 3, 4, 5, sep=", ")   # 1, 2, 3, 4, 5
print(1, 2, 3, 4, 5, sep=" | ")  # 1 | 2 | 3 | 4 | 5 
```

`print`还有另一个名为`end`的关键字参数，它允许我们配置打印行末尾的内容。默认值是`"\\n"`，它是换行符。这就是为什么每个`print`功能通常打印在不同的行上。

也许我们希望在特定的打印之后有一个空行。我们可以这样实现:

```
print(1, 2, 3, 4, 5, sep=" | ", end="\n\n") 
```

这里我们在`print`输出的末尾添加了两个换行符。这有利于处理数字或其他类型的字符，我们不能轻易连接一个新的字符。

`end`的另一个用途是从`print`输出的末尾移除换行符，允许我们将光标留在同一行。

我们要看的最后一个配置是`file`参数。`file`的默认值是`sys.stdout`，但是我们有几个有趣的选项。

第一个是`sys.stderr`，它允许我们打印一行，就像它是一个异常一样。这意味着它是红色的，因此从正常的`print`输出中突出出来。

```
import sys

print("You messed up!", file=sys.stderr) 
```

另一个有趣的选择是打印到任何旧文件。为了做到这一点，我们必须照常使用`open`函数来生成一个文件对象。为此，我建议使用上下文管理器:

```
with open("example.txt", "w") as f:
	print("Here is some file content", file=f) 
```

一般来说，你会想要使用`write`方法来代替这样的东西，但是`print`确实有接受非字符串类型的优势，所以有一些有效的用例。

## 包扎

这就是这篇文章的内容。我希望你学到了一些新东西，如果你有兴趣了解更多关于`print`的信息，一定要查看一下[官方文档](https://docs.python.org/3/library/functions.html#print)。

如果你刚刚开始你的 Python 之旅，你可能想看看我们的[完整的 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！我们会让你马上写出令人敬畏的 Python！

您还应该考虑订阅我们的邮件列表，以便及时了解我们的所有内容，并获得我们所有课程的常规折扣代码。