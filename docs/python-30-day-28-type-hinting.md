# 第 28 天:类型提示| Teclado

> 原文：<https://blog.teclado.com/python-30-day-28-type-hinting/>

欢迎来到 Python 系列 [30 天的第 28 天！今天我们将学习 Python 中使用类型注释的类型提示。](https://blog.teclado.com/30-days-of-python/)

类型注释允许我们告诉 Python 我们期望在应用程序中的给定点赋予名称什么。然后，我们可以使用这些注释来检查程序实际上是在做我们想要做的事情。

## 类型提示的好处

类型提示非常有用，原因有很多。

1.  这使得理解我们的代码变得更容易，因为我们有用的变量名现在伴随着我们期望分配给它们的数据类型的描述。
2.  大多数现代编辑器能够很好地利用我们的类型注释，在我们做像调用函数这样的事情时提供更有意义的提示。
3.  我们可以使用像`mypy`这样的工具来检查我们的类型注释是否被遵守，帮助我们捕捉由于传递不正确的类型而导致的错误。

类型注释不能做的一件事实际上是防止我们违反我们在注释中概述的规则。它们只是一个开发工具，当我们运行应用程序时，对我们的代码没有任何影响。

现在我们已经知道了类型注释能为我们做什么，让我们学习如何实际使用它们。

## 安装`mypy`

如果您使用的是 PyCharm 这样的编辑器，类型提示是内置的，随时可以使用。PyCharm 会在你写代码的时候给你现场提示，所以你不需要做任何额外的事情。

如果您的编辑器不支持这种实时类型提示，或者您希望能够运行一个工具来为您提供关于给定文件的全面信息，我们需要安装一个类似于`mypy`的工具。

我们可以通过运行以下命令来安装`mypy`:

```py
`python -m pip install mypy` 
```

如果你不确定如何使用它，Python 文档中有一个教程[讲述了如何使用命令行安装包。](https://packaging.python.org/tutorials/installing-packages/)

一旦安装了`mypy`,我们就可以通过在`mypy`后面加上我们想要检查的文件名来使用它。例如，如果我们想要检查`app.py`，我们将在控制台中编写以下内容:

这将通知我们在评估该文件时`mypy`能够检测到的任何问题。

## 基本类型提示

让我们从使用类型提示来注释一些我们期望采用基本类型的变量开始。

```py
`name: str = "Phil"
age: int = 29
height_metres: float = 1.87
loves_python: bool = True` 
```

从上面的例子可以看出，我们可以通过在变量名称后添加一个冒号来注释变量，然后指定类型。

这里我已经指出`name`是一个字符串，`age`是一个整数，`height_metres`应该是一个浮点数。在这种情况下，我们所有的类型注释都与已经赋值的值对齐，所以我们没有任何问题。

我们可以通过运行`mypy`来确认这一点。

```py
`> mypy app.py
Success: no issues found in 1 source file` 
```

现在让我们看看当事情不是我们想要的时候会发生什么。

```py
`name: str = 29
age: int = 1.87
height_metres: float = "Phil"` 
```

我们所有的价值观都变得混乱了。也许我们从一个包含顺序错误的值的 iterable 中分配了这些值。

现在，如果我们运行`mypy`，我们会得到一些错误。

```py
`> mypy app.py
app.py:1: error: Incompatible types in assignment (expression has type "int", variable has type "str")
app.py:2: error: Incompatible types in assignment (expression has type "float", variable has type "int")
app.py:3: error: Incompatible types in assignment (expression has type "str", variable has type "float")
Found 3 errors in 1 file (checked 1 source file)` 
```

这些错误实际上非常有用。它们确切地解释出了什么问题，并且在每一行的开始，我们得到一个对发生错误的文件的引用，以及一个行号。

使用这些信息，我们可以很容易地找到问题的根源。

## 增加一些灵活性

假设我们希望在处理`height_metres`时更加灵活一点。我只关心它是一个实数，所以除了接受浮点数，我还想接受整数。

我们完成这个的方法是使用一个叫做`Union`的工具，我们必须从`typing`模块导入这个工具。

```py
`from typing import Union

name: str = "Phil"
age: int = 29
height_metres: Union[int, float] = 1.87` 
```

这里我添加了`Union[int, float]`作为`height_metres`的类型注释，这意味着我们可以接受整数或浮点数。通过在方括号之间添加更多逗号分隔的值，我们可以向这个`Union`添加尽可能多的类型。

我们也可以变得非常灵活，使用另一个叫做`Any`的工具，它可以匹配任何类型。您应该小心使用`Any`，因为它在很大程度上消除了类型提示的好处。这对于向读者表明某些东西是完全通用的是很有用的。

我们可以像使用其他类型一样使用`Any`:

```py
`from typing import Any

name: str = "Phil"
age: int = 29
height_metres: Any = 1.87` 
```

## 注释收藏

既然我们已经看了注释基本类型，让我们谈谈我们如何注释应该是一个列表，或者可能是一个包含特定类型值的元组。

为了注释集合，我们必须从`typing`模块导入特殊类型。对于列表，我们需要导入`List`，对于元组，我们需要导入`Tuple`。

如你所见，这些名字很有意义，但是如果你对需要导入什么有疑问，你可以在这里找到`typing`模块[的文档。](https://docs.python.org/3/library/typing.html)

下面是一个使用`List`注释的变量的例子:

```py
`from typing import List

names: List =  ["Rick", "Morty", "Summer", "Beth", "Jerry"]` 
```

如果我们想指定哪些类型应该在列表中，我们可以添加一组方括号，就像我们对`Union`所做的那样。

```py
`from typing import List

names: List[str] =  ["Rick", "Morty", "Summer", "Beth", "Jerry"]` 
```

去掉括号实际上和写`List[Any]`是一回事。

如果我们愿意，我们可以通过像这样组合`List`和`Union`来允许列表中有多种类型:

```py
`from typing import List, Union

random_values: List[Union[str, int]] =  ["x", 13, "camel", 0]` 
```

当处理元组时，我们可以为序列中的每个项目指定一个类型，因为元组是固定长度的，并且是不可变的。

例如，我们可以这样做:

```py
`from typing import Tuple

movie: Tuple[str, str, int] = ("Toy Story 3", "Lee Unkrich", 2010)` 
```

### 注意

在 Python 3.9 中，我们不必从`typing`模块中导入像`Tuple`、`List`和`Dict`这样的东西。相反，我们将能够使用标准的`tuple`、`list`和`dict`类型进行注释。

你可以在这里找到更多[。](https://docs.python.org/3.9/whatsnew/3.9.html#pep-585-builtin-generic-types)

## 创建类型别名

让我们考虑这样一种情况，我们想在一个列表中存储许多像上面这样的电影。在课程的早期阶段，我们遇到过几次这种情况。我们该如何注释这样的东西呢？

也许是这样的:

```py
`from typing import List, Tuple

movies: List[Tuple[str, str, int]] = [
    ("Finding Nemo", "Andrew Stanton", 2005),
    ("Inside Out", "Pete Docter", 2015),
    ("Toy Story 3", "Lee Unkrich", 2010)
]` 
```

这个*确实可以工作，但是那个类型注释变得很难阅读。这是一个问题，因为使用类型注释的好处之一是*帮助*提高可读性。*

在这种情况下，我们有复杂的类型注释，通常最好为某些类型组合定义新的别名。例如，我认为将这些元组中的每一个称为`Movie`是很有意义的。

我们可以这样做:

```py
`from typing import List, Tuple

Movie = Tuple[str, str, int]

movies: List[Movie] = [
    ("Finding Nemo", "Andrew Stanton", 2005),
    ("Inside Out", "Pete Docter", 2015),
    ("Toy Story 3", "Lee Unkrich", 2010)
]` 
```

现在像`mypy`这样的工具将会认为术语`Movie`的意思是`Tuple[str, str, int]`。如果我们尝试用`mypy`检查我们的代码，我们可以看到一切正常。

```py
`> mypy app.py
Success: no issues found in 1 source file` 
```

现在让我们做一个小小的改变，这样我们就可以保证我们不会得到误报。为了检查，我要把海底总动员的日期改成字符串，`"2005"`。

当我们运行`mypy`时，我们现在可以出错，正如我们所预料的那样。

```py
`> mypy app.py
app.py:11: error: List item 0 has incompatible type "Tuple[str, str, str]"; expected "Tuple[str, str, int]"
Found 1 error in 1 file (checked 1 source file)` 
```

## 注释功能

现在让我们开始注释函数，这是这种工具最有用的地方。现在让我们继续使用我们的电影元组列表，让我们添加一个函数以给定的格式打印每部电影。

```py
`from typing import List, Tuple

Movie = Tuple[str, str, int]

def show_movies(movies):
    for title, director, year in movies:
        print(f"{title} ({year}), by {director}")

movies: List[Movie] = [
    ("Finding Nemo", "Andrew Stanton", 2005),
    ("Inside Out", "Pete Docter", 2015),
    ("Toy Story 3", "Lee Unkrich", 2010)
]

show_movies(movies)` 
```

那么，我们如何注释这个函数呢？

注释参数就像注释任何其他变量一样。在本例中，我们传入了一个电影元组列表，并且我们已经准备好了一个`Movie`注释，所以我们可以这样写:

```py
`from typing import List, Tuple

Movie = Tuple[str, str, int]

def show_movies(movies: List[Movie]):
    for title, director, year in movies:
        print(f"{title} ({year}), by {director}")

movies: List[Movie] = [
    ("Finding Nemo", "Andrew Stanton", 2005),
    ("Inside Out", "Pete Docter", 2015),
    ("Toy Story 3", "Lee Unkrich", 2010)
]

show_movies(movies)` 
```

## 注释返回值

让我们为我们的小应用程序添加另外几个函数。我想添加一个搜索函数来确定电影库中是否存在给定的电影，我还想添加一个函数来处理单部电影的打印。

我的实现看起来会像这样:

```py
`from typing import List, Tuple

Movie = Tuple[str, str, int]

def find_movie(search_term, movies):
    for title, director, year in movies:
        if title == search_term:
            return (title, director, year)

def show_movies(movies: List[Movie]):
    for movie in movies:
        print_movie(movie)

def print_movie(movie):
    title, director, year = movie
    print(f"{title} ({year}), by {director}")

movies: List[Movie] = [
    ("Finding Nemo", "Andrew Stanton", 2005),
    ("Inside Out", "Pete Docter", 2015),
    ("Toy Story 3", "Lee Unkrich", 2010)
]

show_movies(movies)

search_result = find_movie("Finding Nemo", movies)

if search_result:
    print_movie(search_result)
else:
    print("Couldn't find movie.")` 
```

`find_movie`功能比较粗糙。它只返回完全匹配的结果，并且只能找到一个结果，但是对于他的例子来说，这已经足够了。

`print_movie`函数现在处理最初的`show_movies`函数所做的一小部分工作。定义第二个函数允许我们在应用程序的其他部分打印单个的电影，比如当我们从`search_result`得到一部电影时。

让我们从注释所有我们已经知道如何做的事情开始。

```py
`from typing import List, Tuple, Union

Movie = Tuple[str, str, int]

def find_movie(search_term: str, movies: List[Movie]):
    for title, director, year in movies:
        if title == search_term:
            return (title, director, year)

def show_movies(movies: List[Movie]):
    for movie in movies:
        print_movie(movie)

def print_movie(movie: Movie):
    title, director, year = movie
    print(f"{title} ({year}), by {director}")

movies: List[Movie] = [
    ("Finding Nemo", "Andrew Stanton", 2005),
    ("Inside Out", "Pete Docter", 2015),
    ("Toy Story 3", "Lee Unkrich", 2010)
]

show_movies(movies)

search_result: Union[Movie, None] = find_movie("Finding Nemo", movies)

if search_result:
    print_movie(search_result)
else:
    print("Couldn't find movie.")` 
```

用不同的值运行几次`mypy`,确保一切正常。

既然我们已经注释了大部分的应用程序类型，我们还缺少一样东西。我们目前没有告诉 Python 我们的函数应该返回什么。

在定义函数时，我们可以通过在括号后使用`->`来做到这一点。

我们的大多数函数不需要返回注释，因为它们隐式返回。然而，我们的`find_movie`函数可以，它可以返回两个不同的值:一个电影元组，或`None`。在没有找到匹配电影的情况下，它返回`None`。

因此，我们可以这样注释它:

```py
`def find_movie(search_term: str, movies: List[Movie]) -> Union[Movie, None]:
    for title, director, year in movies:
        if title == search_term:
            return (title, director, year)` 
```

但是我们有一个问题。如果我们运行`mypy`，它会抱怨我们遗漏了一个`return`语句。

```py
`> mypy app.py
app.py:5: error: Missing return statement
Found 1 error in 1 file (checked 1 source file)` 
```

这是`mypy`警告我们，我们做了一些不符合 Python 风格指南( [PEP8](https://www.python.org/dev/peps/pep-0008/) )的事情。引用导游的话，

在返回声明中保持一致。要么函数中的所有 return 语句都应该返回表达式，要么都不应该返回表达式。如果任何 return 语句返回一个表达式，任何没有返回值的 return 语句都应该显式地声明这是 return None，并且一个显式的 return 语句应该出现在函数的末尾(如果可以到达的话)

这意味着我们应该这样写函数:

```py
`def find_movie(search_term: str, movies: List[Movie]) -> Union[Movie, None]:
    for title, director, year in movies:
        if title == search_term:
            return (title, director, year)

    return None` 
```

现在一切都顺利通过了:

```py
`> mypy app.py
Success: no issues found in 1 source file` 
```

## 使用`Optional`

在有类似于`Union[Movie, None]`的情况下，其中一个`Union`中的类型是`None`，我们有另一个可以使用的工具叫做`Optional`。如果我们写类似`Optional[Movie]`的东西，这和写`Union[Movie, None]`是一回事。

```py
`def find_movie(search_term: str, movies: List[Movie]) -> Optional[Movie]:
    for title, director, year in movies:
        if title == search_term:
            return (title, director, year)

    return None` 
```

## 练习

今天只有一个练习，因为是个大练习。将你的最终解决方案带到第 14 天的项目中(简单版或硬版)，并为你的代码实现类型提示。

如果你不确定该怎么做，记住你可以随时查看`typing`模块[文档](https://docs.python.org/3/library/typing.html)。

您可以在这里找到我们的解决方案[。](/30-days-of-python/python-30-day-28-exercise-solutions)

## 项目

今天是另一个项目日，所以一旦你完成了今天的练习，一定要看看今天的项目！

今天我们要写一个程序，从互联网上的网页中自动收集信息。