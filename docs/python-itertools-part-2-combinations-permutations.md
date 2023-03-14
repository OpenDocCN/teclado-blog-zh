# Python Itertools 第 2 部分——组合和排列

> 原文：<https://blog.teclado.com/python-itertools-part-2-combinations-permutations/>

在我们的[最后一篇文章](https://blog.teclado.com/python-itertools-part-1-product/)中，我们快速浏览了一下`itertools`模块中的`product`函数。今天我们要看看来自`itertools`模块的几个组合迭代器:`permutations`、`combinations`和`combinations_with_replacement`。

首先，我们来看看`permutations`。`permutations`关心的是为给定的商品集合找到所有可能的订单。例如，如果我们有一个字符串`"ABC"`，置换将找到我们可以重新排列这个字符串中的字母的所有方式，因此每个顺序都是唯一的。

```
from itertools import permutations

p_1 = permutations("ABC")
# ('A', 'B', 'C') ('A', 'C', 'B') ('B', 'A', 'C') ('B', 'C', 'A')
# ('C', 'A', 'B') ('C', 'B', 'A') 
```

默认情况下，`permutations`为整个集合返回不同的排序，但是我们可以使用可选的`r`参数来限制函数查找更短的排列。

```
p_2 = permutations("ABC", r=2)
# ('A', 'B') ('A', 'C') ('B', 'A') ('B', 'C') ('C', 'A') ('C', 'B') 
```

提供一个大于传递给`permutations`的集合长度的`r`值将产生一个空的置换对象。

现在我们来看看`combinations`。`combinations`返回一个 iterable 对象，该对象包含所提供集合中元素的唯一组合。注意，`combinations`与元素的顺序无关，所以`combinations`将把`('A', 'B')`视为与`('B', 'A')`结果相同。

结果组合的长度再次由`r`参数控制，但是在`combinations`的情况下，该参数是强制的。

```
from itertools import combinations

c_1 = combinations("ABC", r=2)
# ('A', 'B') ('A', 'C') ('B', 'C')

c_2 = combinations("ABC", r=3)
# ('A', 'B', 'C') 
```

有可能从`combinations`获得重复的元素，但是只有当提供的 iterable 包含给定元素的多个实例时。例如`(1, 2, 3, 1)`。

```
c_3 = combinations((1, 2, 3, 1), r=2)
# (1, 2) (1, 3) (1, 1) (2, 3) (2, 1) (3, 1) 
```

在这种情况下，`(1, 2)`和`(2, 1)`不仅仅是顺序不同的相同元素，1 实际上是原始集合中不同的元素。

使用`combinations_with_replacement`函数可以包含一个项目与其自身配对的实例。它就像`combinations`一样工作，但是也将匹配每个元素。

```
from itertools import combinations, combinations_with_replacement

c_4 = combinations((1, 2, 3), r=2)
# (1, 2) (1, 3) (2, 3)

c_5 = combinations_with_replacement((1, 2, 3), r=2)
# (1, 1) (1, 2) (1, 3) (2, 2) (2, 3) (3, 3) 
```

这就结束了组合迭代器！我希望你学到了一些新的东西，请务必查看官方文档以了解更多细节。

如果你想进一步提升你的 Python 技能，我们很乐意邀请你参加我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)。你可以点击这篇文章中的链接，以 9.99 美元的价格获得课程，并且还有 30 天的退款保证。

下周一我们将带着的另一个片段帖子[回来，这次涵盖了一个非常有趣的收藏类型。在](https://blog.tecladocode.com/python-deques)[推特](https://twitter.com/TecladoCode)上关注我们，或者注册我们下面的邮件列表，这样你就不会错过了！