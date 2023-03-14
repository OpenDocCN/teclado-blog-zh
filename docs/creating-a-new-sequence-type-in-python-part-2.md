# 在 Python 中创建新的序列类型——第 2 部分

> 原文：<https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-2/>

欢迎回到关于使用类和 Python 的神奇方法在 Python 中创建新序列类型的系列文章。如果你没看第一部分，你可以在这里找到它。

在这篇文章中，我们将把重点放在向新的`LockableList`序列添加项目上，我们还将创建`lock`和`unlock`方法来改变我们的`LockableList`对象的可变性。我们也将实施期待已久的`__repr__`。

## 快速回顾一下

首先，让我们快速回顾一下到目前为止我们所做的工作。目前，我们的`LockableList`类是这样的:

```py
class LockableList:
    def __init__(self, *values, locked=False):
        self.values = list(values)
        self._locked = locked

    def __str__(self):
        return f"{self.values}"

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
```

我们有一个`__init__`方法，当我们的`LockableList`类的一个实例被创建时，这个方法被调用。这允许我们在创建一个`LockableList`的过程中接受值，比如一些初始数据，以及一个锁状态。

我们还定义了其他三种魔法方法:`__str__`、`__len__`和`__getitem__`。

允许我们在 Python 要求我们的对象的字符串表示时返回一些东西。一个例子可能是当我们将对象传递给内置的`print`函数时。

`__len__`允许我们返回对象的长度，表明序列包含多少项。这意味着我们可以在函数上调用`len`而不会引发错误。

是我们迄今为止实现的最复杂的方法。`__getitem__`允许我们访问列表中的特定项目或项目范围。如果我们像这样创建一个可锁定列表:

```py
friends = LockableList("Rolf", "John", "Anna") 
```

我们可以像访问列表一样访问项目:

```py
print(friends[0])    # Rolf
print(friends[1:3])  # ['John', 'Anna'] 
```

我们也可以使用 for 或 while 循环，或者使用 comprehension 来循环我们的`LockableList`对象。

## 添加`lock`和`unlock`方法

让我们从添加使我们的类型独一无二的方法开始:`lock`和`unlock`。

这里的目的是让我们修改我们的`LockableList`对象的锁定状态。当一个列表被锁定时，试图修改序列将会引发一个错误，就像我们试图修改一个元组一样。

`lock`和`unlock`方法本身非常简单。所有检查锁状态的逻辑都将在别处完成。`lock`和`unlock`只关心一件事:将`self._locked`的值从`True`改为`False`和*，反之亦然*。

```py
def lock(self):
    self._locked = True

def unlock(self):
    self._locked = False 
```

我们可以这样测试这个作品:

```py
friends = LockableList("Rolf", "John", "Anna")

friends.lock()
print(friends._locked)  # True

friends.unlock()
print(friends._locked)  # False 
```

## 添加`__repr__`

是一个重要的 dunder 方法，我们应该为我们创建的每个类实现它。很像`__str__`，`__repr__`返回一个特定对象的字符串表示，但是它有不同的功能。

考虑`__str__`和`__repr__`的一个简单方法是，`__str__`用于向用户显示信息，而`__repr__`是给开发者的信息。当我们实现`__repr__`时，我们希望提供重现对象所需的所有信息，越具体越好。例如，这可能包括模块名。

在我们的例子中，我们将展示如何定义一个`LockableList`对象，我们将展示重新创建调用`__repr__`的特定`LockableList`实例所需的参数。

```py
def __repr__(self):
    return f"LockableList({self.values})"

# LockableList(['Rolf', 'John', 'Anna']) 
```

然而，上面的表示没有准确描述如何创建我们关心的`LockableList`对象。相反，我们单独传入每个字符串，不带方括号，任何非字符串条目都将被添加。我们可以很容易地将一个列表、整数或集合存储为我们的`LockableList`的成员，每个都作为一个单独的参数传入。

因此，我们将对`self.values`中的每个项目调用`__repr__`，根据它们的类型，以值的形式为每个对象获取适当的表示。然后我们用`", "`作为连接字符串`join`每个值。

```py
def __repr__(self):
    values = ", ".join([value.__repr__() for value in self.values])
    return f"LockableList({values})"

# LockableList('Rolf', 'John', 'Anna') 
```

这样，我们的`LockableList`类看起来像这样:

```py
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

    def lock(self):
        self._locked = True

    def unlock(self):
        self._locked = False

test = LockableList("Rolf", "John", "Anna", 3, ["Hello", "World"])
print(repr(test))  # LockableList('Rolf', 'John', 'Anna', 3, ['Hello', 'World']) 
```

## 实施`__setitem__`

现在是开始实现`__setitem__`的时候了，这将让我们将新的对象分配给我们的`LockableList`的特定索引。换句话说，我们可以做这样的事情:

```py
friends = LockableList("Rolf", "John", "Anna")
friends[1] = "Jose" 
```

此时，如果我们尝试这样做，Python 会引发一个异常:

```py
TypeError: 'LockableList' object does not support item assignment 
```

