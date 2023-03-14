# 第 9 天:解包、枚举和 zip 函数| Teclado

> 原文：<https://blog.teclado.com/python-30-day-9-enumerate-zip/>

欢迎来到 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列的第 9 天！今天我们来看看一个非常有用的迭代技术，叫做解包。我们还将探索几个新的函数:`enumerate`和`zip`。

如果你错过了昨天的帖子，我们讨论了一个非常重要的结构，叫做`while`循环，所以如果你不熟悉的话，我建议你看一下。

## 拆包

解包通常用于一次执行几个赋值，从一些 iterable 中提取单个值。这个过程也称为析构。

我们来看一个非常简单的案例。可能我有一个电影记录，是一个元组，里面有电影片名，导演名字，上映年份。我想做的是将这个元组的值赋给三个变量:`title`、`director`和`year`。

以我们目前的知识，我们必须做这样的事情:

```py
`movie = ("12 Angry Men", "Sidney Lumet", 1957)

title = movie[0]
director = movie[1]
year = movie[2]` 
```

不过这有点乏味，谢天谢地还有更好的方法。不用写单独的赋值和访问索引，我们可以简单地这样写:

```py
`movie = ("12 Angry Men", "Sidney Lumet", 1957)

title, director, year = movie` 
```

这里发生的事情是 Python 看到我们有三个名字要分配，我们分配给它的元组有三个元素。因此，它从元组中抓取一个项目，一次一个，并按顺序将这些值分配给名称。

名称的数量必须与 iterable 中元素的数量相匹配，否则我们将会得到如下异常:

```py
`Traceback (most recent call last):
  File "main.py", line 3, in <module>
    title, director, year = movie
ValueError: not enough values to unpack (expected 3, got 2)` 
```

这里我们不仅限于元组:我们可以使用列表或任何其他可迭代类型。这是非常有用的，我们可以在循环中使用这种技术达到很好的效果，正如我们将要看到的！

## 在 for 循环中解包

让我们回到我们的电影库示例，其中我们有一个包含几个电影元组的列表:

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]` 
```

当我们第一次看 for 循环时，我们有一个例子，我们以一种很好的方式为用户打印这些元组的值，这样他们就可以看到他们所有的电影。

该循环如下所示:

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]

for movie in movies:
    print(f"{movie[0]} ({movie[2]}), by {movie[1]}")` 
```

这个循环工作完全正常，但是有一点不好的是，这些值真的不透明。什么是`movie[0]`？我们真的得回去看看电影列表，把这个弄清楚。如果我们能在这里使用好听的、描述性的名字，那就更好了。

我们来想想迭代`movies`的时候是怎么回事。对于每次迭代，`movies`将会给我们一个元组，我们将会把这个值赋给`movie`。

如果我们仔细想想，这和这样做很相似:

```py
`movie = ("Memento", "Christopher Nolan", 2000)` 
```

我们有一个名字，`movie`，我们给它分配一个元组。在这种情况下，我们有来自`movies`列表的第二个元组。

然而，我们刚刚看到了一个这样的元组被解包成三个变量的例子，如下所示:

```py
`title, director, year = ("Memento", "Christopher Nolan", 2000)` 
```

事实证明，我们可以在 for 循环中做完全相同的事情:

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]

for title, director, year in movies:
    print(f"{title} ({year}), by {director}")` 
```

对于循环的每次迭代，`movies`给我们一个元组。Python 看到元组中有三个值，我们定义了三个名称。因此，它获取元组中的第一项，并将其分配给名称`title`；它获取第二个项目并将其分配给`director`；它获取第三个值并将其分配给`year`。

通过在这里使用解包，我们使得我们的`print`调用更具可读性，因为我们有清晰的描述性名称，而不是引用索引。

如果你想了解更多关于这项技术的知识，我们有一个[博客帖子，里面有更详细的内容](https://blog.teclado.com/destructuring-in-python/)。

## 列举

解包的一个常见应用是使用名为`enumerate`的函数。

在我们看函数本身之前，我想给我们的打印循环添加一个功能。和电影信息一样，我希望每一行都以一个数字开始，它代表电影的顺序。

换句话说，我想要这样的输出:

```py
`1. Eternal Sunshine of the Spotless Mind (2004), by Michel Gondry
2. Memento (2000), by Christopher Nolan
3. Requiem for a Dream (2000), by Darren Aronofsky` 
```

我们该怎么做呢？

通过某种方式，我们需要生成某种计数器。下面是我经常看到的解决这个问题的一个非常常见的方法:

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]

