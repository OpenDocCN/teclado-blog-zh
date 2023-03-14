# 第 27 天项目:鳄梨

> 原文：<https://blog.teclado.com/python-30-day-27-project/>

欢迎来到 Python 系列 [30 天的](https://blog.teclado.com/30-days-of-python/)[天 27](/30-days-of-python/python-30-day-27-local-development) 项目！在这个项目中，我们将使用名为`pandas`的第三方库进行一些数据分析。

是一个数据分析库，旨在使处理关系数据更加容易。

在开始使用`pandas`之前，我们还需要了解很多内容，所以让我们开始吧！

我们有这个项目的[视频预演](https://youtu.be/g_rfQQC2BjA)。但是我们建议在看视频之前通读一下初级读本！

## 关于`pandas`的快速入门

### 装置

`pandas`不是 Python 标准库的一部分，所以我们必须在使用它之前安装它。如果您正在使用 PyCharm，您可以按照今天的帖子中的说明安装软件包。

如果你在不同的编辑器中工作，你可能想要使用`pip`安装`pandas`。在`pandas` [文档](https://pandas.pydata.org/docs/getting_started/install.html#installing-from-pypi)中有如何操作的说明。

### `Series`和`DataFrame`类型

为了使用`pandas`，我们需要熟悉两种基本类型。

第一个是`DataFrame`类型，用于存储表格数据。它由可以填充数据的行和列组成。

我们可以用几种方法创建一个`DataFrame`，其中一种是使用字典。在这种情况下，字典键成为列的标题，与这些键相关联的值填充该列的单元格。

```
`import pandas as pd

movies = {
    "title": ("Inception", "Pirates of the Caribbean: The Curse of the Black Pearl"),
    "director": ("Christopher Nolan", "Gore Verbinski"),
    "year": (2010, 2003)
}

df = pd.DataFrame(movies)` 
```

如果我们打印`df`，我们实际上会得到一些非常好的表格输出，有点像这样:

| id |头衔|导演|年份| |-|-|-| | | 0 |盗梦空间|克里斯托弗·诺兰| 2010 | | 1 |加勒比海盗:布拉的诅咒...|戈尔·维宾斯基| 2003 年|

第一列包含一个索引，其他列包含我们在`movies`字典中指定的数据。

如果我们想看到这个`DataFrame`的某个子集，我们可以打印`df.head()`，传入我们想看到的行数。这对这个项目非常有用，因为我们使用的数据集有 18，000 多行！

我们的`DataFrame`其实是由其他几个叫做`Series`的结构组成的。我们的`DataFrame`中的每一列都是一个`Series`，它只包含给定列中的值，以及每个值的相关索引。

### 风格注释

导入`pandas`时有一个通用的约定，使用下面的语法:

我建议你遵循这个惯例，不要用别名或者你自己创造的别名来导入`pandas`。对于经常使用`pandas`的人来说，看起来会非常奇怪。

### 修改一个`DataFrame`

我们经常想要对`DataFrame`进行修改，比如删除不必要的列，或者重命名标题。在这个项目中，我们必须双管齐下。

首先，我们来谈谈标题的重命名。我们可以通过在一个`DataFrame`上调用`rename`方法，传入一个字典来实现。这个字典的键表示我们想要替换的名称，与这些键相关联的值显示我们想要用什么来替换现有的名称。

```
`import pandas as pd

movies = {
    "title": ("Inception", "Pirates of the Caribbean: The Curse of the Black Pearl"),
    "director": ("Christopher Nolan", "Gore Verbinski"),
    "year": (2010, 2003)
}

df = pd.DataFrame(movies).rename(columns={"year": "release_year"})` 
```

像`pandas`中的许多方法一样，`rename`方法可以用另一个关键字参数`inplace`来调用。这告诉`pandas`我们是想要创建一个`DataFrame`的副本，还是想要修改一个现有的`DataFrame`。

因此，我们可以这样做:

```
`...

df = pd.DataFrame(movies)
df.rename(columns={"year": "release_year"}, inplace=True)` 
```

现在来说说抛柱。实际上有很多不同的方法可以做到这一点，但为了简洁起见，我在这里只讨论两种。我们将使用的第一个选项是`drop`。

如果我们只想从我们的`DataFrame`中删除一两列的话，`drop`是很好的选择。

让我们使用`drop`从我们一直使用的`DataFrame`中去掉`director`列。

```
`...

df = pd.DataFrame(movies)

df.rename(columns={"year": "release_year"}, inplace=True)
df.drop(columns="director",  inplace=True)` 
```

我们可以看到，格式与使用`rename`非常相似。

如果我们想指定一些列来保留，我们可以从现有的列创建一个新的`DataFrame`,如下所示:

```
`...
df = pd.DataFrame(movies)

df.rename(columns={"year": "release_year"}, inplace=True)
df = df[["title", "release_year"]]` 
```

注意这里双方括号的使用。这真的很重要，否则`pandas`会认为你想要一个`Series`。

### 注意

使用`inplace=True`时，您可能会在控制台中看到这样的消息:

```
`SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame` 
```

在这些情况下，您应该在试图删除列的`DataFrame`上调用`copy`方法。你可以在这里找到更多关于这个[的信息。](https://stackoverflow.com/a/27680109)

### 按值过滤

处理数据的另一个重要部分是能够关注值的某个子集。

同样，对于如何使用`pandas`来做这件事，我们有很多选择。在这篇文章中，我将只讨论`query`方法，因为它是最容易理解的方法之一，并且对于大型数据集非常有效。

使用`query`相当简单，我们只需要提供一个字符串，其中包含一些条件来过滤我们的`DataFrame`。该条件包含普通的 Python 逻辑，因此我们不需要学习任何新的东西。

例如，假设我们之前的`DataFrame`现在已经填充了许多电影，我想从`2020`获取所有电影。我们可以这样做:

```
`...

latest_movies = df.query("year == 2020")` 
```

请注意，列标题不是字符串，字符串数据需要用一组内部引号括起来。

例如，如果我们只想要克里斯托弗·诺兰的电影，我们必须这样做:

```
`...

nolan_movies = df.query("director == 'Christopher Nolan'")` 
```

如果要对这些`DataFrame`对象做进一步的修改，就要做一个拷贝，否则`pandas`会投诉。请参见上一节的注释。

### 注意

如果我们想在查询字符串中引用变量名，我们必须使用特殊的语法，这样`pandas`就不会认为我们引用的是列标题或索引。

我们指定某个东西是变量的方法是在变量名前加一个`@`。

### 从哪里了解更多信息

我们已经在这里介绍了`pandas`所能提供的很小一部分，我强烈建议你浏览他们优秀的[入门教程](https://pandas.pydata.org/docs/getting_started/index.html)来了解更多。

## 案情摘要

在这个项目中，我们将使用 [Kaggle](https://www.kaggle.com/) 上的数据集，如果你对使用 Python 进行数据科学感兴趣，这是一个非常棒的网站。它包含成吨的真实数据集供你使用，完全免费。

我们今天要处理的数据可以在[这里](https://www.kaggle.com/neuromusic/avocado-prices/)找到。

它包含了美国不同地区几年来成千上万的鳄梨价格记录。

对于这个项目，我想知道关于这个数据集中的数据的一些不同的事情:

1.  我想知道哪个地区每年传统种植的鳄梨的平均价格最低，我还想知道有机鳄梨的相同信息。
2.  我想知道哪一个地区每一年这两种鳄梨的平均价格最高。
3.  我想知道传统种植和有机鳄梨的最低价格，也想知道最高价格。

`pandas`有一些内置的计算平均值的工具，所以你可能想看看[的文档](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.mean.html#pandas.core.groupby.GroupBy.mean)来看看怎么做。还有一些方法可以找到`Series`的[最小值](https://pandas.pydata.org/docs/reference/api/pandas.Series.min.html#pandas.Series.min)和[最大值](https://pandas.pydata.org/docs/reference/api/pandas.Series.max.html)。

最后一点，源数据有很多我们不需要的字段。考虑将数据精简到地区、年份、鳄梨的种类和价格。

祝你好运！

## 我们的解决方案

首先，我们需要实际下载我们的源数据，并将 CSV 文档放入我们的项目工作区。我将把我的`avocado.csv`文件放在我的`app.py`旁边的根文件夹中。

现在我们有了数据，我们需要导入`pandas`并从文件中读取数据。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    pass` 
```

有几种方法可以将数据从文件中取出并放入`DataFrame`。例如，我们可以手动解析数据，或者我们可以使用`csv`模块来获取所有信息。然而，这是一个非常常见的操作，以至于`pandas`有一些内置工具可以直接从 CSV 文档创建一个`DataFrame`。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    df = pd.read_csv(avocado_prices)` 
```

这里,`read_csv`函数将解析`avocado_prices`中的数据，并将其转换为我们的`DataFrame`。在`pandas`中还有很多其他的`read_`功能可以处理其他格式。[教程](https://pandas.pydata.org/docs/getting_started/intro_tutorials/02_read_write.html#min-tut-02-read-write)更深入地谈到了这一点。

现在我们已经有了数据，下一步是删除所有我们不需要的字段。在这种情况下，我们只需要保留四个，所以我们不打算在这里使用`drop`。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    df = pd.read_csv(avocado_prices)

df = df[["year", "region", "AveragePrice", "type"]]` 
```

这里的一个问题是标题不太好看。它们不遵循我们习惯的 Python 命名约定，所以在创建初始的`DataFrame`时，我将把`AveragePrice`重命名为`price`。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    df = pd.read_csv(avocado_prices).rename(columns={"AveragePrice": "price"})

df = df[["year", "region", "price", "type"]]` 
```

接下来我将把`df`分成两个更小的`DataFrame`对象:一个用于传统种植的鳄梨，一个用于有机鳄梨。

我这样做的原因是因为我们设置的所有任务都不同地对待有机和传统鳄梨，所以我们在处理数据时会不断地筛选出一组。

为了得到传统的鳄梨，我将使用`query`方法。在这种情况下，我们的查询非常简单，我们需要鳄梨的值为`"conventional"`的所有行。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    df = pd.read_csv(avocado_prices).rename(columns={"AveragePrice": "price"})

df = df[["year", "region", "price", "type"]]

conventional = df.query("type == 'conventional'")` 
```

此时，我们不再真正需要这个更小的`DataFrame`的`type`列，因为所有的值都是相同的。我们可以将所有的`drop`列放在同一行，或者我们也可以就地放置。

如果我们做一个就地删除，我们必须记住在我们分配给`conventional`的行上调用`copy`。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    df = pd.read_csv(avocado_prices).rename(columns={"AveragePrice": "price"})

df = df[["year", "region", "price", "type"]]

conventional = df.query("type == 'conventional'").copy()
conventional.drop(columns="type", inplace=True)` 
```

现在我们可以对我们的有机鳄梨做同样的事情。

app.py

```
`import pandas as pd

with open("avocado.csv", "r") as avocado_prices:
    df = pd.read_csv(avocado_prices).rename(columns={"AveragePrice": "price"})

df = df[["year", "region", "price", "type"]]

conventional = df.query("type == 'conventional'").copy()
conventional.drop(columns="type", inplace=True)

organic = df.query("type == 'organic'").copy()
organic.drop(columns="type", inplace=True)` 
```

现在我们必须找出一种方法来计算和存储每个地区每年的平均价格。因为这很可能是一个复杂的过程，而且我们必须多次执行，所以我们最好定义一些函数。

我们还需要知道我们的数据集涵盖的年份。我们可以手动编写，或者通过在`year`列中查找唯一值，从原始的`df`中获取这些信息。

我们可以使用以下属性和方法来实现这一点:

为了方便地存储平均值，我将创建一个字典。这个字典的关键字将是我们数据集中不同的年份，与这些关键字相关联的值将是包含该特定年份数据的`DataFrame`对象。

这些`DataFrame`对象将包含每个地区的单独一行，该地区的价格反映了全年的平均价格。

首先，让我们定义一个函数，它可以获取给定年份的值，返回该年份的新的`DataFrame`。

app.py

```
`def filter_by_year(df, year):
    return df.query("year == @year").drop(columns="year")` 
```

接下来让我们定义一个函数，它将每个地区的年平均值排列到一个字典中。

app.py

```
`def get_average_by_year(df):
    averages = {}
    years = df.year.unique()

    for year in years:
        averages_for_year = filter_by_year(df, year).groupby("region").mean()
        averages.update({year: averages_for_year})

    return averages` 
```

我们现在可以像这样将它们插入到现有的代码中:

app.py

```
`import pandas as pd

def get_average_by_year(df):
    averages = {}
    years = df.year.unique()

    for year in years:
        averages_for_year = filter_by_year(df, year).groupby("region").mean()
        averages.update({year: averages_for_year})

    return averages

def filter_by_year(df, year):
    return df.query("year == @year").drop(columns="year")

with open("avocado.csv", "r")as avocado_prices:
    df = pd.read_csv(avocado_prices).rename(columns={"AveragePrice": "price"})

df = df[["year", "region", "price", "type"]]

conventional = df.query("type == 'conventional'").copy()
conventional.drop(columns="type", inplace=True)

organic = df.query("type == 'organic'").copy()
organic.drop(columns="type", inplace=True)

conventional_averages = get_average_by_year(conventional)
organic_averages = get_average_by_year(organic)` 
```

现在我们已经按照我们想要的方式组织了所有的数据，我们只需要几个 for 循环来输出我们需要的所有信息。

我建议为此编写一两个函数，因为我们需要多次重复代码，但这里的基本循环结构如下:

app.py

```
`...

for year, data in conventional_averages.items():
    highest_value = data.price.max()
    location = data.query("price == @highest_value").index[0]
    print(f"Highest price for conventional avocados in {year} was ${highest_value:.2f} in {location}.")

print()

for year, data in conventional_averages.items():
    lowest_value = data.price.min()
    location = data.query("price == @lowest_value").index[0]
    print(f"Lowest price for conventional avocados in {year} was ${lowest_value:.2f} in {location}.")

print()` 
```

这里的一切我们以前都见过，但我们确实需要提一下这个`.index[0]`。当我们按区域执行`groupby`操作时，这些区域变成了新`DataFrame`的行索引。因此，我们必须使用`index`属性来获取区域。这将给我们一个索引列表，在这种情况下，我们只是抓取第一个项目。

我们需要做的最后一件事是计算这两种鳄梨的总体最低和最高值。

app.py

```
`highest_conventional = conventional.price.max()
print(f"The highest price for conventional avocados was ${highest_conventional:.2f}")

lowest_conventional = conventional.price.min()
print(f"The lowest price for conventional avocados was ${lowest_conventional:.2f}")

highest_organic = organic.price.max()
print(f"The highest price for organic avocados was ${highest_organic:.2f}")

lowest_organic = organic.price.min()
print(f"The lowest price for organic avocados was ${lowest_organic:.2f}")` 
```

确保尝试重构输出数据的循环。您可能还想考虑将程序的不同部分分成不同的文件。

如果您想分享您的解决方案，请到我们的 [Discord 服务器](https://discord.gg/BBWwyMq)与我们聊天。