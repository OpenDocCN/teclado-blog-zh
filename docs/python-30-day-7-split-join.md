# 第 7 天:分割、连接和切片| Teclado

> 原文：<https://blog.teclado.com/python-30-day-7-split-join/>

欢迎来到 Python 系列 [30 天的第 7 天！今天我们将回到集合，我们将主要讨论字符串和其他集合类型如列表和元组之间的转换。我们将学习如何使用切片来获取集合的子集。](https://blog.teclado.com/30-days-of-python/)

如果你错过了[第 6 天](/30-days-of-python/python-30-day-6-for-loops)，如果你不知道以下任何一项，我建议你查看一下:

*   `for`循环
*   `break`声明
*   `range`功能

## 将元组和列表转换为字符串

元组和列表有一个字符串表示，但这通常不是我们想要的。如果我们试图将一个列表传递给`str`来创建一个新的字符串，就像这样:

```py
`numbers = [1, 2, 3, 4, 5]
numbers = str(numbers)` 
```

如果我们试着打印出来，我们会看到这样的东西:

你们中的一些人可能已经注意到，如果我们试图打印一个列表，这正是我们得到的。这是因为当我们将一个列表传递给`print`，`print`会为我们将列表转换成一个字符串。

就代码而言，这就是你最终得到的结果。包含方括号、数字和逗号的字符串:

```py
`numbers = "[1, 2, 3, 4, 5]"` 
```

然而，在这种情况下，我实际上想要的是这样的东西:

例如，也许我有一个描述谁在某个项目中工作的名字列表，我想将这些名字作为句子的一部分打印出来:

```py
`The people who worked on this project are: Mike, Sofia, Helen.` 
```

我不想要的是:

```py
`The people who worked on this project are: [Mike, Sofia, Helen].` 
```

我们问题的解决方案是`join`方法。

我们在一个字符串上调用`join`,这个字符串就是我们想要放在集合中想要粘合在一起的项目之间的内容。在上面的示例中，我们只想使用逗号和空格连接这些值，因此我们的代码如下所示:

```py
`project_authors = ["Mike", "Sofia", "Helen"]
project_authors = ", ".join(project_authors)

print(f"The people who worked on this project are: {project_authors}.")` 
```

现在输出正是我们想要的:

```py
`The people who worked on this project are: Mike, Sofia, Helen.` 
```

我们可以通过将`join`调用放在 f 字符串的花括号内来实现同样的事情:

```py
`project_authors = ["Mike", "Sofia", "Helen"]

print(f"The people who worked on this project are: {', '.join(project_authors)}.")` 
```

只是要小心你的报价！如果您使用相同的引号来定义连接字符串，那么您将很快切断外部字符串。

使用`join`时需要注意的一点是，我们只能连接字符串集合。如果我们有一个数字列表，我们必须首先将每个数字转换成一个字符串。

*   我们从一系列数字开始，称为`numbers`。
*   然后我们创建一个空列表，`stringified_numbers`。这将把我们的数字转换成字符串。
*   使用一个`for`循环，我们遍历`numbers`列表。我们将每个数字转换成一个字符串，并将其附加到`stringified_numbers`。
*   最后，我们用`join`搭配`stringified_numbers`。

```py
`numbers = [1, 2, 3, 4, 5]

stringified_numbers = []

for number in numbers:
    stringified_numbers.append(str(number))

print(', '.join(stringified_numbers)) # 1, 2, 3, 4, 5` 
```

我知道这看起来很麻烦，但是有更有效、更简洁的方法来做这样的事情，我们将在本系列的后面探索。

## 分割字符串

通常，我们不是将一个集合组合成一个字符串，而是将一个字符串转换成另一个包含几个项目的集合。例如，在处理用户输入时，如果用户提供多个值来响应一个提示，我们可能希望这样做。

一种常见的方法是使用`split`方法。当调用`split`时，我们经常需要传入第二个字符串，我们将使用它作为分隔符序列。每当遇到这个字符序列，就标志着一个值的结束和下一个值的开始。

例如，假设我们想从用户那里获得五个数字，我们要求这些数字用逗号分隔。我们可能会这样写:

```py
`user_numbers = input("Please enter 5 numbers separated by commas: ") # 1,2,3,4,5
numbers_list = user_numbers.split(",")

print(numbers_list) # ['1', '2', '3', '4', '5']` 
```

这里我们指定了一个逗号作为分隔符序列，所以每个逗号都标记了我们想要添加到集合中的值的结尾。

我们可以要求用户输入用破折号分隔的数字，然后我们必须传递一个破折号给`split`作为分隔符:

```py
`user_numbers = input("Please enter 5 numbers separated by dashes: ") # 1-2-3-4-5
numbers_tuple = user_numbers.split("-")

print(numbers_tuple) # ['1', '2', '3', '4', '5']` 
```

我们从`split`得到的是一个列表，但是我们总是可以通过将结果传递给`tuple`来创建另一种类型的集合，例如，如果这是我们想要的:

```py
`user_numbers = input("Please enter 5 numbers separated by commas: ") # 1,2,3,4,5
numbers_tuple = tuple(user_numbers.split(","))

print(numbers_tuple) # ('1', '2', '3', '4', '5')` 
```

需要注意的一点是`split`不会为我们去掉任何空白。如果用户写的数字带有逗号，后跟空格，这些空格将出现在我们放入新集合的字符串中:

```py
`user_numbers = input("Please enter 5 numbers separated by commas: ") # 1, 2, 3, 4, 5
numbers_list = user_numbers.split(",")

print(numbers_list) # ['1', ' 2', ' 3', ' 4', ' 5']` 
```

有时候这并不是一个真正的问题，但是有时候你可能需要使用一个`for`循环来遍历集合，并使用`strip`来清理。

```py
`user_numbers = input("Please enter 5 numbers separated by commas: ") # 1, 2, 3, 4, 5
user_numbers = user_numbers.split(",")

numbers_list = []

for number in user_numbers:
    numbers_list.append(number.strip())

print(numbers_list) # ['1', '2', '3', '4', '5']` 
```

您也可以使用同样的方法以其他方式处理单个字符串。

如果我们不为`split`指定一个分隔符序列，它将根据任何长度的空白进行分割。这是非常方便的，因为我们知道无论我们得到什么，我们都不必费心去剥离它。用户可以输入如下内容:

我们仍然会得到这样一个列表:

```py
`['1', '2', '3', '4', '5']` 
```

如果我们处理跨不同行的字符串，这也是可行的！稍后会有更多的介绍。

我们并不总是需要调用`split`。如果我们只想将每个字符作为不同的条目放入一个列表或元组中，我们可以将字符串传递给`list`或`tuple`函数:

```py
`sample_string = "Python"

print(list(sample_string)) # ['P', 'y', 't', 'h', 'o', 'n']
print(tuple(sample_string)) # ('P', 'y', 't', 'h', 'o', 'n')` 
```

## 换行符

你可能想知道的一件事是，我们如何告诉计算机我们应该换行？

实际上，当我们按下“回车”来标记行尾时，会添加一个看不见的字符。如果你进入一个文字处理器，你按下“回车”进入下一行，当你按下“退格”时会发生什么？我们删除了一些东西，这让我们回到了上一行。你删除的是标志行尾的换行符。

在 Python 中，我们将这个字符表示为`\n`，我们可以像其他任何字符一样在字符串中使用它。例如，我可以写一个这样的字符串:

```py
`print("Super Special Mega Awesome Program\n\nBy Phillip Best")` 
```

这里我在某个程序的标题后添加了两个换行符，然后是关于程序作者的信息。输出将如下所示:

```py
`Super Special Mega Awesome Program

By Phillip Best` 
```

如果我们愿意，我们可以使用这个字符连接字符串，也可以使用`\n`拆分字符串，但是这对用户输入来说并不实用。然而，它在处理文件时非常有用，这是我们下周要讨论的内容。

## 限幅

到目前为止，我们已经研究了如何拆分字符串，以及如何连接其他集合的值，但是如果我们只想获取集合的某个*部分*呢？为此，我们可以使用切片。

切片是我们对序列使用订阅表达式的一种新方法。我们没有提供单一的索引，而是指定了一个索引范围。我们将得到的是一个与我们切片的类型相同的集合，包含指定范围内的所有项目。切片的一个伟大之处在于，它给了我们一个新的系列，而没有触及原来的系列。

让我们看一个字符串的例子。假设我们有一个字符串`Python`，出于某种原因，我想要这个字符串的前 3 个字符。使用切片，我们可以这样写:

```py
`original_string = "Python"
sliced_string = original_string[0:3]

print(sliced_string)  # Pyt` 
```

在这里，我们的切片被写成了`[0:3]`，这意味着我们希望从索引`0`开始，我们希望抓取到索引`3`之前的所有项目，但不包括索引`0`。我们得到的是一个只包含三个字符的新字符串:`"Pyt"`。

我们可以用另一种方式来写这个片段，就像这样:`[:3]`。如果我们没有为开始索引指定一个值，切片从序列的开始处开始。如果不指定停止索引，切片将在序列的末尾停止。

所以如果我们写这个`[3:]`，这意味着给我从索引`3`开始的所有内容。我们可以在这里看到一个例子:

```py
`original_string = "Python"
sliced_string = original_string[3:]

print(sliced_string)  # hon` 
```

回到第 4 天，我们看到可以使用负索引来访问序列中的元素，也可以对片使用负索引。然而，理解`[3:-1]`和`[3:]`不是一回事非常重要，因为对于切片，停止索引是不包含的。

当我们写`[3:-1]`时，我们说我们想要从索引`3`到最后一个元素的所有内容，但不包括最后一个元素。当我们写`[3:]`时，我们说我们想要从索引`3`开始的所有内容。我们可以看到这里的区别:

```py
`original_string = "Python"

print(original_string[3:])  # hon
print(original_string[3:-1])  # ho` 
```

如果您出于某种原因想要复制整个集合，我们实际上可以使用`[:]`获取包含整个序列的切片。

切片实际上是一个非常多才多艺的工具，还有很多东西需要学习。如果你感兴趣，在这篇文章的末尾有一些额外的资源，它们更详细地讨论了切片。

## `len`功能

如果我们想知道给定集合中有多少项，我们可以使用`len`函数，简称“length”。

这将适用于字符串、元组、列表以及我们将在本系列后面看到的任何其他类型。如果你试图在一个不支持这个操作的类型上调用`len`，你会得到一个类似这样的异常:

```py
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    len(12)
TypeError: object of type 'int' has no len()` 
```

假设将一个适当的类型传递给`len`，它将返回一个表示集合中项目数量的整数:

```py
`numbers = [1, 2, 3, 4, 5]
len(numbers) # 5` 
```

## 练习

1)要求用户输入他们的名和姓来响应一个提示。使用`split`提取名称，然后将每个名称分配给不同的变量。在本练习中，您可以假设用户只有一个名和一个姓。

2)使用`join`方法以`1 | 2 | 3 | 4 | 5`的格式打印列表`[1, 2, 3, 4, 5]`。请记住，您只能连接字符串集合，因此您需要对数字列表进行一些初步处理。

3)下面是简短的报价列表:

```py
 `quotes = [
    "'What a waste my life would be without all the beautiful mistakes I've made.'",
    "'A bend in the road is not the end of the road... Unless you fail to make the turn.'",
    "'The very essence of romance is uncertainty.'",
    "'We are not here to do what has already been done.'"
 ]` 
```

每个引号都是一个字符串，但是每个字符串实际上都在开头和结尾包含引号字符。使用切片，从每个字符串中提取文本，去掉多余的引号，并打印每个引号。

您可能还想尝试使用 strip 的解决方案。

4)让用户输入一个单词，然后打印出单词的长度。您应该考虑用户输入中任何多余的空格，所以您必须在找到字符串长度之前对其进行处理。

如果你想更进一步，你可以要求用户输入一段很长的文本。然后你可以告诉他们文本中总共有多少个字符，你也可以给他们一个字数。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-7-exercise-solutions)

## 项目

一旦你完成了上面的练习，你应该试试[周末项目](/30-days-of-python/python-30-day-7-project)，在那里我们将处理高预算电影。

## 额外资源

如果你想了解更多关于切片的知识，我们有三个帖子你可以看看。

前两个是一个小系列，涵盖了关于 Python 切片的所有知识:[第 1 部分](https://blog.teclado.com/python-slices/)、[第 2 部分](https://blog.teclado.com/python-slices-part-2/)。

第三篇文章是关于如何使用切片来创建一个新的序列，其中项目的顺序是相反的。我建议你在阅读这篇文章之前先通读前两篇文章。