for index in range(len(movies)):
    print(f"{index  +  1}. {movies[index][0]} ({movies[index][2]}), by {movies[index][1]}")` 
```

非常明确地说，虽然这种解决方案很常见，但它不是一个好的解决方案。这显然很难读懂，我们已经失去了所有伟大的名字。

为了完整起见，让我们来谈谈这是如何工作的，因为我保证你会在某个时候看到这样的解决方案。

首先，我们调用这个名为`len`的函数来查找`movies`列表中有多少部电影。在我们的例子中，有`3`电影，所以`len(movies)`将评估到`3`。

这个值`3`然后被传递到`range`中。正如我们在第 6 天看到的，如果我们向`range`传递一个值，它会给出从`0`到(但不包括)我们传入的数字的一系列值。我们的`range`因此看起来是这样的:

`range`是一个可迭代的，它给我们的第一个值是`0`，我们将其命名为`index`。然后我们使用这个值在`movies`中查找电影元组。

记住`movies[0]`会给我们列表中的第一部电影，`movies[1]`给我们第二个值，以此类推。

当我们遍历`range`值时，数字会增加，最终我们会依次查看每部电影。对于每个电影元组，我们使用另一个订阅表达式(方括号语法)访问我们想要的数据，就像我们以前做的那样。

`index`也是我们的柜台。对于第一次迭代，它是`0`，所以我们只需要给它加上`1`就可以得到印在我们第一部电影旁边的数字`1`。然后它每次增加`1`，在打印输出开始时给我们一个递增的计数器。

好吧，这不是一个很好的解决方案，但是有什么更好的方法呢？好吧，我们可以在循环之外定义一个计数器，然后我们可以在每次迭代中递增计数器:

```py
`movies = [ ... ]
counter = 1

for title, director, year in movies:
    print(f"{counter}. {title} ({year}), by {director}")
    counter += 1` 
```

我们以前没有见过这种`+=`语法，但是`counter += 1`是这样写的简写:

这是一个更好的解决方案，但我们现在有这本书保持不变，我真的不喜欢这样。在我看来，更好的解决方案是使用`enumerate`。

`enumerate`是一个内置函数，它在 iterable 中的值旁边生成一个计数器。我们最终得到的是一系列元组，其中每个元组中的第一项是计数器，第二个值是 iterable 中的原始项。

我们先来看一个简单的例子。我只想创建一个名字列表旁边的计数器。我们可以这样做:

```py
`names = ["Harry", "Rachel", "Brian"]

for counter, name in enumerate(names):
    print(f"{counter}. {name}")` 
```

在这种情况下,`enumerate`包含的是这样的一系列元组:

```py
`(0, "Harry"), (1, "Rachel"), (2, "Brian")` 
```

我们的输出看起来像这样:

```py
`0. Harry
1. Rachel
2. Brian` 
```

这并不是我想要的，因为我想要值从`1`开始。这很容易解决，因为我们可以在调用`enumerate`时为计数器设置一个初始值:

```py
`names = ["Harry", "Rachel", "Brian"]

for counter, name in enumerate(names, start=1):
    print(f"{counter}. {name}")` 
```

现在我们得到了我们想要的输出:

```py
`1. Harry
2. Rachel
3. Brian` 
```

让我们回到最初的电影例子。这里的事情不太容易，因为我们的每个项目本身就是一个元组。我们可以这样做:

```py
`movies = [
    (
        "Eternal Sunshine of the Spotless Mind",
        "Michel Gondry",
        2004
    ),
    (
        "Memento",
        "Christopher Nolan",
        2000
    ),
    (
        "Requiem for a Dream",
        "Darren Aronofsky",
        2000
    )
]

for counter, movie in enumerate(movies, start=1):
    print(f"{counter}. {movie[0]} ({movie[2]}), by {movie[1]}")` 
```

这里`enumerate`的结果是这样的:

