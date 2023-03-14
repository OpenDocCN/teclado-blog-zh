# 在 Python 中创建新的序列类型——第 1 部分

> 原文：<https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-1/>

在这篇文章中，我们将会看到 Python 的魔术或“dunder”方法，我们将会使用这些魔术方法从头开始创建一个全新的序列类型。

我们将要创建的新序列类型称为`LockableList`。它将拥有常规列表的所有核心特性，但是我们将通过调用`LockableList`对象上的`lock`方法来选择使其不可变。如果我们想让它再次可变，我们只需要在我们的`LockableList`上调用`unlock`，之后我们将能够再次修改值。

下面是它如何工作的一个例子:

```
friends = LockableList("Rolf", "Bob", "Jen")
friends.append("Adam")
print(friends)  # ['Rolf', 'Bob', 'Jen', 'Adam']

friends.lock()
friends.append("Anne")  # Error 
```

这篇文章和随后的部分将涉及一些相对高级的材料，但是我们有一些资源可以帮助你。我们将和班级一起工作，所以如果你需要一个简短的复习，你可以看看[这篇文章](https://blog.teclado.com/introduction-to-object-oriented-programming-in-python/)。我们还将为我们的`LockableList`实现切片，所以如果你不太熟悉切片是如何工作的，我们有关于这个主题的[介绍](https://blog.teclado.com/python-slices/)和[更高级的帖子](https://blog.teclado.com/python-slices-part-2/)。如果你想要更全面的东西，你可能会对我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/)感兴趣。

所有这些都解决了，让我们开始吧！

## 定义我们的`LockableList`类

首先，如果我们需要为我们的`LockableList`定义一个类。在该类中，我们将创建属性来存储列表的值，并跟踪对象的锁定状态。我们还将定义大量的方法，这些方法将允许我们重新创建 Python 的标准列表类型的行为，只是有我们自己的特殊变化。

```
class LockableList:
	def __init__(self):
		pass 
```

我们已经遇到了我们的第一个特殊方法:`__init__`。与我们将要定义的其他方法不同，`__init__`在创建对象实例时被自动调用，它允许我们设置一些初始值。现在，我们的`__init__`方法做得不太好，所以让我们来解决这个问题。

```
class LockableList:
	def __init__(self, values):
		self.values = values 
```

我们现在已经为`__init__`定义了第二个参数，称为`values`，这意味着我们现在可以在创建我们的`LockableList`的实例时传入一些参数。我们传入的值随后存储在`self.values`中。我们可以这样检查这个作品:

```
l = LockableList([1, 2, 3, 4, 5])
print(l.values)  # [1, 2, 3, 4, 5] 
```

除了存储一组值，我们还需要跟踪我们的锁状态。默认情况下，我认为创建我们的`LockableList` unlocked 是有意义的，如果用户愿意，可以选择指定锁定状态。

```
class LockableList:
	def __init__(self, values, locked=False):
		self.values = values
		self._locked = locked 
```

出于上述原因，我在这里使用默认参数值`False`作为我们的锁定状态。请注意，我还在`locked`属性前面加了一个下划线。这只是一个约定，用于表示该变量仅供内部使用。

总的来说，这是一个好的开始，但我认为我们可以做得更好。

## 改进我们创造`LockableList`的方式

目前我们有一点小问题。如果用户像这样创建一个`LockableList`,会发生什么:

```
l = LockableList(2, 6) 
```

我们一直假设我们的用户只是要传入一个列表或一些其他集合来填充我们的`LockableList`，但是没有什么可以阻止他们做一些像我们在上面的例子中所做的事情。

在这种情况下，我们的`_locked`状态将采用值`6`，但是我们期望一个布尔值。稍后我们将检查`_locked`的真实性，像`6`这样的值将评估为`True`，即使最初的`_locked`值应该是`False`。

更糟糕的是，values 变量只包含`2`，所以我们不小心丢弃了一些用户数据。

现在假设用户在初始化他们的`LockableList`时输入了第三个值。现在他们将得到一个`TypeError`，因为他们提供了太多的参数。不太好。

一种解决方案是去掉`values`参数，用`*values`代替。这里重要的部分是`*`，它实际上为我们做了两件事:

1.  它允许我们接受任意数量的位置参数(`*`语法的主要用途)。
2.  它仅使我们的`locked`参数成为关键字，因为它出现在`*values`之后。

太好了，所以我们防止了用户意外覆盖默认的`_locked`状态，并且我们也使`LockableList`的设置变得更简单了。用户现在可以写`LockableList(1, 4, 6, 2)`，而不需要传入一个集合。

那么我们如何访问`*values`中的值呢？

默认情况下，`*values`收集的值将放在一个元组中，可以通过引用`values`参数来访问。这不是我们真正想要的。首先，元组是不可变的对象，使用可变类型作为我们的内部存储将使我们的生活更加容易。

我将把它转换成一个列表，而不是使用标准的`values`元组:

```
class LockableList:
	def __init__(self, *values, locked=False):
		self.values = list(values)
		self._locked = locked 
```

这样，我们现在将用户的值作为标准 Python 列表存储在内部。

## 打印我们的内容`LockableList`

早些时候，我们成功地打印出了我们的`LockableList`实例`l`的内容，但是感觉非常笨拙。当我们打印一个普通的列表时，我们可以只使用一个变量名，我们不必引用`list`类的一些内部结构。我希望我们的`LockableList`以同样的方式工作。

事实证明，我们可以使用另一种特殊的方法来完成这项工作。当我们调用`print`时，Python 会查看我们作为一个名为`__str__`的方法的参数传入的对象内部。如果它找不到，它就会寻找一个叫做`__repr__`的，我们稍后会讲到。

`__str__`允许我们定义给定对象的用户友好表示。在我们的例子中，我认为使用标准的列表语法来表示我们的`LockableList`是相当安全的，但是我们真的可以返回我们想要的任何东西。我们需要记住的唯一规则是它必须返回一个字符串。

```
class LockableList:
	def __init__(self, *values, locked=False):
		self.values = list(values)
		self._locked = locked

	def __str__(self):
		return f"{self.values}" 
```

因为我们已经在内部使用了一个列表来存储一个`LockableList`中的值，所以我们可以使用`{self.values}`来获取这个列表并将其放入一个 f 字符串中。现在我们可以这样做:

```
friends = LockableList("John", "Rolf", "Mary")
print(friends)  # ['John', 'Rolf', 'Mary'] 
```

好多了！

## 返回一个`LockableList`的长度

Python 有一个方便的内置函数叫做`len`,它让我们可以测量物体的大小。对于集合，这通常是它们包含的对象的数量。

如果我们试图得到一个`LockableList`的`len`会发生什么？

```
friends = LockableList("John", "Rolf", "Mary")
print(len(friends))  # TypeError: object of type 'LockableList' has no len() 
```

那是不行的。

幸运的是，Python 再次有了一个特殊的方法，我们可以实现它来提供这个功能:`__len__`。

在这一点上，你应该注意到一些模式。我们希望序列类型具有一些功能，为了提供这些功能，我们求助于一个特殊的方法。这些方法的名称通常也非常恰当，并且与我们想要使用的内置方法或操作紧密相关。

实现`__len__`是一个相对简单的任务。在内部，我们使用一个标准的列表来存储用户的值，并且一个列表已经实现了`len`，所以我们不需要自己计算值。

```
class LockableList:
	def __init__(self, *values, locked=False):
		self.values = list(values)
		self._locked = locked

	def __str__(self):
		return f"{self.values}"

	def __len__(self):
		return len(self.values)

friends = LockableList("John", "Rolf", "Mary")
print(len(friends))  # 3 
```

## 按索引获取项目

到目前为止，我们可以打印出一个`LockableList`中的所有条目，并且可以找出集合中有多少条目，但是我们还不能通过它们的索引来访问特定的条目。

如果你读了我们的博客文章[中的切片](https://blog.teclado.com/python-slices-part-2/)，你会知道当我们写`friends[2]`时，Python 使用一个叫做`__getitem__`的方法来检索该索引处的项目。它使用相同的方法来检索某个序列的切片。

正如我们所看到的，这是另一个 dunder 方法，我们可以在我们的`LockableList`中实现它。

在我们编写任何代码之前，当涉及到我们的`__getitem__`实现时，我们需要考虑一些事情。

1.  首先，我们需要区分切片和对单个索引的引用，因为我们需要不同地处理`__getitem__`的这两种用法。
2.  其次，我们需要考虑如何处理负索引。

从技术上来说，我们可以将负索引直接传递给内部链表，但是我们将会想象我们没有这种奢侈。处理切片也是如此。

让我们首先考虑如何处理负索引。如果用户请求索引`-1`处的项目，他们想要序列中的最后一个项目。在一个长度为`5`的序列中，索引`-1`和索引`4`是同一个东西。这些数字之间有一个非常清晰的联系，这将允许我们处理负指数:序列的长度加上负指数等于负指数的正指数版本。

### 获取单个项目

这样，我们可以实现`__getitem__`的第一部分来处理特定索引项的检索:

```
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
```

首先，我们使用`isinstance`来检查为`i`传入的值的类型。具体来说，我们检查它是否是一个整数。如果是的话，我们知道我们可以用它来访问序列中的特定项。

现在，我们将忽略任何其他输入类型，但是我们将在以后处理它们。

一旦我们知道我们正在处理一个整数，我们就检查给我们的索引是否是负的。如果是的话，我们知道我们需要用上面描述的方法把它转换成一个正指数。

如果提供给我们的索引大于或等于我们序列的长度，我们知道它一定在我们拥有的值的范围之外，因为我们的索引从`0`开始。因此，我们的最高指标是序列 1 的长度。类似地，如果他们提供一个大的负指数，它可能超过我们序列的长度。

如果用户试图访问一个不存在的索引值，我们将引发一个`IndexError`。

最后，如果我们没有遇到任何问题，我们将返回内部列表中与用户请求的索引相对应的条目。耶！

这实际上非常重要，因为我们现在可以对我们的`LockableList`对象做些别的事情:我们可以对它们进行循环！

```
friends = LockableList("John", "Rolf", "Mary")

for friend in friends:
	print(friend) 
```

## 拿回一份我们的`LockableList`

现在我们可以从我们的`LockableList`中获取单个项目，我们需要处理切片。

记住，当抓取某个序列的切片时，比如通过做`friends[1:3]`，Python 将`1:3`切片对象传递给`__getitem__`。

我们应该做的第一件事是检查`index`是否属于类型`slice`。如果不是(也不是一只`int`，那么我们需要养一只`TypeError`。

一旦我们知道我们正在处理一个切片，我们就可以利用`indices`方法，它将返回我们为调用它的切片对象传入的特定序列的具体范围值。

```
friends = ["John", "Rolf", "Mary", "Adam", "Jose"]
my_slice = slice(-3, -1, 1)  # akin to [-3:-1]
my_slice.indices(len(friends))  # (2, 4, 1) 
```

在上面的例子中，我们有一个切片对象，`slice(-3, -1, 1)`。然后，我们在这个切片对象上调用`indices`,并将`friends`作为参数传递。因为`friends`的长度为`5`，所以我们得到一个这样的元组:`(2, 4, 1)`。它包含一个开始索引、一个非包含停止索引和一个步长值。

我们可以将这些值直接放入`range`中，以创建我们应该返回值的索引序列。然后，我们可以循环遍历所需的索引，并通过解包一个 list comprehension 将值作为一个新的`LockableList`返回。

```
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

这样我们现在可以接受切片对象作为`__getitem__`的参数！

## 包扎

如果你喜欢这篇文章，请务必在下周回来查看[，那时我将实现`__getitem__`的对应物，用于修改序列:`__setitem__`。在那之前，先试验一下代码，看看还能自己实现什么。你可以在这里找到很多 Python 特殊方法](https://blog.teclado.com/creating-a-new-sequence-type-in-python-part-2/)[的细节](https://docs.python.org/3/reference/datamodel.html#special-method-names)。