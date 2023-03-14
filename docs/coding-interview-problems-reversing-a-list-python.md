# 编码面试问题:反转列表(Python)

> 原文：<https://blog.teclado.com/coding-interview-problems-reversing-a-list-python/>

列表或数组操作问题在编码面试中很常见。您可能需要执行的一种操作是反转列表:换句话说，将列表按相反的顺序排列。

在 Python 中，我们实际上有一个负责反转列表的内置方法，所以您的第一步肯定应该是询问您是否可以使用内置的`reverse`方法。

```
base = [1, 2, 3, 4, 5]
base.reverse() 
```

即使你的面试官说“不”，你至少证明了你知道这种方法的存在。

那么，让我们假设面试官说他们想看一个定制的解决方案。我们如何处理这个问题。

## 讨论这个问题

像这样的面试问题很大一部分是通过推理来解决问题。我们必须实现什么？我们将采取什么步骤来实现这个目标？我们如何验证解决方案是否有效？

先把这个做成一个具体的问题，而不是抽象的东西。如果我们有以下列表:

```
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9] 
```

我们想要的，是它像这样结束:

```
numbers_reversed = [9, 8, 7, 6, 5, 4, 3, 2, 1] 
```

我们可能想要做的一个非常有效的方法是从列表的开头移除条目，并将它们添加到末尾，直到我们重复这个过程，直到列表中的条目都存在为止。另一种选择可能是从末尾取出项目，并重复地将这些最后的项目添加到新的列表中。

在上面的列表中，我们有 9 个条目，我们可以使用`len`来计算长度，这意味着我们可以使用列表长度作为循环的终止条件。

Python 还有一个名为`pop`的方法，既可以从列表中移除一个项目，又可以返回它。这对于我们的情况来说显然非常方便，因为我们希望再次使用这个值，可能是作为一个`append`方法的一部分。默认情况下，`pop`删除列表中的最后一项，所以我们不需要提供任何参数。

每次我们从`pop`得到一个返回值，我们就知道它是当前链表的最终值，所以如果我们重复`append`这些最终值到一个新的链表中，我们应该会得到一个逆序的链表。

使用我们的具体例子，`pop`返回的第一个值将是`9`，因此这将是我们`append`的第一个值。我们`append`的下一个`pop`值将是`8`，以此类推，直到我们到达`1`，这是添加到新列表中的最终值。

## 编写代码

既然我们已经讨论了解决方案，实际的代码并不太具有挑战性。

我们可以创建一个名为`reversed_list`的新的空列表，但是我们也应该创建一个基本列表的副本，以便我们可以在以后检查它。记住`pop`将消耗来自`original_list`的值，所以当我们完成时，它将是空的。

对于实际的循环，我们将跟踪`original_list`的长度，正如我刚才提到的，当我们重复调用`pop`时，它将被消耗掉。我们将不断重复这个循环，直到没有更多的项目到`pop`为止。

对于每次迭代，我们得到一个由`pop`返回的新条目，并将其存储在一个`final_item`变量中，然后我们使用一个参数作为我们在`reversed_list`上调用的`append`方法的参数。

```
base = [1, 2, 3, 4, 5]
original_list = list(base)
reversed_list = []

while len(original_list) > 0:
    final_item = original_list.pop()
    reversed_list.append(final_item) 
```

您可以自行测试，但是这个解决方案应该非常好。

如果你愿意的话，你可以通过去掉对`final_item`的赋值，把`pop`方法直接放在`append`方法里面，这样就可以缩短一点。

```
base = [1, 2, 3, 4, 5]
original_list = list(base)
reversed_list = []

while len(original_list) > 0:
    reversed_list.append(original_list.pop()) 
```

结果完全相同，但是您可能会发现可读性较差。

## 使用黑魔法

我们上面的解决方案非常好，但是也有一些奇特的技巧可以用来解决这个问题。一种解决方案是使用像这样的切片:

```
original_list = [1, 2, 3, 4, 5]
reversed_list = original_list[::-1] 
```

这是一个非常酷的解决方案。它非常短，而且不消耗我们的原始列表。问题是，如果你不知道切片，这个语法绝对不会告诉你它做了什么。幸运的是，我们的变量有名字，这使得一切变得难以置信的清晰，但情况并非总是如此。