```py
`(1, ("Eternal Sunshine of the Spotless Mind", "Michel Gondry", 2004)),
(2, ("Memento", "Christopher Nolan", 2000)),
(3, ("Requiem for a Dream", "Darren Aronofsky", 2000))` 
```

所以`counter`取第一个位置的每个数字的值(`1`、`2`和`3`)。而`movie`在第二个位置取每个元组的值(比如`("Memento", "Christopher Nolan", 2000)`)。

然而，我仍然希望能够将这个电影元组解包到我们之前拥有的这些描述性变量名称中。但是我们如何解开像这样的东西:

```py
`(2, ("Memento", "Christopher Nolan", 2000))` 
```

我们所要做的就是匹配我们要打开的东西的结构。这里我们有一个两个元素的元组，其中第二项是一个三个元素的元组。所以我们可以像这样解开上面的结构:

```py
`counter, (title, director, year) = 2, ("Memento", "Christopher Nolan", 2000)` 
```

在我们的循环中，我们只需要做同样的事情:

```py
`movies = [ ... ]

for counter, (title, director, year) in enumerate(movies, start=1):
    print(f"{counter}. {title} ({year}), by {director}")` 
```

注意这里的括号**是**真的很重要。如果我们写这个，

```py
`movies = [ ... ]

for counter, title, director, year in enumerate(movies, start=1):
    print(f"{counter}. {title} ({year}), by {director}")` 
```

我们将得到一个`ValueError`，因为我们提供了四个名称，但只有两个值:计数器和电影元组。通过使用这些括号，我们指定我们希望将第二个值(元组)分解成三个独立的名称。

## `zip`功能

是一个非常强大和通用的函数，用于将两个或更多的可重复项合并成一个可重复项。

例如，假设我们有两个这样的列表:

```py
`pet_owners = ["Paul", "Andrea", "Marta"]
pets = ["Fluffy", "Bubbles", "Captain Catsworth"]` 
```

`zip`将允许我们把它变成一个新的 iterable，它包含以下内容:

```py
`("Paul", "Fluffy"), ("Andrea", "Bubbles"), ("Marta", "Captain Catsworth")` 
```

本质上，它从每个 iterable 中取出第一项，并把它们放在一个 tuple 中。然后从每个 iterable 中取出第二个项目，依此类推，直到其中一个 iterable 的值用完。我们稍后会回到这一点，因为它真的很重要。

要使用`zip`，我们所要做的就是调用函数并传入我们想要压缩在一起的 iterables。

```py
`pet_owners = ["Paul", "Andrea", "Marta"]
pets = ["Fluffy", "Bubbles", "Captain Catsworth"]

pets_and_owners = zip(pet_owners, pets)` 
```

如果我们想把三个甚至更多的 iterables 压缩在一起，我们可以在调用它的时候把越来越多的 iterables 传递给`zip`。

与`range`非常相似，`zip`是懒惰的，这意味着它只在我们请求它的时候计算下一个值。因此我们不能直接打印它，但是如果我们想看到输出，我们可以把它转换成类似列表的东西:

```py
`print(list(pets_and_owners))

# [('Paul', 'Fluffy'), ('Andrea', 'Bubbles'), ('Marta', 'Captain Catsworth')]` 
```

## 在循环中使用`zip`

另一种使用`zip`的常见方式是在 for 循环中一次迭代两个或更多的 iterables。

让我们回到宠物主人的例子，但是现在我想打印一些描述谁拥有哪只宠物的输出。

我们可以使用`zip`和一点析构来以一种非常清晰的方式做到这一点，因为我们可以在循环中使用清晰的变量名:

```py
`pet_owners = ["Paul", "Andrea", "Marta"]
pets = ["Fluffy", "Bubbles", "Captain Catsworth"]

for owner, pet in zip(pet_owners, pets):
    print(f"{owner} owns {pet}.")` 
