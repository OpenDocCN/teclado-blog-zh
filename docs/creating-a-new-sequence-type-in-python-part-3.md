# 在 Python 中创建新的序列类型——第 3 部分

> 原文：<https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-3/>

欢迎来到 Python 特殊方法系列的第 3 部分。在本帖中，我们将介绍一个叫做操作符重载的特性，以及我们如何使用 Python 的特殊方法来实现这个特性。为此，我们将继续我们的旅程来创建我们的新序列类型(即`LockableList`)，因此如果您还没有跟上这个系列，请务必查看[第 1 部分](https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-1/)和[第 2 部分](https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-2/)。

## 快速回顾一下

虽然我建议看一看我们以前的帖子，以获得完整的代码分解，但是到目前为止，我们已经创建了一个类来描述我们新的`LockableList`序列类型。在这个类中，我们定义了许多特殊的或“dunder”方法，从`__init__`开始。

当我们的类的一个实例被创建时，`__init__`被自动调用，在这里我们接受一些初始值，以及一些关于我们的列表的锁定状态的配置。

我们接下来有`__str__`和`__repr__`方法，它们返回给定`LockableList`对象的字符串表示，`__str__`提供用户友好的输出，而`__repr__`更面向使用我们代码的开发人员。

我们还定义了`__getitem__`和`__setitem__`，它们负责使用索引或切片对象在序列中检索、更新和添加值。

最后，我们定义了两个名为`lock`和`unlock`的普通方法，它们允许我们更新给定`LockableList`对象的锁定状态。当被锁定时，`LockableList`不能变异，其行为非常像元组。

以下是迄今为止完整的类定义:

```
class LockableList:
    def __init__(self, *values, locked=False):
        self.values = list(values)
        self._locked = locked

    def __str__(self):
        return f"{self.values}"

    def __repr__(self):
        values = ", ".join([value.__repr__() for value in self.values])
        return f"LockableList({values})"

    def __len__(self):
        return len(self.values)

    def __getitem__(self, i):
        if isinstance(i, int):
            # Perform conversion to positive index if necessary
            if i < 0:
                i = len(self.values) + i

            # Check index lies within the valid range and return value if possible
            if i < 0 or i >= len(self.values):
                raise IndexError("LockableList index out of range")
            else:
                return self.values[i]
        elif isinstance(i, slice):
            start, stop, step = i.indices(len(self.values))
            rng = range(start, stop, step)
            return LockableList(*[self.values[index] for index in rng])
        else:
            invalid_type = type(i)
            raise TypeError(
                "LockableList indices must be integers or slices, not {}"
                .format(invalid_type.__name__)
            )

    def __setitem__(self, i, values):
        if self._locked:
            raise RuntimeError(
                "LockedList object does not support item assignment while locked"
            )

        if isinstance(i, int):
            # Perform conversion to positive index if necessary
            if i < 0:
                i = len(self.values) + i

            # Check index lies within the valid range and assign value if possible
            if i < 0 or i >= len(self.values):
                raise IndexError("LockableList index out of range")
            else:
                self.values[i] = values
        elif isinstance(i, slice):
            start, stop, step = i.indices(len(self.values))
            rng = range(start, stop, step)
            if step != 1:
                if len(rng) != len(values):
                    raise ValueError(
                        "attempt to assign a sequence of size {} to extended slice of size {}"
                        .format(len(values), len(rng))
                    )
                else:
                    for index, value in zip(rng, values):
                        self.values[index] = value
            else:
                self.values = self.values[:start] + values + self.values[stop:]
        else:
            invalid_type = type(i)
            raise TypeError(
                "LockableList indices must be integers or slices, not {}"
                .format(invalid_type.__name__)
            )

    def lock(self):
        self._locked = True

    def unlock(self):
        self._locked = False 
```

## 什么是运算符重载？

运算符重载是许多语言的一个特性，它允许我们为现有的运算符定义与某些操作数类型相关的新行为。在 Python 中，我们使用特殊的方法来实现这种行为，我们之前已经提到过这种行为。注意，我们可以使用下标符号通过索引访问`LockableList`项。

```
l = LockableList(1, 2, 3)
print(l[2])  # 3 
```

这是操作符重载的一个例子，因为我们已经重新定义了这些方括号在与`LockableList`对象结合使用时的行为。

我们可以为我们能想到的任何操作符定义行为，每个操作符对应一个通常被合理命名的特殊方法。例如，`*`运算符对应于`__mul__`，而`+`对应于`__add__`。

然而，这并不是故事的结尾。如果你仔细阅读文档，你也会发现`__radd__`和`__iadd__`，那么这是怎么回事呢？

当 Python 遇到类似于`+`的二元操作符时，Python 首先检查左边的操作数，以获得如何对相关类型执行操作的信息。如果不存在这样的信息，Python 会尝试右边的操作数，但是这次会寻找相关特殊方法的`r`版本。因此我们有特殊的方法，如`__radd__`、`__rmul__`、`__rsub__`。

