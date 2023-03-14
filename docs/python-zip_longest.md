# Python 的 zip_longest 函数

> 原文：<https://blog.teclado.com/python-zip_longest/>

在[上周的片段帖子](https://blog.teclado.com/python-zip/)中，我们谈到了极其强大的`zip`函数，以及我们如何使用它来使我们的循环更加 Pythonic 化。本周我们将谈论`zip`不太为人所知的兄弟:`zip_longest`。

`zip_longest`住在`itertools`模块里，在之前我们已经简单地谈过了[。`itertools`包含各种围绕迭代运算的有用函数。](https://blog.teclado.com/python-itertools-part-1-product/)

那么`zip_longest`和普通的老式`zip`有什么不同呢？我们为什么要关心它？

嗯，当我们使用`zip`时，一旦其中一个元素用完了，`zip`就会停止组合我们的 iterables。如果其他条目更长，我们就把多余的条目扔掉。看一下这个例子:

```
l_1 = [1, 2, 3]
l_2 = [1, 2]

combinated = list(zip(l_1, l_2))

print(combinated)  # [(1, 1), (2, 2)] 
```

如你所见，我们刚刚在`l_1`中失去了那个`3`。这有时会很成问题。我们通常不希望像这样丢弃数据。

幸运的是我们有`zip_longest`来拯救我们。

让我们再看看上面的例子。这次使用`zip_longest`。

```
from itertools import zip_longest

l_1 = [1, 2, 3]
l_2 = [1, 2]

combinated = list(zip_longest(l_1, l_2, fillvalue="_"))

print(combinated)  # [(1, 1), (2, 2), (3, '_')] 
```

这里有几点需要注意。首先，我们在 zip 对象中获得了第三个元组，这意味着更长的列表能够提供它的所有值。其次，我们有这个关键字参数叫做`fillvalue`。

如果我们查看我们的小代码段的`print`输出，我们可以得到一些这是做什么的指示。正如我们所看到的，没有值匹配到`3` , `zip_longest`将`3`匹配到我们的`fillvalue`。

本质上，任何时候`zip_longest`都没有与我们的 iterable 元素匹配的值，它会将这个`fillvalue`放在那里以填补空白。

我们真的可以在这里使用任何我们想要的`fillvalue`:数字、列表、字典、`None`。无论你能想到什么？这使得它的用途非常广泛，绝对是值得了解的。

如果您感兴趣，我们可以不带`fillvalue`参数调用`zip_longest`，在这种情况下，它将默认使用`None`作为`fillvalue`。

## 包扎

这篇文章就写到这里吧！我希望你学到了一些新的东西，并且我确信你会发现在你自己的代码中使用`zip_longest`的各种令人敬畏的方法。

和往常一样，如果你真的想提高你的 Python，我推荐你看看我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)！它有超过 35 个小时的材料，以及测验、练习和几个大型项目，所以这是一个发展您的 Python 技能的极好方式。

你也应该考虑注册我们下面的邮件列表，因为我们为我们的课程定期发布优惠券代码，确保你总是得到最好的交易。

编码快乐！