实现`__setitem__`也将允许我们执行片分配。如果你对这个话题不熟悉，通读我们的[高级切片贴](https://blog.teclado.com/python-slices-part-2/)是一个好主意，以提高速度。

同样，我们将需要一个不同的过程来处理项目分配和片分配，并且我们还将不得不处理无效的索引，无论它们是否超出范围，或者它们是否是一个`TypeError`的结果。

在`__setitem__`的情况下，我们还必须注意我们的`LockableList`的锁定状态，我们必须确保当用户试图修改锁定序列时引发异常。

### 检查锁定状态

首先，让我们定义新的`__setitem__`方法，并添加一些逻辑来检查`LockableList`是否确实被锁定。如果是，我们可以养一只`RuntimeError`。

对于`__setitem__`，我们将指定三个参数:`self`，因为我们需要访问`self._locked`；`i`代表某个指标或指标范围；和`values`，它代表我们想要添加到我们的`LockableList`对象中的项目。

```py
def __setitem__(self, i, value):
    if self._locked == True:
        raise RuntimeError(
            "LockableList object does not support item assignment while locked"
        ) 
```

检查锁状态就像使用 if 语句检查`self._locked`的当前值一样简单。如果`self._locked`是`True`，我们立即提高我们的`RuntimeError`，否则我们将尝试添加指定的项目到序列中。

### 添加单项

一旦我们确定我们的`LockableList`是解锁的并且能够突变，我们就可以开始给序列分配项目。

我们对`__setitem__`的实现看起来将非常类似于对`__getitem__`的实现。首先，我们检查索引是否为负，如果是，我们尝试将其转换为正索引。然后，我们检查索引是否在这个特定的`LockableList`实例的有效范围内，如果不在，就抛出一个`IndexError`。

我们实现的不同之处在于我们如何处理没有错误发生的情况。我们不是返回给定索引处的项，而是给它赋值。由于我们在内部使用一个列表来存储我们的值，这应该相对容易:

```py
def __setitem__(self, i, values):
    if self._locked == True:
        raise RuntimeError(
            "LockableList object does not support item assignment while locked"
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
```

### 实现切片分配

在为切片实现`__setitem__`时，我们需要记住一些事情:

1.  切片允许不对称分配。我们可以分配比容纳它们的空间更多的值。
2.  我们也可以分配比我们替换的项目更少的项目，序列中的后续值将移动以填充剩余的空间。
3.  使用扩展切片时，除非步长值为 1，否则这些特殊属性不适用。

严格来说，这些限制并不是由片本身强加的，而是由 list 如何实现片赋值强加的。由于我们试图模仿内置列表类型的功能，我们也将施加这些限制。

我们可以采取简单的方法，让底层列表为我们做所有的工作，但是这有什么意思呢？让我们从头开始做尽可能多的事情。

### 扩展切片

让我们首先创建一个`elif`分支来捕获切片对象。我们可以使用`indices`来获得对应于这个特定序列的`start`、`stop`和`step`值，就像我们在[第 1 部分](https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-1/)中对`__getitem__`所做的那样。

`rng`将包含序列中的特定索引，这些索引对应于所提供的切片对象，就像前面一样。

我们现在可以执行一个检查，看看步长值是否为 1 以外的任何值。如果是，我们知道`rng`的`len`一定等于传入`__setitem__`的`values`的`len`。如果这个条件不满足，我们就抛出`ValueError`。

```py
elif isinstance(i, slice):
    start, stop, step = i.indices(len(self.values))
    rng = range(start, stop, step)
    if step != 1:
        if len(rng) != len(values):
            raise ValueError(
                "attempt to assign a sequence of size {} to extended slice of size {}"
                .format(len(values), len(rng))
            ) 
```

如果我们没有遇到任何异常，我们需要遍历`rng`中的索引，并一次一个地分配`values`中的每个项目。

在本例中，我将使用`zip`创建一系列元组，第一个值是`rng`中的一个索引，第二个值是`values`中的一个条目。

```py
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
```

现在，我们可以将一个步长值不为 1 的扩展切片赋值，如下所示:

```py
friends = LockableList("Rolf", "John", "Anna")

friends[0:3:2] = ["Jose", "Mary"]
print(friends)  # ["Jose", "John", "Mary"]

friends[2::-2] = ["Rolf", "Anna"]
print(friends)  # ["Anna", "John", "Rolf"] 
```

### 标准切片分配

最后，我们可以处理切片的标准分配情况。这有一点复杂，因为我们可能需要将底层列表分割以适应值。

幸运的是，我们可以在这里使用切片的一些属性来帮助我们。我们已经有了所提供的切片对象`i`的`start`和`stop`值。我们也知道切片的`start`指标是包含性的，而`stop`不是。

因此，我们可以这样做:

```py
self.values = self.values[:start] + values + self.values[stop:] 
```

在这里，我们从内部列表的开始到我们的切片的开始取所有值的切片，不包括。换句话说，直到第一个切片项目的所有内容。我们将想要添加到序列中的值追加到这个值范围中。

然后我们追加另一组值，这次是从切片的`stop`索引到序列的末尾。

然后将所有这些分配给`self.values`，用这个新序列替换内部列表。

我们需要做的最后一件事是处理用户传入无效索引的情况，就像我们对`__getitem__`所做的那样。

对于我们所有的新代码，我们最终会得到这样的结果:

```py
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
        if self._locked == True:
            raise RuntimeError(
                "LockableList object does not support item assignment while locked"
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

friends = LockableList("Rolf", "John", "Anna")
friends[1:2] = ["Jose", "Mary"]

print(friends)  # ['Rolf', 'Jose', 'Mary', 'Anna'] 
```

我希望你喜欢这个系列。我们将很快[回来](https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-3/)为我们新的序列类型添加*甚至更多的*功能。在那之前，看看`__add__`、`__radd__`和`append`方法，看看你自己是否能从头开始实现它们。你可以在[官方文档](https://docs.python.org/3/reference/datamodel.html#special-method-names)中找到关于`__add__`和`__radd__`方法的信息。

## 包扎

如果你正在寻找更全面的东西，你可能想试试我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)。它长达 35 个小时，包含练习、项目和测验，可以帮助你很好地理解 Python。我们认为这太棒了，我们相信你也会的！

你可能也想考虑注册我们下面的邮件列表！