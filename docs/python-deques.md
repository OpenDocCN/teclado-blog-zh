# Python Deques

> 原文：<https://blog.teclado.com/python-deques/>

双端队列是来自于`collections`模块的一种方便的集合类型，允许从集合的任意一端进行高效的`append`和`pop`操作。它们还附带了一个非常有用的`rotate`方法，用于从队列的一端获取一个项目，并将其附加到另一端。

`collections`模块是 Python 标准库的一部分，但是我们仍然需要导入它来使用 deques。

```
from collections import deque 
```

在这里，我们可以通过向`deque`类传递一个 iterable 来创建一个 deque。这可能是一个列表，一个集合，甚至是一个字符串。

注意，要创建包含单个字符串的 dequee 对象，我们必须将该字符串作为另一个 iterable 对象中的一个元素传入，否则每个字符都将是结果 dequee 中的一个单独的元素。

```
oops = deque("abc")  # deque(['a', 'b', 'c']) 
```

一旦我们有了一个包含我们想要处理的项的 deque，我们就可以使用许多与使用常规列表相同的方法。这包括`append`、`count`、`copy`、`clear`等几个。然而，我们也获得了一些特定于 deques 的新方法。

第一种注法是`appendleft`。这就像`append`一样，但是把新元素放在索引`0`处。`appendleft`有一个对应的`popleft`，其功能与标准列表的`pop(0)`完全相同。deques 的一个限制是它们的`pop`方法不接受任何参数，并且总是弹出集合中的最后一项。

```
base = deque([1, 2, 3])

x = base.pop()  # 3
base.appendleft(x)  # deque([3, 1, 2])

y = base.popleft()  # 3
base.append(y)  # deque([1, 2, 3]) 
```

使用 deques 时，我们可以使用的另一个非常有趣的方法是`rotate`。`rotate`允许我们将一个项目从队列的一端`pop`到另一端`append`。

```
base = deque([1, 2, 3])
base.rotate()  # deque([3, 1, 2]) 
```

在上面的例子中，3 从右边弹出并附加到左边。

然而，我们并不局限于单次旋转，我们可以通过传入一个数字和相关的符号作为参数来控制旋转的角度和方向。默认情况下，队列向右旋转，但提供负旋转值将导致其向左旋转。

```
base = deque([1, 2, 3, 4, 5])

# rotates base 2 steps to the left
base.rotate(-2)  # deque([3, 4, 5, 1, 2])

# rotates base 3 steps to the right
base.rotate(3)   # deque([5, 1, 2, 3, 4]) 
```

## 包扎

这就是我们对德克的简要介绍！如果你想了解更多关于 deques 的知识，你可以在[官方文档](https://docs.python.org/3.7/library/collections.html#collections.deque)中阅读它们，以及`collections`模块中所有其他酷的集合类型。

我们也在我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)中涵盖了类似的内容，所以如果你想进一步提升你的 Python 技能，就来看看吧。

如果你喜欢这个小帖子，请与你的朋友分享，并确保在 [Twitter](https://twitter.com/TecladoCode) 上关注我们，或者注册下面的邮件列表，以了解我们所有的最新内容！