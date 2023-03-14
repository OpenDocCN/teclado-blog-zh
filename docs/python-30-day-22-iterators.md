# 第 22 天:迭代器| Teclado

> 原文：<https://blog.teclado.com/python-30-day-22-iterators/>

欢迎来到 Python 系列 [30 天的第 3 周！今天我们要来看看迭代器！](https://blog.teclado.com/30-days-of-python/)

虽然我们没有遇到这个术语*迭代器*，但是我们实际上已经看到了很多迭代器。例如，任何时候我们调用`zip`、`enumerate`、`map`或`filter`，我们得到的是一个迭代器。

今天，我们将深入了解它们是什么，为什么它们很重要，以及我们需要注意的一些问题。

## 什么是迭代器？

迭代器在 Python 中有一个技术定义,但是我们还没有涵盖正确理解这个定义所需的许多概念。尽管如此，我们仍然可以通过一些相对直观的概念来大致了解迭代器是什么。

让我们从谈论 *iterables* 开始，因为这是一个我们在整个系列中多次使用的术语，也是一个相关的术语。

iterable 实际上是我们可以用类似于`for`循环的东西迭代的任何值。我们也可以析构 iterables，我们可以通过将值传递给像`list`或`tuple`这样的函数来创建集合。

以下是我们到目前为止遇到的一些可迭代类型:

*   用线串
*   列表
*   元组
*   字典
*   设置
*   `range`物体
*   `zip`物体
*   `enumerate`物体
*   `map`物体
*   `filter`物体

如果我们想要一个非常宽泛的笔画定义，那么一个可迭代对象实际上就是我们一次可以从一个对象中获取值的东西。

我们应该问自己的一个问题是，我们如何掌握这些价值观？Python 怎么知道给我们什么？

这个问题的答案是，它使用迭代器。如果可迭代是我们可以从中获取值的东西，那么迭代器就是给我们这些值的东西。它们是 Python 用来从 iterables 中提取值的机制。

从我们的可迭代列表中可以看到，我在这篇文章的介绍中提到的*迭代器*也是*可迭代器*。事实上，*所有的*迭代器都是可迭代的。

这与迭代器的定义有技术上的原因，但也有一些直观的意义。

当我们想在迭代中从 iterable 中获取值时，我们会说，

> "给我负责提供你的值的迭代器."

然后我们接触迭代器，它开始向我们释放值，一次一个。

如果我们直接问迭代器，它会说，

> “其实，*我*负责发布数值，所以，*我*可以给你。”

### 重要的

并非所有迭代器都从另一个 iterable 中检索值。许多迭代器自发地为我们生成值。

在 [itertools 模块](https://docs.python.org/3.8/library/itertools.html)中有很多这种迭代器的例子。

这是因为迭代器是可迭代的(这意味着我们可以对它们进行迭代以获得我们想要的值)，迭代器能够在没有另一个迭代器的帮助下向我们提供值。迭代器只需要有一种在内部生成值的方法，而不是提供一种让我们访问在别处定义的值的方法。

## 迭代器的重要性质

迭代器有几个我们应该知道的重要而有用的属性。

### 迭代器通常很懒

首先，大多数迭代器是懒惰类型，尤其是那些在标准库中定义的迭代器。这意味着他们不需要预先计算所有的价值。我们在一些非迭代器类型中也遇到过这种情况，比如`range`对象。

懒惰带来了一些显著的内存好处，因为我们只需要在内存中保存最近的值。一旦我们把它用在我们正在做的任何事情上，我们就可以把它扔掉，潜在地释放内存。

在计算方面也有一些潜在的节省，因为我们可能不需要迭代器提供的每一个值。如果我们只关心前 3 个值，就没有必要去计算接下来的 200 个值。

然而，这些好处不是免费的:其中有一些妥协。

首先，通常不可能确定一个惰性类型的长度，因为它包含的项数可能只有在计算这些值之后才变得明显。

例如，假设我们试图过滤一个单词列表，这样我们只保留以`"a"`开头的单词。如果你不熟悉`filter`和`methodcaller`，看看[第 20 天](/30-days-of-python/python-30-day-20-map-filter)。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)` 
```

很明显，在我们实际执行过滤操作之前，我们无法确定`a_words`的长度。然而，`filter`要等到我们向它要值的时候才能计算任何东西。因此我们不可能知道`a_words`到底有多长。

同样，我们也不可能轻易看出`filter`对象包含什么*值*。为了找到答案，我们必须从迭代器中请求值，这可能会导致一些额外的问题，我们马上就会看到。

### 迭代器值被消耗

这是需要牢记的非常重要的一点，因为这是经常绊倒新 Python 开发人员的常见“陷阱”。

使用我们上面的`filter`例子可以很容易地证明这一点。让我们使用一个`for`循环来迭代这些值，看看过滤后还剩下什么。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

for word in a_words:
    print(word)` 
```

到目前为止一切顺利。我们将所有以`"a"`开头的单词打印到控制台上。

现在，如果我们添加另一个`for`循环来做完全相同的事情，会怎么样呢？

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

for word in a_words:
    print(word)

for word in a_words:
    print(word)` 
```

我们得到完全相同的输出。

我们通常期望名字打印两次，但事实并非如此。

让我们试着在`for`循环后将`a_words`转换成一个列表，看看里面有什么。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

for word in a_words:
    print(word)

a_words = list(a_words)
print(a_words)  # []` 
```

我们得到的是一个空列表。

发生这种情况的原因是因为在我们迭代完我们的`iterator`之后，没有什么可以放入我们的新列表。值已经提供给我们了，迭代器不会再给我们第二次。

如果您在检查代码时试图查看迭代器内部的内容，这通常会导致问题，因为通过解包迭代器并打印其值，您正在消耗这些值。如果在`print`调用之后有依赖于这些值的代码，您可能会得到一个意外的异常，或者至少是意外的行为。

### 对可变集合的更改会影响迭代器

让我们再次使用我们的过滤例子。

这一次我们将从我们的`filter`创建`a_words`，然后我将对`words`列表做一些修改。然后我们将打印`a_words`来看看这有什么影响，如果有的话。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

words.append("apple")

for word in a_words:
    print(word)` 
```

尽管我们在修改`words`之前已经创建了`filter`迭代器，但是新单词仍然包含在`filter`中。

```py
`anaconda
anime
addition
apple` 
```

这是懒惰的另一个副作用，而且很有道理。当我们定义`a_words`时，`filter`实际上并没有做任何过滤，它也没有存储自己版本的列表来获取它的值。它只是引用了内存中已经存在的那个。

如果我们修改那个列表，然后我们要求`filter`在那个改变之后从列表中过滤值，它甚至不知道新的条目被添加。它并不在乎。它会一直给我们提供价值，直到用完为止。

## 使用`next`功能进行手动迭代

现在我们对迭代器有了一个概念，并对它们的属性有了一点了解，让我们转向一些有趣的使用方法。

我们使用迭代器的一个非常有用的方法是手动迭代。也就是说，一次从一个迭代器中请求一个值，而不需要解包或`for`循环等工具的帮助。

我们从迭代器中请求新项的方法是使用`next`函数。当我们调用`next`时，它期望引用一个迭代器。这对常规的可重复项无效！

首先，让我们试着抓取以`"a"`开头的`words`列表中的第一个单词。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

first_word = next(a_words)
print(first_word)  # "anaconda"` 
```

当然，我们也可以直接将`next`的返回值传递给`print`。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

print(next(a_words))  # "anaconda"` 
```

正如我们所看到的，我们得到了`"anaconda"`，这是`words`列表中的第一个`"a"`单词。

如果我们再次调用`next`，我们得到`"anime"`:在`words`中的第二个`"a"`字。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

print(next(a_words))  # "anaconda"
print(next(a_words))  # "anime"` 
```

那么，这个的实际应用是什么呢？

还记得从[第 14 天](/30-days-of-python/python-30-day-14-files)开始的虹膜数据吗？最初，我们使用类似下面的方法来获取文件数据并提取文件头。

```py
`with open("iris.csv", "r") as iris_file:
    iris_data = iris_file.readlines()

headers = iris_data[0].strip().split(",")
irises = []

for row in iris_data[1:]:
    iris = row.strip().split(",")
    iris_dict = dict(zip(headers, iris))

    irises.append(iris_dict)` 
```

这种方法工作得很好，但是如果这是一个非常大的文件，它就不是最好的方法，原因有两个:

1.  通过使用`readlines`,我们立即将整个文件作为一个列表存储在内存中。
2.  通过在`for`循环中使用切片，我们在内存中创建了另一个列表，它包含了几乎所有与第一个列表相同的数据。

我们可以通过使用迭代器来避免这一切。原来,`open`函数已经给了我们一个迭代器，所以我们不需要做太多工作就能完成。

```py
`irises = []

with open("iris.csv", "r") as iris_file:
    headers = next(iris_file).strip().split(",")

    for row in iris_file:
        iris = row.strip().split(",")
        iris_dict = dict(zip(headers, iris))

        irises.append(iris_dict)` 
```

这里我们从从`iris_file`迭代器请求文件的第一行开始。然后我们像往常一样处理这个字符串，并将其分配给`headers`。

请记住，这消耗了值，所以当我们迭代`iris_file`时，我们只会从第 2 行开始。

此外，我们一次只处理一行，并且在处理完之后不会保留该行。我们只需要创建我们需要的字典，然后我们丢弃原来的字符串以支持新的字符串。与我们之前所做的相比，这非常节省内存。

根据我们想要对这些数据做什么，有一些方法可以完成我们所有的计算，而不必创建一些集合来存储所有的值。

## `StopIteration`异常

手动迭代非常有用，但是我们要小心一点，因为我们可能会遇到一个叫做`StopIteration`的异常。虽然不完全是一个错误，但如果我们不正确处理这个异常，我们的程序仍然会终止。

在[的第 19 天](/30-days-of-python/python-30-day-19-exception-handling)，我们谈了一些关于`StopIteration`的事情，但不是很深入。简单提醒一下，当我们迭代一个 iterable 时，如果它没有更多的值可以提供给我们，就会引发`StopIteration`异常。

当我们使用一个`for`循环时，或者当我们析构一个集合时，这个异常会被处理，但是当我们执行手动迭代时不会。

例如，如果我们再次查看我们的`filter`示例，如果我们试图请求四个项目，我们将得到一个`StopIteration`异常引发。

```py
`from operator import methodcaller

words = ["anaconda", "peach", "gravity", "cattle", "anime", "addition"]
a_words = filter(methodcaller("startswith", "a"), words)

print(next(a_words))  # "anaconda"
print(next(a_words))  # "anime"
print(next(a_words))  # "addition"

print(next(a_words))  # StopIteration` 
```

因此，在执行手动迭代时，使用`try`语句捕捉任何潜在的`StopIteration`异常是一个非常好的主意。

## 练习

1)下面你会发现一个包含几个充满数字的元组的列表:

```py
`numbers = [(23, 3, 56), (98, 1034, 54), (254, 344, 5), (45, 2), (122, 63, 74)]` 
```

2)使用`map`函数找出每个元组中数字的总和。使用手动迭代打印由结果`map`对象提供的前两个结果。

3)假设你有 3 名员工，并且已经商定员工将在晚上轮流锁门。这意味着，对于员工 A、B 和 C，员工 A 将在第 1 天关闭商店，然后 B 将在第 2 天关闭商店，C 将在第 3 天关闭商店，然后我们再次从员工 A 开始循环。

编写一个程序来创建一个时间表，列出在 30 天的时间里，你的哪些员工将在某一天锁上商店。您应该列出日期、员工姓名和星期几。您可以选择任何员工在第 1 天锁定商店，也可以选择第 1 天对应于一周中的哪一天。

您应该利用`itertools`模块中的`cycle`函数来创建一系列重复的值。你可以在这里找到文件。

一旦你完成了练习，你可以在这里对照我们的[检查你的答案。](/30-days-of-python/python-30-day-22-exercise-solutions)