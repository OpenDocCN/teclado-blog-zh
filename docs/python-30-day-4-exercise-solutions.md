# 第 4 天练习解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-4-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 4 天练习](/30-days-of-python/python-30-day-4-lists-tuples)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)创建一个包含单个元组的`movies`列表。元组应该包含电影标题、导演姓名、电影上映年份和电影预算。

让我们首先创建一个空列表，并将其分配给名称`movies`。我们可以这样使用一对空方括号:

在这个`movies`列表中，我们需要添加一个元组。因为我们在一个列表中，所以我们需要记住在这里使用括号来将我们的元组标记为单个值。

我要用这个房间作为我的第一部电影。你选择什么电影并不重要。我们只是需要一些数据来处理。

```
`movies = [
    ("The Room", "Tommy Wiseau", "2003", "$6,000,000")
]` 
```

### 2)使用`input`功能收集另一部电影的信息。你需要一个标题，导演的名字，发行年份和预算。

为了简单起见，我们将使用四个单独的`input`调用来获取电影信息。我们已经这样做了几次，所以我不认为我们需要在如何做这件事上犹豫不决。

```
`movies = [
    ("The Room", "Tommy Wiseau", "2003", "$6,000,000")
]

title = input("Title: ")
director = input("Director: ")
year = input("Year of release: ")
budget = input("Budget: ")` 
```

### 3)使用`input`从您收集的值创建一个新的元组。确保它们与您在`movies`列表中编写的元组顺序相同。

创建一个元组需要一系列逗号分隔的值。如果您愿意，可以将这些值用括号括起来，以表明这是一个元组:

```
`movies = [
    ("The Room", "Tommy Wiseau", "2003", "$6,000,000")
]

title = input("Title: ")
director = input("Director: ")
year = input("Year of release: ")
budget = input("Budget: ")

new_movie = title, director, year, budget` 
```

我已经将新的元组赋给了一个名为`new_movie`的变量。确保是描述性的。

### 4)通过访问您的新电影元组，使用 f 字符串打印电影名称和发行年份。

当使用 f 字符串时，我们可以直接在另一个字符串中计算表达式，只要我们把它们放在花括号中。这包括引用变量，或者使用订阅表达式来访问集合中的元素。

```
`title = input("Title: ")
director = input("Director: ")
year = input("Year of release: ")
budget = input("Budget: ")

new_movie = title, director, year, budget

print(f"{new_movie[0]} ({new_movie[2]})")` 
```

在这里，我选择以下列格式打印我的电影数据:

如果你想尝试不同的东西，那完全没问题。

不要忘记序列的索引从`0`开始。如果我们想要第一个项目，我们需要访问索引`0`处的项目，而不是索引`1`。

因为我们想要第一个和第三个项目，所以我们需要索引为`0`和`2`的项目。

这可能会让你一时出错，但很快它就会成为你的第二天性。

### 5)使用`append`将新的电影元组添加到`movies`集合。

因为我们已经创建了引用元组的`new_movie`变量，所以这一步相当简单。我们只需要使用点语法调用`append`，在调用方法时需要传入`new_movie`:

```
`movies = [
    ("The Room", "Tommy Wiseau", "2003", "$6,000,000")
]

title = input("Title: ")
director = input("Director: ")
year = input("Year of release: ")
budget = input("Budget: ")

new_movie = title, director, year, budget

print(f"{new_movie[0]} ({new_movie[2]})")

movies.append(new_movie)` 
```

如果某个东西被称为方法，在 Python 中你总是会以同样的方式调用它。你要调用方法的地方，然后是一个点，然后是你要调用的方法，后面是通常的括号。

方法是某种类型特有的功能。例如，列表有一个`append`方法，而字符串和元组没有。`print`另一方面是一个函数，它与给定的类型无关。

### 6)打印`movies`收藏中的两部电影。

这里我们只需要再次使用订阅表达式来访问`movies`中的项目。不要忘记第一个项目是在索引`0`！

```
`movies = [
    ("The Room", "Tommy Wiseau", "2003", "$6,000,000")
]

title = input("Title: ")
director = input("Director: ")
year = input("Year of release: ")
budget = input("Budget: ")

new_movie = title, director, year, budget

print(f"{new_movie[0]} ({new_movie[2]})")

movies.append(new_movie)

print(movies[0])
print(movies[1])` 
```

### 7)从`movies`中删除第一部电影。用你喜欢的任何方法。

我们有很多选择。

首先我们可以这样使用`del`:

或者，我们可以使用`pop`:

`pop`是一个方法，所以我们需要使用这个点语法。

或者，我们可以使用`remove`方法，在索引`0`处传入项目:

我认为`del`是这种情况下最干净的选择，所以我最终的解决方案如下所示:

```
`movies = [
    ("The Room", "Tommy Wiseau", "2003", "$6,000,000")
]

title = input("Title: ")
director = input("Director: ")
year = input("Year of release: ")
budget = input("Budget: ")

new_movie = title, director, year, budget

print(f"{new_movie[0]} ({new_movie[2]})")

movies.append(new_movie)

print(movies[0])
print(movies[1])

del movies[0]` 
```

希望你能够完成所有这些步骤。如果你对某件事的工作原理感到困惑，请记住，你可以随时通过我们的 [Discord 服务器](https://discord.gg/BBWwyMq)与我们联系。