```

使用`zip`的常见替代方式是我们前面看到的使用`enumerate`的讨厌的`range` + `len`模式。我建议不惜一切代价避免这种情况！

## 使用`enumerate`和`zip`时的注意事项

当涉及到`enumerate`和`zip`时，你应该知道的一件事是，当我们对它们进行迭代时，它们被消耗掉了。当我们在循环中直接使用它们时，这通常不是问题，但当新开发人员将`zip`或`enumerate`对象赋给变量时，这有时会出错。

这里有一个例子，我们将调用`zip`的结果赋给一个变量:

```py
`movie_titles = [
    "Forrest Gump",
    "Howl's Moving Castle",
    "No Country for Old Men"
]

movie_directors = [
    "Robert Zemeckis",
    "Hayao Miyazaki",
    "Joel and Ethan Coen"
]

movies = zip(movie_titles, movie_directors)` 
```

我们可以毫无问题地迭代电影:

```py
`for title, director in movies:
    print(f"{title} by {director}.")` 
```

然而，如果我们现在再次尝试使用`movies`，我们会发现它是空的。尝试运行下面的代码来查看这一点:

```py
`movie_titles = [
    "Forrest Gump",
    "Howl's Moving Castle",
    "No Country for Old Men"
]

movie_directors = [
    "Robert Zemeckis",
    "Hayao Miyazaki",
    "Joel and Ethan Coen"
]

movies = zip(movie_titles, movie_directors)

for title, director in movies:
    print(f"{title} by {director}.")

movies_list = list(movies)

print(f"There are {len(movies_list)} movies in the collection.")
print(f"These are our movies: {movies_list}.")` 
```

如果您尝试在初始循环后迭代`movies`，您还会发现它不包含任何值。

发生这种情况的原因是因为`zip`和`enumerate`产生了一个叫做*迭代器*的东西。在本系列中，我们不会深入讨论迭代器，因为迭代器是一个高级主题，但是迭代器的一个关键特性是，当我们请求它们的值时，它们就被消耗掉了。这实际上是一个非常有用的特性，但是如果你不熟悉这种行为，这也是一个常见的错误来源。

绕过这个限制的一个简单方法是将迭代器转换成非迭代器集合，比如列表或元组。

```py
`movie_titles = [
    "Forrest Gump",
    "Howl's Moving Castle",
    "No Country for Old Men"
]

movie_directors = [
    "Robert Zemeckis",
    "Hayao Miyazaki",
    "Joel and Ethan Coen"
]

movies = list(zip(movie_titles, movie_directors))` 
```

现在我们可以任意多次访问`movies`中的值。

## 练习

1)以下是一些关于 BoJack Horseman 中角色的简单数据:

```py
`main_characters = [
    ("BoJack Horseman", "Will Arnett", "Horse"),
    ("Princess Carolyn", "Amy Sedaris", "Cat"),
    ("Diane Nguyen", "Alison Brie", "Human"),
    ("Mr. Peanutbutter", "Paul F. Tompkins", "Dog"),
    ("Todd Chavez", "Aaron Paul", "Human")
]` 
```

这些数据包含角色的名字、配音演员和角色的种类。

编写一个使用析构的 for 循环，以便可以按以下格式打印每个元组:

```py
`BoJack Horseman is a horse voiced by Will Arnet.` 
```

请注意，在打印时，您必须将物种信息改为小写。如果您需要一个关于如何做到这一点的提示，我们在第一周的第 3 天已经介绍过了。

2)将以下元组解包为 4 个变量:

```py
`("John Smith", 11743, ("Computer Science", "Mathematics"))` 
```

这些数据依次表示学生的姓名、学号以及主修和辅修科目。

3)调查当您尝试压缩两个不同长度的可重复项时会发生什么。例如，尝试压缩一个包含三项的列表和一个包含四项的元组。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-9-exercise-solutions)

## 项目

一旦你完成了练习，我们今天会为你安排另一个项目。这次我们要写一个简短的程序来验证信用卡！

## 额外资源

如果你想了解更多关于拆包的知识，我们有一篇[博客文章，其中有更详细的内容](https://blog.teclado.com/destructuring-in-python/)。

我们还有一篇关于`enumerate`的博文，你可以在这里找到[，还有一篇关于`zip`的博文，你可以在这里](https://blog.teclado.com/python-enumerate/)看到[。](https://blog.teclado.com/python-zip/)

除了所有这些，我们有一个帖子谈论另一个叫做`zip_longest`的函数。这允许我们在不丢失数据的情况下压缩不同大小的集合，但是它需要一些我们还没有研究过的技术，比如从其他文件导入代码。如果你感兴趣，你可以在这里找到那个帖子。