# 第 7 天项目:电影预算| Teclado

> 原文：<https://blog.teclado.com/python-30-day-7-project/>

欢迎来到 Python 系列 [30 天的第一个周末项目！对于这个项目，我们将编写一个程序来分析一些电影数据。](https://blog.teclado.com/30-days-of-python/)

特别是，我们将在我们的数据集中找到电影的平均预算，我们将确定超出我们计算的平均预算的高预算电影。

## 案情摘要

下面你会发现一个列表，其中包含了一些电影的相关数据。列表中的每一项都是一个元组，按以下顺序包含电影名称和电影预算:

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]` 
```

对于这个项目，你的程序应该做到以下几点:

1.  计算数据集中所有电影的平均预算。
2.  打印出每一部预算高于你计算的平均值的电影。你还应该打印出比电影平均预算高出多少。
3.  打印出有多少部电影的花费超过了你计算的平均值。

如果你想要一点额外的挑战，允许用户在运行计算之前向数据集添加更多的电影。

你可以通过询问用户想要添加多少部电影来做到这一点，这将允许你使用一个`for`循环和`range`来重复一些代码给定的次数。在`for`循环中，您可以编写一些代码来接收一些用户输入，并将一个包含所收集数据的电影元组添加到电影列表中。

## 我们的解决方案

下面是我们的解决方案演练，包括这个项目中的额外挑战。

我们也有一个[视频演练](https://youtu.be/uRhlo64P2H0)，它涵盖了这个项目的主要部分。

在我们做其他事情之前，我们真的需要知道数据集的平均预算，所以这应该是我们的第一步。

为了计算平均预算，我们需要两样东西:

*   所有电影的总预算支出。
*   电影的数量。

为了计算所有预算的总和，我们可以从创建一个名为`total_budget`的变量并设置初始值`0`开始。

然后使用一个`for`循环，我们可以遍历`movies`列表中的每部电影。我们将把每部电影的预算加到我们的总数中，就像这样:

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]

total_budget = 0

for movie in movies:
    total_budget = total_budget + movie[1]` 
```

现在我们已经知道了，我们可以通过将`total_budget`除以电影数量来计算平均值。

我们将使用`len()`函数来获取电影的数量。

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]

total_budget = 0

for movie in movies:
    total_budget = total_budget + movie[1]

average_budget = total_budget / len(movies)` 
```

太好了！我们有平均预算。

然而，因为我们在这里处理的是数百万美元，所以让这个值保持为 float 就没有什么意义了。我认为我们应该把它改成一个整数，这样我们就不会在输出中得到随机的十进制值。

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]

total_budget = 0

for movie in movies:
    total_budget = total_budget + movie[1]

average_budget = int(total_budget / len(movies))` 
```

接下来，我们需要再次遍历电影，检查每部电影是否超出平均预算。如果一部电影的预算大于我们刚刚计算的平均预算，我们将打印它。否则我们就继续看下一部电影。

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]

total_budget = 0

for movie in movies:
    total_budget = total_budget + movie[1]

average_budget = int(total_budget / len(movies))

for movie in movies:
    if movie[1] > average_budget:
        over_average_cost = movie[1] - average_budget
        print(f"{movie[0]} cost ${movie[1]}: ${over_average_cost} over average.")` 
```

对于每部电影，我们还会计算它们超出平均预算多少，并将其作为一个格式化字符串的一部分打印出来。这里我们使用了 f 字符串，但是如果你愿意，你也可以使用`format`方法。

最后，我们需要跟踪有多少电影超出了平均预算，这样我们就可以在最后告诉用户这些信息。

有两种方法可以做到这一点:

1.  我们可以创建一个新的变量来记录超出预算的电影数量。您可以在`if`语句中将`1`添加到这个变量中。
2.  我们可以创建一个列表，把超出平均预算的电影放在那里。最后，我们可以使用`len`来计算列表的长度，这将告诉我们超出平均预算的电影数量。

在这种情况下，我将选择第二个选项:

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]

high_budget_movies = []
total_budget = 0

for movie in movies:
    total_budget = total_budget + movie[1]

average_budget = int(total_budget / len(movies))

for movie in movies:
    if movie[1] > average_budget:
        high_budget_movies.append(movie)
        over_average_cost = movie[1] - average_budget
        print(f"{movie[0]} cost ${movie[1]}: ${over_average_cost} over average.")

print(f"There were {len(high_budget_movies)} movies with over average budgets.")` 
```

这看起来很棒，但是如果我们想变得更漂亮，我们可以在长长的预算数字中添加逗号分隔符，使它们更容易阅读。为了做到这一点，我们需要改变这一行:

```
`print(f"{movie[0]} cost ${movie[1]}: ${over_average_cost} over average.")` 
```

对此:

```
`print(f"{movie[0]} cost ${movie[1]:,}: ${over_average_cost:,} over average.")` 
```

这个`:,`是 Python 内置的特殊格式化语言的一部分，它允许我们以各种方式格式化字符串。这不是我们将在本系列中深入讨论的内容，但是你可以在官方文档中找到相关信息[。](https://docs.python.org/3/library/string.html#format-specification-mini-language)

## 额外的

对于这项作业的额外部分，我们将:

1.  询问用户他们想在列表中添加多少部电影。
2.  使用`range`和一个`for`循环执行某个选项指定的次数。
3.  在循环的每次迭代中向用户询问电影名称和预算，并在包含这些信息的`movies`列表中添加一个元组。

我们将把这段代码直接添加到我们的`movies`变量的定义下面。

```
`movies = [
    ("Eternal Sunshine of the Spotless Mind", 20000000),
    ("Memento", 9000000),
    ("Requiem for a Dream", 4500000),
    ("Pirates of the Caribbean: On Stranger Tides", 379000000),
    ("Avengers: Age of Ultron", 365000000),
    ("Avengers: Endgame", 356000000),
    ("Incredibles 2", 200000000)
]

new_movie_count = int(input("Enter how many new movies you wish to add: "))

for _ in range(new_movie_count):
    name = input("Enter new movie name: ")
    budget = int(input("Enter new movie budget: "))
    new_movie = (name, budget)
    movies.append(new_movie)

high_budget_movies = []
total_budget = 0

for movie in movies:
    total_budget = total_budget + movie[1]

average_budget = int(total_budget / len(movies))

for movie in movies:
    if movie[1] > average_budget:
        high_budget_movies.append(movie)
        over_average_cost = movie[1] - average_budget
        print(f"{movie[0]} cost ${movie[1]}: ${over_average_cost} over average.")

print(f"There were {len(high_budget_movies)} movies with over average budgets.")` 
```

就是这样！

这个项目比我们之前尝试过的要长一点，所以到目前为止做得很好。我希望完成这个项目有助于巩固你第一周的知识。现在你已经为第 2 周做好了准备！