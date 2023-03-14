# 第 9 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-9-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 9 天练习](/30-days-of-python/python-30-day-9-enumerate-zip)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)下面是一些关于 BoJack Horseman 中角色的简单数据。编写一个使用析构的 for 循环，以便可以打印每个元组。

```py
`main_characters = [
    ("BoJack Horseman", "Will Arnett", "Horse"),
    ("Princess Carolyn", "Amy Sedaris", "Cat"),
    ("Diane Nguyen", "Alison Brie", "Human"),
    ("Mr. Peanutbutter", "Paul F. Tompkins", "Dog"),
    ("Todd Chavez", "Aaron Paul", "Human")
]` 
```

对于本练习，预期的输出应该是这样的:

```py
`BoJack Horseman is a horse voiced by Will Arnet.` 
```

让我们从不进行析构开始。在这个阶段，这应该变得相当常规。我们只是迭代一个集合，并通过索引访问值。

```py
`for character in main_characters:
    print(f"{character[0]} is a {character[2].lower()} voiced by {character[1]}.")` 
```

不要忘记为角色的种类改变大小写。在原始数据中，第一个字母是大写的，但是我们希望输出时是小写的。

现在让我们转换我们的循环来使用析构。我们在每个元组中有三个值，所以我们的循环需要三个变量来分配这些值。我将使用`character`、`actor`和`species`。

```py
`for character, actor, species in main_characters:
    print(f"{character} is a {species.lower()} voiced by {actor}.")` 
```

我认为做出这种改变强调了这种析构方法的可读性。

### 2)将以下元组解包为 4 个变量。

```py
`("John Smith", 11743, ("Computer Science", "Mathematics"))` 
```

这里我们在一个元组中有四段数据。学生的姓名、学生证号、主修科目和辅修科目。这些主题存储在主元组中的第二个元组中，所以我们在析构集合时必须考虑到这一点。

记住，要析构嵌套的集合，我们只需要匹配我们要解包的集合的结构。

```py
`name, id_number, (major, minor) = ("John Smith", 11743, ("Computer Science", "Mathematics"))` 
```

如果元组存储在变量中，则工作方式相同:

```py
`student = ("John Smith", 11743, ("Computer Science", "Mathematics"))

name, id_number, (major, minor) = student` 
```

### 3)调查当您尝试压缩两个不同长度的可重复项时会发生什么。

让我们用一对列表来尝试一下。我们使用的 iterable 类型并不重要，所以选择你所看到的。

我要有一个三个字母的列表，和一个两个数字的列表。

```py
`letters = ["a", "b", "c"]
numbers = [1, 2]` 
```

现在让我们看看当我们将这两个列表传递给`zip`时会发生什么。记住`zip`是懒惰的，所以我们必须将结果转换成列表或元组来打印任何有意义的输出。

```py
`letters = ["a", "b", "c"]
numbers = [1, 2]

print(list(zip(letters, numbers)))` 
```

我们在终端中得到的是:

我们可以看到，一旦我们到达最短集合的末尾，`zip`就停止了。如果我们有三个不同长度的集合，同样的事情也会发生。

这是一个需要记住的非常重要的细节，因为如果你不小心的话，它会导致意外的数据丢失。

如果你需要确保所有的值都出现在结果 iterable 中，我们有一些选择，在今天帖子的附加资源部分讨论。