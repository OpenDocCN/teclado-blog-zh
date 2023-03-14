# 用 Python 生成随机数集合

> 原文：<https://blog.teclado.com/python-generating-a-collection-of-random-numbers/>

欢迎回到另一篇 [Python 片段帖子](https://blog.teclado.com/tag/python-snippets/)。本周我们将学习使用[随机模块](https://docs.python.org/3.7/library/random.html#module-random)生成一组随机数。

在随机模块中，我们将关注两个函数:`choices`和`sample`。它们彼此非常相似，但是我们一会儿将要讨论的差异很少。

## `random.sample`

先说`sample`函数。`sample`以一个总体作为第一个自变量，它必须是一个序列或一个集合。这个群体就是随机值的来源。第二个参数叫做`k`，这个值决定了从总体中选择的项目数量。

```
import random

random_numbers = random.sample(range(10), k=3) 
```

这里`random_numbers`将是一个列表对象，包含从 0 到 9 的三个唯一整数。

注意，`range`是有效的`population`，因为`range`对象是不可变的序列类型。

我刚才提到，我们将从`sample`获得唯一的整数。这是因为`sample`只会从`population`中选择一个项目一次。然而，如果`population`包含重复的值，那么在我们的结果列表中有可能得到重复的值。

因为每个项目只能被选择一次，如果我们提供一个大于`population`长度的`k`值，我们得到一个`ValueError`。

## `random.choices`

`choices`函数的工作方式非常相似。我们从一个`population`开始，`choices`也接受一个`k`值作为参数，它决定了结果集合中有多少个值。

```
import random

random_numbers = random.choices(range(10), k=3) 
```

`sample`和`choices`的区别在于`choices`可以从`population`中多次选取相同的值。这也意味着我们可以请求超过总体长度的多个值，并且我们不会得到一个`ValueError`。我们将保证在结果列表中有一个重复的条目。

但是，空人口将导致`IndexError`。

除了一个`population`和一个`k`值，我们可以为每个值提供一个相对权重，改变该值出现在结果列表中的可能性。我们可以通过为`weights`参数提供一个参数来实现，这个参数必须是一个序列。

```
import random

random_numbers = random.choices(range(5), weights=[10, 10, 20, 10, 10], k=3) 
```

在这种情况下，值`2`被选择的可能性是任何其他值的两倍。

如果我们给`population`提供一个不同长度的`weights`序列，我们得到一个`TypeError`。

## 包扎

这星期到此为止！我希望您学到了一些关于使用 random 模块在 Python 中生成随机集合的新知识。虽然这些例子集中在数字上，但是人口可以是你想要的任何数量，我鼓励你去尝试！

如果你喜欢这篇文章，如果你能把它分享给你的朋友，我们会很感激，你可能想在 Twitter 上关注我们，以了解所有的内容。

如果你刚刚开始学习 Python，或者你只是想提升你的 Python 技能，你可能也会对完整的 Python 课程感兴趣。希望在那里见到你！