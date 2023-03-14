# 第 11 天:布景|台克拉多

> 原文：<https://blog.teclado.com/python-30-day-11-sets/>

欢迎来到 Python 系列 [30 天的第 11 天！在这篇文章中，我们将讨论另一种叫做集合的集合类型。集合对于比较值的集合非常有用，并且对于这种操作是高度优化的。](https://blog.teclado.com/30-days-of-python/)

集合与字典密切相关，所以如果你还不熟悉字典，我强烈建议你看看这个系列的第[天。我们在这里将要讨论的大部分内容都是建立在我们在那篇文章中学到的基础上的。](/30-days-of-python/python-30-day-10-dictionaries/)

## 设置

集合与我们到目前为止看到的集合有些不同，因为它们没有可靠的排序。如果我们创建一个列表或一个元组并打印它，我们知道这些项将按照我们定义的顺序显示。这不是一个集合的情况。

集合也只能包含唯一的元素，就像我们看到的字典键一样。事实上，我们真的可以把一个集合想象成一个只包含键的字典。

那么我们为什么要关心集合呢？首先，它们提供了许多非常有用的操作来比较集合的成员。因此，它们对于过滤值非常有用，对集合执行这种成员测试也非常有效。

## 定义集合

很像字典，我们使用花括号定义集合，但是我们只是添加一系列逗号分隔的值，而不是键值对。

```py
`vegetables = {"carrot", "lettuce", "broccoli", "onion", "carrot"}` 
```

在这一组中，我们有两次字符串`"carrot"`。这不会产生错误，但是集合将只包含一个`"carrot"`的实例。我们上面定义的集合与下面的集合完全相同。

```py
`vegetables = {"carrot", "lettuce", "broccoli", "onion"}` 
```

正如我们在[第 10 天](/30-days-of-python/python-30-day-10-dictionaries/)中看到的，我们可以使用`{}`定义一个空字典。因此，我们不能使用这个选项来定义空集。相反，我们必须这样写:

如果我们打印一个空的集合，我们也会得到这个`set()`表示，以区别输出和空字典。

有些值我们不能包含在集合中，就像我们看到的字典键一样。限制实际上是完全一样的，所以我们不能在一个集合中包含任何可变类型，或者包含可变类型的任何不可变类型。

正因为如此，我们不能向集合中添加列表或字典，也不能添加包含列表或字典之类的东西的元组。

我们也不能将集合包含在其他集合中，因为集合也可以被修改。因此，以下内容在 Python 中是非法的:

```py
`nested_sets = {{1,  2,  3},  {"a",  "b",  "c"}}` 
```

如果我们尝试，我们会得到一个`TypeError`:

```py
`Traceback (most recent call last):
  File "main.py", line 1, in <module>
    nested_sets = {{1, 2, 3}, {"a", "b", "c"}}
TypeError: unhashable type: 'set'` 
```

## 修改集合

在 Python 中，我们有几种修改集合的方法，其中大部分选项与字典中的选项相同。

### 向器械包添加物品

我们有几个向器械包添加新物品的选项。

首先我们有`add`方法，它允许我们将一个值添加到我们调用它的集合中:

```py
`vegetables = {"carrot", "lettuce", "broccoli", "onion"}
vegetables.add("potato")

print(vegetables)  # {'lettuce', 'broccoli', 'onion', 'potato', 'carrot'}` 
```

请注意，打印输出与我们定义的集合顺序不同。如果您自己运行代码，您的输出也可能是不同的顺序。

如果我们试图添加一个已经存在于集合中的项目，则集合保持不变。

如果我们想一次添加几个项目，我们可以使用`update`方法。在这种情况下，我们可以传入任何 iterable，如果有效，它包含的项将被添加。

```py
`vegetables = {"carrot", "lettuce", "broccoli", "onion"}
vegetables.update(["potato", "pumpkin"])

print(vegetables)  # {'broccoli', 'lettuce', 'carrot', 'potato', 'pumpkin', 'onion'}` 
```

在这两种情况下，如果我们试图向集合中添加一个可变项，我们将得到一个类似于我们之前看到的`TypeError`。

### 从集合中删除项目

我们也有很多从集合中删除项目的选项。

首先我们有`remove`方法，它允许我们删除单个项目。我们必须在调用方法时指定要移除的内容:

```py
`vegetables = {"carrot", "lettuce", "broccoli", "onion"}
vegetables.remove("lettuce")

print(vegetables)  # {'broccoli', 'carrot', 'onion'}` 
```

如果我们试图删除一个不存在的项目，我们会得到一个`KeyError`。如果我们不希望这个`KeyError`被提高，我们可以使用另一个叫做`discard`的方法。

`discard`的工作方式完全相同，但它只会尝试移除存在的项目。如果该项目不在集合中，则该操作不执行任何操作。

`remove`和`discard`都要求我们知道自己想要删除什么，但有时这真的无关紧要。我们只想要其中一件。在这些情况下，我们应该使用`pop`方法。

就像其他收藏一样，`pop`给出了被删除的项目。

```py
`vegetables = {"carrot", "lettuce", "broccoli", "onion"}
random_vegetable = vegetables.pop()  # 'lettuce'

print(vegetables)  # {'broccoli', 'onion', 'carrot'}` 
```

最后，我们有`clear`方法，它将为我们清空集合。

## 集合操作

集合为我们提供了几种有效比较集合的新操作。我们今天要学习的主要运算是并、交、差和对称差。

如果你曾经学习过数学中的集合，这些术语可能对你来说很熟悉。

### 联盟

两个集合的并集本质上是两个集合的组合。结果包括两个集合的所有成员，其中重复的成员只包括一次。

我们可以用`union`方法找到两个集合的并集。

```py
`letters = {"a", "b", "c"}
numbers = {1, 2, 3}

letters_and_numbers = letters.union(numbers)

print(letters_and_numbers)  # {'a', 'c', 1, 2, 3, 'b'}` 
```

`union`与`update`方法非常相似，但是`union`创建一个新的集合，而`update`修改一个现有的集合。

### 交集

当我们找到两个集合的交集时，我们得到一个包含两个集合共有元素的新集合。

例如，如果我们有一个包含可被 2 整除的数的集合，另一个包含可被 3 整除的数的集合，那么这些集合的交集将只包含可被 2 和 3 整除的数。

```py
`mod_2 = {2, 4, 6, 8, 10, 12, 14, 16, 18}
mod_3 = {3, 6, 9, 12, 15, 18}

mod_6 = mod_2.intersection(mod_3)

print(mod_6)  # {18, 12, 6}` 
```

### 差异

当使用`difference`时，我们必须小心一点，因为与我们提到的其他集合操作不同，使用`difference`时，集合的顺序很重要。

例如，假设我们有两个游戏包:

```py
`bundle_1 = {"Resident Evil 3", "Final Fantasy VII", "Cyberpunk 2077"}
bundle_2 = {"Doom Eternal", "Halo Infinite", "Resident Evil 3"}` 
```

如果我们在`bundle_1`上调用`difference`，传入`bundle_2`，我们将得到与在`bundle_2`上调用`difference`，传入`bundle_1`不同的结果:

```py
`print(bundle_1.difference(bundle_2))  # {'Final Fantasy VII', 'Cyberpunk 2077'}
print(bundle_2.difference(bundle_1))  # {'Halo Infinite', 'Doom Eternal'}` 
```

`difference`给我们的是我们调用方法的集合中的每一项，除了在第二个集合中出现的那些。

当我们写这个的时候:

```py
`bundle_1.difference(bundle_2)  # {'Final Fantasy VII', 'Cyberpunk 2077'}` 
```

我们得到`"Final Fantasy VII"`和`"Cyberpunk 2077"`，因为它们出现在`bundle_1`中，而不是`bundle_2`中。`"Resident Evil 3"`不包括在内，因为两套都有。

### 对称差

给我们所有只在其中一个集合中出现的物品。与`difference`不同，集合的顺序无关紧要。

如果我们回到我们的游戏捆绑示例，我们可以看到`bundle_1`和`bundle_2`的对称差异是除了`"Resident Evil 3"`之外的所有游戏，因为`"Resident Evil 3"`是两个集合中唯一的游戏:

```py
`bundle_1 = {"Resident Evil 3", "Final Fantasy VII", "Cyberpunk 2077"}
bundle_2 = {"Doom Eternal", "Halo Infinite", "Resident Evil 3"}

print(bundle_1.symmetric_difference(bundle_2))

# {'Cyberpunk 2077', 'Final Fanstay VII', 'Halo Infinite', 'Doom Eternal'}` 
```

## 与其他集合的集合操作

我们只能在集合上调用集合操作符方法，但实际上我们可以将任何我们喜欢的集合传入该方法。这是因为在执行操作之前，Python 将把我们传入的集合转换成一个集合。

例如，如果我们回到我们的字母和数字的例子，如果`numbers`是一个列表就很好，只要我们调用`letters`集合上的方法:

```py
`letters = {"a", "b", "c"}
numbers = [1, 2, 3]

letters_and_numbers = letters.union(numbers)

print(letters_and_numbers)  # {'a', 'c', 1, 2, 3, 'b'}` 
```

## 检查项目是否在集合中

我们经常想要检查一个值是否在集合中，我们可以使用`in`关键字来执行这种检查。`in`将产生`True`是找到的物品，否则产生`False`:

```py
`numbers = {1, 2, 3, 4, 5}

print(3 in numbers)  # True
print(7 in numbers)  # False` 
```

虽然这种测试对集合非常有效，但是我们可以对任何集合使用`in`关键字。例如，我们可以检查给定的字母是否在字符串中:

```py
`print("j" in "Python")  # False
print("n" in "Python")  # True` 
```

我们还可以使用`in`来检查一个键是否在字典中:

```py
`student = {
    "name": "Eric Cartman",
    "age": 10,
    "school": "South Park Elementary"
}

print("grades" in student)  # False
print("school" in student)  # True` 
```

或其价值观之一:

```py
`student = {
    "name": "Eric Cartman",
    "age": 10,
    "school": "South Park Elementary"
}

print(10 in student.values())  # True` 
```

## 练习

1)创建一个空集，并将其赋给一个变量。

2)使用几个`add`调用或一个`update`调用向您的空集合添加三个项目。

3)创建第二集合，该第二集合包括至少一个与第一集合共有的元素。

4)求两个集合的并、对称差、交。打印每个操作的结果。

5)使用`range`创建一个数字序列，然后要求用户输入一个数字。通知用户他们的号码是否在您指定的范围内。

如果你想要一个额外的挑战，也告诉用户他们的数字是太高还是太低。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-11-exercise-solutions/)

## 额外资源

除了集合操作的方法语法之外，我们还有许多操作符可以用来完成同样的任务。你可以在我们的博客上读到更多关于那个[的内容。](https://blog.teclado.com/python-set-operators/)

在[的另一篇博文](https://blog.teclado.com/python-symmetric-difference/)中，我们也有关于`symmetric_difference`的更详细的信息。

如果你有兴趣学习更多关于集合的知识，你可以在官方文档中找到更多的信息，包括一些额外的方法。