特殊方法的版本代表一种就地操作。这是一个不需要赋值的操作。

```
a = 5
a = a + 1  # __add__

b = 5
b += 1  # __iadd__ 
```

这些特殊方法的`i`版本对我们来说非常重要，因为当我们想要实现对我们的`LockableList`对象的适当修改时，我们想要在`LockableList`被锁定时阻止这样的操作。

## `__add__`、`__radd__`和`__iadd__`

让我们从`__add__`开始。首先，我们必须考虑当我们试图连接`LockableList`对象以及其他类型的对象时，我们想要发生什么。一个重要的问题是，我们将排除哪些类型？

在我的实现中，我将遵循列表类型的例子，除了我将接受列表和`LockableList`对象。结果类型将总是一个新的`LockableList`对象，并且新对象将使用默认的锁定状态。

最终这些类型的决定完全取决于你，所以如果你不喜欢我在这方面的选择，你可以写你自己的类来实现不同于我的功能。您可能希望只允许与其他`LockableList`对象连接，或者如果任何一个操作数被锁定，您可能希望将新对象设置为锁定。你真的可以为所欲为。

### 实施`__add__`

```
def __add__(self, other):
    if isinstance(other, (list, LockableList)):
        return LockableList(*(self.values + other))

    invalid_type = type(other)
    raise TypeError(
        'can only concatenate list or LockableList (not "{}") to LockableList'
        .format(invalid_type.__name__)
    ) 
```

这里我们用两个参数定义`__add__`:`self`和`other`。这些名称完全由惯例决定，实际上您可以随意命名。

在我们的例子中，`self`通常指的是当前的`LockableList`对象，而`other`指的是`+`操作符的右边操作数。我们不知道我们的用户会尝试将什么古怪的东西连接到我们的`LockableList`对象上，所以我们需要进行一些检查。

