# 第 26 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-26-exercise-solutions/>

以下是我们对 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 26 天](/30-days-of-python/python-30-day-26-standard-library)练习的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)使用接受标题、导演、发行年份和预算的`namedtuple`定义一个`Movie`元组。提示用户为每个字段提供信息，并创建您定义的`Movie`元组的一个实例。

首先，我们需要记住从 collections 模块导入`namedtuple`。

现在，我们将能够创建我们的模板。在这种情况下，已经为我们提供了新元组类型的名称和字段，所以我们只需要调用`namedtuple`来获得这些信息。

```
`import namedtuple

Movie = namedtuple("Movie", ["title", "director", "year", "budget"])` 
```

最后，我们只需要从用户那里获得一些信息来创建新的`Movie`元组的实例。

```
`import namedtuple

Movie = namedtuple("Movie", ["title", "director", "year", "budget"])

title = input("What is the title of the movie? ")
director = input(f"Who directed {title}? ")
year = input(f"When was {title} released? ")
budget = input(f"What was the budget for {title}? ")

movie = Movie(title, director, year, budget)` 
```

### 2)使用`defaultdict`存储给定字符串中出现的每个字符的计数。打印本词典中最常见的字符。

这个练习与帖子中的盘点练习非常相似。

首先我们需要从`collections`模块导入`defaultdict`类型。然后我们需要创建一个`defaultdict`的实例，其中工厂函数是`int`。

此时，我还将创建一个包含单词拟声词的字符串作为测试字符串。

```
`from collections import defaultdict

s = "onomatopoeia"
letter_count = defaultdict(int)` 
```

接下来让我们迭代`s`并开始计算字符数，使用我们之前用来增加物品数量的相同方法。

```
`from collections import defaultdict

s = "onomatopoeia"
letter_count = defaultdict(int)

for character in s:
    letter_count[character] += 1` 
```

现在我们只需要通过调用`max`并传入一些排序键来找到这个新字典中最常见的字母。在这种情况下，我将使用 lambda 表达式返回每个键的值。

```
`from collections import defaultdict

s = "onomatopoeia"
letter_count = defaultdict(int)

for character in s:
    letter_count[character] += 1

most_common_character = max(letter_count, key=lambda key: letter_count[key])

print(most_common_character)  # o` 
```

我们可以看到，我们得到了正确的答案:`"o"`。

### 3)使用`operator`模块中的`mul`函数创建一个名为`double`的`partial`，它总是提供`2`作为第一个参数。

在这种情况下，我们只需要在创建`partial`时传入一个位置参数，因为我们只对修复第一个参数感兴趣。

我们需要在这里进行两次导入。首先，我们需要从`operator`导入`mul`，其次，我们需要从`functools`导入`partial`。

```
`from operator import mul
from functools import partial` 
```

在这种情况下，创建分部相当简单。

```
`from operator import mul
from functools import partial

double = partial(mul, 2)` 
```

### 4)使用以读取(`"r"`)模式打开文件的`partial`创建一个`read`函数。

创建这个`partial`的过程与练习 3 中的 on 非常相似，但是这次我们需要使用一个关键字参数。

这种情况下我们需要赋值的参数名是`mode`。

```
`from functools import partial

read = partial(open, mode="r")` 
```

我们现在可以像使用`open`函数一样使用这个`read`函数:它甚至可以在上下文管理器中工作！