请不要在你的实际代码中使用它，至少不要用于列表。通常没有理由仅仅使用`reverse`就用这个。然而，这个技巧对于字符串或元组非常有用，因为它们没有很好的内置方法来反转顺序。如果你想了解更多关于切片有多棒，看看我们以前关于这个话题的帖子:[第一部分](https://blog.teclado.com/python-slices/)，[第二部分](https://blog.teclado.com/python-slices-part-2/)。

## 使用排序

使用`sorted`还有另一个一行程序解决方案，但是我还是不建议在野外使用它。

与 slice 语法相比，`sorted`的一个好处是，它至少在概念上很有意义。我们只是把名单倒过来排序。

```
base = [1, 2, 3, 4, 5]
original_list = list(base)
reversed_list = sorted(original_list, key=lambda x: base.index(x), reverse=True) 
```

不幸的是，实现并不那么简单。我们必须使用一个 lambda 函数来让`sorted`知道我们想要基于索引排序，而不是基于值本身。

总的来说，我不认为这是解决问题的好办法。我们的 while 循环长度相当，并且更容易阅读和理解。我们甚至可以把这个解决方案带给 Python 初学者，他们可以相对容易地解决这个问题。这里不是这样的。

**更新:**我们的一位读者指出了我在这篇文章中完全忽略的东西:包含重复元素的列表。因为`index`方法找到给定值的第一个实例的*的索引，所以使用`sorted`的解决方案对于具有重复元素的列表将变得不可靠。*

谢谢你，马克！

## 测试我们的解决方案

在整个过程中，我已经确保按照原始顺序保留原始列表项，因为这意味着我们可以测试我们的解决方案是否有效。现在我们只需要想出如何做到这一点。

### 关于列表需要知道的一些有用的事情

列表是按索引排序的，所以我们可以通过引用给定索引处的项目来讨论列表的内容。

根据我们开始时的列表:

```
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers_reversed = [9, 8, 7, 6, 5, 4, 3, 2, 1] 
```

在`numbers`列表中，索引`0`处的项目是一个值为`1`的整数对象，而在`numbers_reversed`中，索引`0`处的项目是一个值为`9`的整数对象。

我们也可以使用负索引来引用列表中的项，其中`-1`是指序列中的最后一项，`-2`是倒数第二项，`-3`是倒数第二项，依此类推。对于长度为`n`的列表，可以通过使用索引`0`或`-n`的引用来访问第一项。

如果列表颠倒，条目的当前索引和它的索引之间也有关系。了解这一点将让我们以编程方式检查列表反转的结果。

对于给定负指数的项目，其新的正指数将是负指数的绝对值(离零有多远)- 1。

对于给定正索引的项目，其新的负索引将是其当前正索引加 1，全部乘以负 1。

用代码写，这些关系看起来像这样。假设我们想在列表反转后检查索引`2`(即列表中的 3 号)是否在正确的位置:

```
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers_reversed = [9, 8, 7, 6, 5, 4, 3, 2, 1]

index_to_check = 2
negative_index = (index_to_check + 1) * -1  # -3
correct = numbers[index_to_check] == numbers_reversed[negative_index]

print(correct)  # True 
```

### 创建测试函数

使用这些信息，我们可以编写下面的函数来检查所有的东西都到了它应该去的地方:

```
def check_reversal(original_list, reversed_list):
    for index, item in enumerate(original_list):
        negative = (index + 1) * -1
        if reversed_list[negative] != item:
            print(f"Element {item} was not located at the expected index")
            break
    else:
        print("List reversed successfully") 
```

我们接受两个列表作为参数，尽管实际上我们传递它们的顺序并不重要:反转列表的逆列表毕竟是原始列表，所以我们的检查应该不管顺序如何。

我们获取第一个列表并调用`enumerate`来同时抓取条目和索引。我们将当前的正索引转换为反向列表中项目的负索引，并将该值赋给`new_neg`。

我们执行一个检查，以确保该项目在新列表中的正确位置结束，如果我们发现一个放错位置的项目，则中断循环，同时在控制台中打印一条消息。

如果循环运行完成，else 块将执行，让我们知道列表已经成功反转。如果你不熟悉这个 for / else 语法，我们有一个关于这个主题的简短 Python 片段，但是你也可以在[官方文档](https://docs.python.org/3/tutorial/controlflow.html?highlight=try%20else#break-and-continue-statements-and-else-clauses-on-loops)中找到更多信息。

阅读我们的[单元测试文章](https://blog.teclado.com/introduction-to-unit-testing-with-python/)的介绍，看看我们将如何为我们的列表反转函数编写测试。

## 包扎

感谢您的阅读。我希望你在这篇文章中学到了一些东西！

有什么问题吗？对我们发推文。考虑加入我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)以了解更多 Python 的优点，或者查看我们的[Python 自动化软件测试](https://www.udemy.com/automated-software-testing-with-python/?couponCode=BLOGGER)课程以深入了解测试 Python 和 web 应用程序。