一个简单的 if 语句和内置函数`isinstance`就足够了。如果你不了解`isinstance`，可以在[官方文档](https://docs.python.org/3/library/functions.html#isinstance)中了解更多。

本质上，`isinstance`将允许我们验证给定的对象是某种类型的。我们可以用一个元组指定许多类型，正如你在上面看到的，我们指定了`list`和`LockableList`。如果`other`是`list`或`LockableList`的实例，`isinstance`将返回`True`。

如果我们找到一个有效的类型，我们可以简单地将`other`的值连接到我们用于存储的内部列表，并将新列表解包到一个新的`LockableList`对象中。这里有一个问题，我们一会儿会讲到。

如果用户试图连接一个无效的类型，我们会引发一个`TypeError`。这与我们之前在这个项目中使用的错误非常相似。

有了它，我们可以做一些很酷的事情:

```
numbers_one = LockableList(1, 2, 3)
numbers_two = [4, 5, 6]

print(numbers_one + numbers_two)  # [1, 2, 3, 4, 5, 6] 
```

但是正如我提到的，有一个问题:

```
numbers_one = LockableList(1, 2, 3)
numbers_two = LockableList(4, 5, 6)

print(numbers_one + numbers_two)  # TypeError 
```

记住，在内部，我们使用一个列表来存储我们的`LockableList`中的值。我们对`__add__`的返回值是`self.values + other`。

`self.values`是列表；`other`是一个`LockableList`，所以当我们使用`+`时，我们最终调用列表的`__add__`方法，列表只允许与其他列表连接。我们因此得到一个`TypeError`。

解决方案是实现`__radd__`，这样当列表的方法产生错误时，Python 可以依靠定义的方法`LockableList`。

### 实施`__radd__`

我们对`__radd__`的实现将与`__add__`几乎相同，但是`other`和`self.values`的顺序颠倒了。这是为了根据操作数的顺序保持值的顺序。

```
def __radd__(self, other):
    if isinstance(other, (list, LockableList)):
        return LockableList(*(other + self.values))

    invalid_type = type(other)
    raise TypeError(
        'can only concatenate list or LockableList (not "{}") to LockableList'
        .format(invalid_type.__name__)
    ) 
```

现在我们可以避免之前出现的错误:

```
numbers_one = LockableList(1, 2, 3)
numbers_two = LockableList(4, 5, 6)

print(numbers_one + numbers_two)  # [1, 2, 3, 4, 5, 6] 
```

### 实施`__iadd__`

最后，我们来照顾一下`__iadd__`。

有一点不同，因为我们必须注意我们的锁状态，但是我们实现的其余部分是相当相似的。

```
def __iadd__(self, other):
    if self._locked:
        raise RuntimeError(
            "LockedList object does not support in-place concatenation while locked"
        )

    if isinstance(other, (list, LockableList)):
        self.values = self.values + list(other)
        return self

    invalid_type = type(other)
    raise TypeError(
        'can only concatenate list or LockableList (not "{}") to LockableList'
        .format(invalid_type.__name__)
    ) 
```

在`__iadd__`的情况下，我们直接更新内部列表，然后返回一个对当前对象的引用。这允许我们保留相同的对象，同时更新包含在给定的`LockableList`中的值。

```
numbers_one = LockableList(1, 2, 3)
numbers_two = LockableList(4, 5, 6)
numbers_one += numbers_two

print(numbers_one)  # [1, 2, 3, 4, 5, 6] 
```

### 关于`__iadd__`的说明

值得注意的一件有趣的事情是，在我们实现`__iadd__`之前，我们的类已经能够处理`+=`增加的算术运算符。你自己试试吧。

如果是这样的话，我们为什么还要费心去实现`__iadd__`？`__iadd__`确实是一项优化功能。在没有`__iadd__`的情况下，Python 将退回到`__add__`方法，但是我们的`__add__`方法创建了一个新的`LockableList`对象，并且创建一个对象不是免费的。通过实现`__iadd__`,我们可以绕过这个新对象的创建，节省一点计算时间。

### 已完成的课程

这样，我们的类定义变得非常大，但是我们已经完成了很多:

```
class LockableList:
    def __init__(self, *values, locked=False):
        self.values = list(values)
        self._locked = locked

    def __str__(self):
        return f"{self.values}"

    def __repr__(self):
        values = ", ".join([value.__repr__() for value in self.values])
        return f"LockableList({values})"

    def __len__(self):
        return len(self.values)

    def __getitem__(self, i):
        if isinstance(i, int):
            # Perform conversion to positive index if necessary
            if i < 0:
                i = len(self.values) + i

            # Check index lies within the valid range and return value if possible
            if i < 0 or i >= len(self.values):
                raise IndexError("LockableList index out of range")
            else:
                return self.values[i]
        elif isinstance(i, slice):
            start, stop, step = i.indices(len(self.values))
            rng = range(start, stop, step)
            return LockableList(*[self.values[index] for index in rng])
        else:
            invalid_type = type(i)
            raise TypeError(
                "LockableList indices must be integers or slices, not {}"
                .format(invalid_type.__name__)
            )

    def __setitem__(self, i, values):
        if self._locked:
            raise RuntimeError(
                "LockedList object does not support item assignment while locked"
            )

        if isinstance(i, int):
            # Perform conversion to positive index if necessary
            if i < 0:
                i = len(self.values) + i

            # Check index lies within the valid range and assign value if possible
            if i < 0 or i >= len(self.values):
                raise IndexError("LockableList index out of range")
            else:
                self.values[i] = values
        elif isinstance(i, slice):
            start, stop, step = i.indices(len(self.values))
            rng = range(start, stop, step)
            if step != 1:
                if len(rng) != len(values):
                    raise ValueError(
                        "attempt to assign a sequence of size {} to extended slice of size {}"
                        .format(len(values), len(rng))
                    )
                else:
                    for index, value in zip(rng, values):
                        self.values[index] = value
            else:
                self.values = self.values[:start] + values + self.values[stop:]
        else:
            invalid_type = type(i)
            raise TypeError(
                "LockableList indices must be integers or slices, not {}"
                .format(invalid_type.__name__)
            )

    def __add__(self, other):
        if isinstance(other, (list, LockableList)):
            return LockableList(*(self.values + other))

        invalid_type = type(other)
        raise TypeError(
            'can only concatenate list or LockableList (not "{}") to LockableList'
            .format(invalid_type.__name__)
        )

    def __radd__(self, other):
        if isinstance(other, (list, LockableList)):
            return LockableList(*(other + self.values)) 

        invalid_type = type(other)
        raise TypeError(
            'can only concatenate list or LockableList (not "{}") to LockableList'
            .format(invalid_type.__name__)
        )

    def __iadd__(self, other):
        if self._locked:
            raise RuntimeError(
                "LockedList object does not support in-place concatenation while locked"
            )

        if isinstance(other, (list, LockableList)):
            self.values = self.values + other
            return self

        invalid_type = type(other)
        raise TypeError(
            'can only concatenate list or LockableList (not "{}") to LockableList'
            .format(invalid_type.__name__)
        )

    def lock(self):
        self._locked = True

    def unlock(self):
        self._locked = False 
```

## 从这里去哪里

如果你觉得这个项目很有趣，我建议你自己尝试实现`__mul__`，以及它的变体方法。当然，如果你愿意，你可以走得更远:这完全取决于你自己！你可以在[官方文档](https://docs.python.org/3/reference/datamodel.html#special-method-names)中找到大量关于 Python 特殊方法的信息。

我希望通过这个简短的系列文章，您已经了解了 Python 的特殊方法是如何工作的，并且我希望您能在未来的项目中找到新的有趣的方法来使用它们。

如果你有兴趣更全面地了解 Python 中的面向对象编程，你可能想看看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！有超过 35 个小时的材料，所以有很多让你咬紧牙关。