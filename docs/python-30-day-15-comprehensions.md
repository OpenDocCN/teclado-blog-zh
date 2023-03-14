# 第 15 天:理解

> 原文：<https://blog.teclado.com/python-30-day-15-comprehensions/>

欢迎来到 Python 系列 [30 天的第 15 天！今天我们将学习一种叫做理解的新型句法。](https://blog.teclado.com/30-days-of-python/)

理解允许我们以一种非常简洁的方式从一些其他的可迭代对象中创建一个集合。通常，它们会为我们节省一行又一行的代码，并帮助减少不必要的样板文件。

## 列出理解

有几种不同类型的理解，但最常用的是列表理解。

列表理解用于从其他一些可重复项创建一个新列表。它可能是另一个列表，甚至可能是类似于`zip`对象的东西。

让我们来看一个例子，当列表理解可能是有用的。想象一下我有一个这样的名字列表:

```
`names = ["mary", "Richard", "Noah", "KATE"]` 
```

现在他们不是很一致。名字以不同的大小写书写，如果我们从用户那里获取数据，这很容易发生。

我想确保所有的名字都是大写的，所以我要遍历这个列表，从每个名字创建一个新的大写字符串。

```
`names = ["mary", "Richard", "Noah", "KATE"]
processed_names = []

for name in names:
    processed_names.append(name.title())` 
```

一旦我们完成了，`processed_names`将包含我们所有的标题案例字符串，正如我们所希望的那样。

不过，我不喜欢这种方法的一些地方。

1.  我们必须为第二个列表使用一个新的名称。我们不能简单地丢弃旧的`names`列表并替换它，因为我们必须迭代该列表来处理值。
2.  这些代码中有很多感觉有点多余。我们实际上只是试图创建一个具有不同值的新列表，所以创建一个空列表并追加值感觉像是语言在做一些简单的事情。

这就是理解语法的用武之地。在我们详细讨论语法之前，让我们看一下上面使用列表理解代替传统循环的例子:

```
`names = ["mary", "Richard", "Noah", "KATE"]
processed_names = [name.title() for name in names]` 
```

更好的是，我们可以重用`names`而不是分配给`processed_names`:

```
`names = ["mary", "Richard", "Noah", "KATE"]
names = [name.title() for name in names]` 
```

相当酷！

对于这样的理解，这个结构有两个主要部分:我们想要添加到新列表中的值，以及一个循环定义。

在上面的例子中，我们想要添加的值是`name.title()`，我们已经定义的循环是`for name in names`。

我们可以认为这是在说，“对于`names`中的每个`name`，将`name.title()`放入新列表中”。

就像常规循环一样，我们可以定义更复杂的理解，其中有多个循环变量。

例如，我们可以采用这样的代码:

```
`names = ("mary", "Richard", "Noah", "KATE")
ages = (36, 21, 40, 28)

people = []

for name, age in zip(names, ages):
    person_data = (name.title(), age)
    people.append(person_data)` 
```

我们可以用一种理解来改写它:

```
`names = ("mary", "Richard", "Noah", "KATE")
ages = (36, 21, 40, 28)

people = [(name.title(), age) for name, age in zip(names, ages)]` 
```

在这两种情况下，`people`列表最终包含以下内容:

```
`[('Mary', 36), ('Richard', 21), ('Noah', 40), ('Kate', 28)]` 
```

注意，在这个例子中，我对`names`和`ages`集合使用了元组，因为这些数据保持特定的顺序非常重要。我们不希望有人对这些收藏或类似的东西进行分类，因为我们最终可能会将一个名字与错误的年龄相匹配。

### 风格注释

有时理解会变得相当长，特别是当我们有很长的变量名时。在这种情况下，将理解分成几行会很有帮助，如下所示:

```
`people = [
    (name.title(), age)
    for name, age in zip(names, ages)
]` 
```

一个合理的方法是把我们添加到列表中的值放在一行，循环定义放在下一行。

同样值得注意的是，理解并不总是这项工作的正确工具，它们也不是常规 for 循环的大规模替代品。如果我们要执行一些复杂的逻辑，常规的循环会清晰得多，因为我们可以将这些操作分解成更小的步骤，并使用变量名来标记各个点上发生的事情。

所有这些在理解中都是不可能的，所以它们通常只适用于更简单的操作。

## 集合理解

一种不太为人所知的理解类型是集合理解。这就像列表理解一样，但是它产生一个集合，而不是一个列表。

理解的内部结构和列表理解是一样的，我们只需要把所有东西都用花括号括起来，而不是方括号。

```
`names = ["mary", "Richard", "Noah", "KATE"]
names = {name.title() for name in names}` 
```

现在`names`是包含四个标题案例字符串的集合:

```
`{'Mary', 'Richard', 'Noah', 'Kate'}` 
```

## 词典释义

我们可以利用的最后一种理解是词典理解。像集合理解和常规字典一样，字典理解用花括号括起来。

字典理解与我们已经看到的列表和集合理解有些不同，因为字典需要每个条目的键和值。

我们用于键值对的语法与我们正常定义字典时完全相同。首先是键，后面是冒号(`:`)，然后是值。这整个结构就是我们理解的价值位置。

例如，假设我们有这样一个循环:

```
`student_ids = (112343, 134555, 113826, 124888)
names = ("mary", "Richard", "Noah", "KATE")

students = {}

for student_id, name in zip(student_ids, names):
    student = {student_id: name.title()}
    students.update(student)` 
```

这里我们有两个元组，包含一些学生的相关信息。在第一个元组中，我们有一些学生 id 号，在第二个元组中，我们有相同顺序的学生姓名。

我们将这些集合压缩在一起，这样在我们的`zip`对象中就有这样的东西:

```
`(112343, "mary"), (134555, "Richard"), (113826, "Noah"), (124888, "KATE")` 
```

然后我们使用这些对来创建新的字典，这样我们就可以用新的键值对来更新`students`字典。记住`update`可以接受一个字典作为参数，它会将那个字典的键和值合并到另一个字典中。

结果是这样的字典:

```
`{112343: 'Mary', 134555: 'Richard', 113826: 'Noah', 124888: 'Kate'}` 
```

我们可以用这样的词典理解来生产同一本词典:

```
`student_ids = (112343, 134555, 113826, 124888)
names = ("mary", "Richard", "Noah", "KATE")

students = {
    student_id: name.title()
    for student_id, name in zip(student_ids, names)
}` 
```

同样，我们有想要添加到新集合中的值，对于字典理解来说，这是一个键值对。

随后是一个循环定义，确定原始值的来源:

```
`for student_id, name in zip(student_ids, names)` 
```

所有这些都放在花括号内，表示我们在使用字典理解:

```
`students = {
    student_id: name.title()
    for student_id, name in zip(student_ids, names)
}` 
```

## 理解和范围

你可能会疑惑，为什么我们可以在理解中使用一个变量名，并赋予相同的变量名？当我们访问它们的时候，我们不是在替换它们吗？这难道不危险吗？

你可能也注意到了，我们不能在理解之外引用我们在理解循环中定义的变量。

例如，对于常规循环，我们可以这样做:

```
`names = ["Mary", "Richard", "Noah", "Kate"]
names_lower = []

for name in names:
    names_lower.append(name.lower())

print(name)  # This refers to the name variable we defined in the loop` 
```

`name`只是一个普通变量，一旦循环完成，那个变量依然存在。它的值将会是循环中最后一次被赋值的值。在上面的例子中，`name`指的是字符串，`"Kate"`。

如果我们用另一方面的理解，我们就不能这样做:

```
`names = ["Mary", "Richard", "Noah", "Kate"]
names_lower = [name.lower() for name in names]

print(name)  # NameError` 
```

参考`name`给我们一个`NameError`，因为`name`没有被定义:

```
`Traceback (most recent call last):
  File "main.py", line 4, in <module>
    print(name)  # NameError
NameError: name 'name' is not defined` 
```

这到底是怎么回事？

这些差异的原因是理解实际上是函数。当我们写下这样的话:

```
`names = ["Mary", "Richard", "Noah", "Kate"]
names_lower = [name.lower() for name in names]` 
```

在幕后，Python 正在运行一个与此类似的函数:

```
`def temp():
    new_list = []

    for name in names:
        new_list.append(name.lower())

    return new_list` 
```

这只是一个常规的`for`循环，就像我们之前使用的一样，只是这个`for`循环是在函数内部运行的。

如果你还记得第 13 天，我们谈到了这样一个事实，当我们调用一个函数时，一个新的命名空间被创建来存储我们在函数中定义的变量名。这个名称和值的字典只能在函数中访问。

我们现在可以明白为什么理解中的变量不能在理解之外被访问。它们只在后台函数运行时存在，并且只在该函数内部可用。

类似地，我们可以看到为什么我们可以给一个变量名赋值，而表面上在理解范围内使用它。

在函数内部，我们实际上是在创建一个全新的列表，我们将这个列表赋给一个临时名称。因此，在填充新列表时，我们不会修改原始的 iterable。只有当我们完成时，我们才返回新的列表，并可能替换赋给变量的现有值。

我们通常不必担心 Python 为我们制作和运行的这个函数，但是很高兴知道理解实际上只是其核心的常规`for`循环。

它们内部的代码被封装在这个函数中的事实也非常有用，因为这意味着我们的理解不会像`for`循环那样影响它们周围的代码。毕竟，如果我们使用一个名称作为循环变量，我们可能会覆盖与该名称相关的现有值。这不是理解上的问题。

## 练习

1)将下面的 for 循环转换成一个理解:

```
`numbers = [1, 2, 3, 4, 5]
squares = []

for number in numbers:
    squares.append(number ** 2)` 
```

2)使用字典理解从下面的字典创建新字典，其中每个值都是标题大小写。

```
`movie = {
    "title": "thor: ragnarok",
    "director": "taika waititi",
    "producer": "kevin feige",
    "production_company": "marvel studios"
}` 
```

请记住，默认情况下，遍历字典只会给我们提供键。您可以使用`items`方法来获取键和值。详见第 10 天。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-15-exercise-solutions)

## 额外资源

我们也可以通过在理解中添加条件逻辑来使用理解进行过滤。在我们的博客上，我们有[篇文章详细讨论了这个](https://blog.teclado.com/python-list-comprehensions-conditionals/)。

我们还将在第 20 天讲述这一点。