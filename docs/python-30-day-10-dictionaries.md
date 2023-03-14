# 第 10 天:字典|泰克拉多

> 原文：<https://blog.teclado.com/python-30-day-10-dictionaries/>

欢迎来到 Python 系列 [30 天的第 10 天！在本帖中，我们将讨论一种叫做字典的新型收藏。](https://blog.teclado.com/30-days-of-python/)

上一次我们讨论了解包 iterables、`enumerate`函数和非常有用的`zip`函数。如果你不熟悉这些工具和技术，我建议你看看[第 9 天](/30-days-of-python/python-30-day-9-enumerate-zip/)，因为我们将在这里再次使用这些工具。

此外，我们还制作了第 8 天和第 9 天的简短回顾，您可以在此处查看来刷新您的记忆。

## 什么是字典？

字典是 Python 版本的一种叫做*关联数组*的东西，它的工作方式与列表、字符串和元组略有不同。

到目前为止，我们看到的集合都是*序列类型*，我们通过索引访问它们的值。如果我们改变一个序列的顺序，我们可能在修改后通过不同的索引引用一个给定值，因为索引实际上只引用集合中的一个位置。

**序列**是 Python 中的一个技术术语，指满足*序列协议*的集合。对于一个序列，它必须有有序的索引，从`0`开始，以`1`为单位递增。用`len`也可以找到集合的长度。

例如，如果我们有以下列表:

```py
`simpsons = ["Homer", "Marge", "Bart", "Lisa", "Maggie"]` 
```

我们可以通过参考`simpsons`列表的索引`0`来参考`"Homer"`。然而，如果我们按字母顺序对列表进行排序，我们可以使用`sort`方法，我们现在必须使用索引`1`来引用`"Homer"`。

```py
`simpsons = ["Homer", "Marge", "Bart", "Lisa", "Maggie"]
simpsons.sort()

print(simpsons)  # ["Bart", "Homer", "Lisa", "Maggie", "Marge"]` 
```

在关联数组中，也就是在字典中，值与其他一些术语相关联。如果我们改变这些值的顺序，这没有关系，因为每个值仍然会与同一个项相关联。这些相关的术语称为关键字。

一个**键**有点像一个变量，我们可以用它来引用一些值。与变量不同，键本身就是值，因为它们被写成字符串、整数等。

```py
`In a single dictionary, each key must be unique, but different dictionaries can have the same keys as each other.` 
```

一个字典可以有许多键，但是每个键在该字典中必须是唯一的，并且每个键必须有一个值。然而，该值可以是一个集合，包括另一个字典。

字典的一个常见用例非常类似于我们使用元组来表示表中的行。例如，不是用这样的元组来表示电影:

```py
`("Up", "Pete Doctor", 2009)` 
```

我们可以使用带有三个键的字典:`"title"`、`"director"`和`"release_year"`。然后，我们可以通过向字典查询与给定键相关联的值来检索我们想要的值。例如，我们可能想要与键`"title"`相关联的值。在这种情况下，我们会被交给字符串，`"Up"`。

## 定义字典

我们用一对花括号创建了一个字典:

这其实是一个空字典，很像`[]`是一个空列表。

当我们想要创建一个包含内容的字典时，我们必须成对地使用键和值。字典不能有没有值的键，我们也不能有与键无关的值。

假设我们正在为一个学生创建一个字典，我们希望从表示他们姓名的键和值开始:

```py
`student = {"name": "John Smith"}` 
```

首先我们有一个键—在本例中是字符串，`"name"`—后跟一个冒号(`:`)，然后是与该键相关联的值。

如果我们想要添加多个键和值，我们必须用逗号分隔每个键值对。

例如，让我们在该学生的姓名旁边添加一个成绩列表:

```py
`student = {
    "name": "John Smith",
    "grades": [88, 76, 92, 85, 69]
}` 
```

### 风格注释

在上面的例子中，字典被分成了几行，就像我们对元组和列表所做的那样。当字典变得很长时，这很常见，因为这有助于可读性。

不要觉得你需要把所有的东西都塞进这样一行:

```py
`student = {"name": "John Smith", "grades": [88, 76, 92, 85, 69]}` 
```

如果字典可以很容易地放在一行中，确保在每个逗号后都有一个空格，就像我们对其他集合所做的那样。

创建可读字典的另一个重要因素是在冒号后添加一个空格。这有助于区分键和值，使它们一眼就能被发现。

## 我们可以用什么作为字典键名？

关于我们可以在 Python 字典中使用什么作为键，也有一些技术限制。

正如我们已经看到的，字符串是非常好的，但是数字，甚至元组也是如此。然而，我们不能使用列表作为键。为什么不呢？因为名单会变。

字典的一个重要特征是它们的键是唯一的。正如我们将要看到的，这对于检索值非常重要。列表的问题在于它们提供了一个违反这个规则的机会，因为我们可以在将列表设置为键之后更改它。通过添加或删除条目，我们可以修改一对列表，使它们最终完全相同。

这种情况的发生意味着 Python 不允许我们使用列表作为键，这同样适用于我们可以修改的任何其他类型。

有些人可能想知道为什么我们可以使用字符串作为键，因为我们似乎也可以修改字符串。然而，我们实际上从来没有修改过一个字符串:我们总是创建一个新的。

刚才我提到我们可以使用一个元组作为一个键，但是这里也有一些限制。这是因为元组可以包含列表之类的东西，我们可以修改那些内部列表。

例如，我们可以这样做:

```py
`student = (
    "John Smith",
    [88, 76, 92, 85, 69]
)

# Append 77 to the list at index 1
student[1].append(77)

print(student)  # ('John Smith', [88, 76, 92, 85, 69, 77])` 
```

因此，我们只能使用不包含任何可变值的元组作为字典中的键。

## 访问字典中的值

我们可以使用订阅表达式访问字典中的值，就像我们访问列表和元组一样。然而，我们不是使用索引，而是使用它们的键来检索值。

让我们看看我们的学生示例，并尝试从该词典中获取分数列表:

```py
`student = {
    "name": "John Smith",
    "grades": [88, 76, 92, 85, 69]
}

print(student["grades"])  # [88, 76, 92, 85, 69]` 
```

正如您所看到的，与使用类似元组的东西相比，这带来了一些显著的可读性好处。虽然使用索引可能相当不透明，但键描述了它们所引用的数据。至少，如果我们使用了好的键名，他们会这样做！

那么，如果我们引用一个不存在的键会发生什么呢？

```py
`student = {
    "name": "John Smith",
    "grades": [88, 76, 92, 85, 69]
}

print(student["grade"])  # KeyError` 
```

在这里，我试图引用一个名为`"grade"`的键，但真正的键名为`"grades"`。在这种情况下，我们得到的是一个`KeyError`，这与我们试图访问一个未定义的变量时得到的`NameError`非常相似。

```py
`Traceback (most recent call last):
  File "main.py", line 6, in <module>
    print(student["grade"])
KeyError: 'grade'` 
```

如果我们不确定某个键是否存在，并且不希望引发异常，我们可以使用一个名为`get`的方法。如果键存在，`get`的工作方式与使用订阅表达式完全相同，但是如果键不存在，它将返回`None`而不是使程序崩溃。

```py
`student = {
    "name": "John Smith",
    "grades": [88, 76, 92, 85, 69]
}

print(student.get("grade"))  # None` 
```

如果愿意，我们可以通过向`get`传递第二个值来指定一个不同的默认值。例如，如果没有`"grade"`键，我们可以告诉它我们想要一个空列表:

```py
`student = {
    "name": "John Smith",
    "grades": [88, 76, 92, 85, 69]
}

print(student.get("grade", []))  # []` 
```

## 修改字典

有三种方法可以修改现有的字典。我们可以添加新的键值对；我们可以修改现有键的值；或者我们可以删除一个键值对。让我们从添加新的键值对开始。

### 向词典中添加条目

要向字典中添加一个新的键值对，我们需要做的就是给一个尚不存在的键赋值。让我们看一个例子:

```py
`student = {
    "name": "John Smith",
    "grades": [88, 76, 92, 85, 69]
}

student["age"] = 17` 
```

我们在字典中给键`"age"`赋了一个值`17`。`"age"`不存在，所以 Python 为我们创造了它。如果我们打印这本字典，我们会看到这样的内容:

```py
`{
    'name': 'John Smith',
    'grades': [88, 76, 92, 85, 69],
    'age': 17
}` 
```

我们还可以使用现有的字典和`update`方法来扩展字典。让我们看一个新的例子:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}` 
```

这里我们有一本字典，里面有一些关于复仇者联盟 4：终局之战的基本信息。我们只有电影名、导演和上映年份。

让我们想象一下，我们在另一本字典中有一些关于这部电影的次要信息，就像这样:

```py
`meta_info = {
    "runtime": 181,
    "budget": "$356 million",
    "earnings": "$2.798 billion",
    "producer": "Kevin Feige"
}` 
```

我想要的是获取原始的`movie`字典，并用来自`meta_info`的信息对其进行补充，这样我们最终会得到包含所有这些键和值的`movie`。我们是这样完成的:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

meta_info = {
    "runtime": 181,
    "budget": "$356 million",
    "earnings": "$2.798 billion",
    "producer": "Kevin Feige"
}

movie.update(meta_info)` 
```

如果我们打印`movie`，它看起来会像这样:

```py
`{
    'title': 'Avengers: Endgame',
    'directors': ['Anthony Russo', 'Joe Russo'],
    'year': 2019,
    'runtime': 181,
    'budget': '$356 million',
    'earnings': '$2.798 billion',
    'producer': 'Kevin Feige'
}` 
```

如果我们愿意，我们也可以直接向`update`传递一个字典，而不是预先定义`meta_info`:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

movie.update({
    "runtime": 181,
    "budget": "$356 million",
    "earnings": "$2.798 billion",
    "producer": "Kevin Feige"
})` 
```

如果我们使用较短的词典，或者如果我们没有将其他词典用于任何其他目的，这将非常有用。

### 修改字典中的现有条目

修改条目的方法实际上与我们想要添加新条目的方法完全相同。我们可以不使用不存在的键，而是为现有的键赋值，这将替换当前值:

```py
`student = {
    'name': 'John Smith',
    'grades': [88, 76, 92, 85, 69],
    'age': 17
}

student["age"] = 18` 
```

我们还可以使用`update`方法一次替换许多值，或者执行一些加法和替换的组合。任何已经存在的键的值都将被替换，而新的键将像我们之前看到的那样被创建。

### 从字典中删除条目

从字典中删除条目的方法与从列表中删除非常相似。

我们的第一个选项是使用`del`从字典中删除一个特定的键，这将删除键值对:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019,
    "runtime": 181
}

del movie["runtime"]` 
```

这将给我们留下这样一部字典:

```py
`{
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}` 
```

我们也可以使用`pop`，传入一个键。与列表非常相似，这也将返回被删除的值。

如果我们使用这两种方法引用一个不存在的键，我们得到一个`KeyError`。

我们还有`clear`方法，它将完全清空一个字典，就像我们在列表中使用它一样。

## 遍历字典

迭代字典时需要注意的一点是，默认情况下，我们只获取键。例如，让我们试着打印我们的`movie`字典中的每个条目:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

for attribute in movie:
    print(attribute)` 
```

我们在这种情况下得到的是:

我看到当人们想要这些值时，他们通常会这样做:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

for key in movie:
    print(movie[key])` 
```

这是可行的，但我们真的不需要诉诸这样的东西。我们可以调用字典上的`values`方法来获取包含每个键的值的 iterable:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

for value in movie.values():
    print(value)` 
```

但是如果我们想要密钥和值呢？我们必须回到这样的方法吗？

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

for key in movie:
    print(f"{key}: {movie[key]}")` 
```

谢天谢地没有！我们可以使用`items`方法来代替。这将给出一系列元组，其中每个元组的第一项是键，第二项是值。

我们可以解构这些元组来得到我们想要的:

```py
`movie = {
    "title": "Avengers: Endgame",
    "directors": ["Anthony Russo", "Joe Russo"],
    "year": 2019
}

for key, value in movie.items():
    print(f"{key}: {value}")` 
```

## 字典中条目的顺序

在现代 Python 中(从版本 3.7 开始)，字典已经作为语言规范的一部分被排序。这个顺序是基于我们将条目添加到字典中的顺序。如果我们将`"title"`键放在`"artist"`键之前，这将反映在我们创建的字典中。然而，情况并非总是如此。

如果出于某种原因，您发现自己不得不使用较旧的 Python 版本，比如 Python 3.5，您将无法依赖于字典中值的顺序。当我们试图迭代字典时，或者当我们试图将它们的内容解包到变量中时，这将变得相关。

## 练习

1)下面是描述相册的元组:

```py
 `(
    "The Dark Side of the Moon",
    "Pink Floyd",
    1973,
    (
        "Speak to Me",
        "Breathe",
        "On the Run",
        "Time",
        "The Great Gig in the Sky",
        "Money",
        "Us and Them",
        "Any Colour You Like",
        "Brain Damage",
        "Eclipse"
    )
 )` 
```

在这个元组中，我们有专辑名称、艺术家(在这个例子中是乐队)、发行年份，然后是另一个包含曲目列表的元组。

将这个外部元组转换成一个有四个键的字典。

2)迭代您在练习 1 中创建的字典的键和值。对于每个键和值，您应该打印键的名称，然后在旁边打印值。

3)从您创建的词典中删除曲目列表和发行年份。完成后，向字典中添加一个新的键来存储发布日期。《月球背面》的上映日期是 1973 年 3 月 1 日。

如果您使用不同的相册进行练习，请相应地更新日期。

4)尝试检索您从字典中删除的一个值。这应该会给你一个`KeyError`。一旦您尝试了这一点，使用`get`方法重复该步骤以防止引发异常。

## 额外资源

`update`方法可以接受几种不同格式的值，而不仅仅是我们在这篇文章中看到的那种。如果你感兴趣的话，我们有一篇关于`update`的[博客文章](https://blog.teclado.com/python-updating-dictionaries/)，这篇文章会更详细。

此外，我们有一个关于字典和循环的帖子。

如果你想了解更多关于字典的知识，Python 文档也是一个很好的地方。