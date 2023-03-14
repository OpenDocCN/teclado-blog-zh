# 第 15 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-15-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 15 天练习](/30-days-of-python/python-30-day-15-comprehensions)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)将下面的 for 循环转换成一个理解。

```py
`numbers = [1, 2, 3, 4, 5]
squares = []

for number in numbers:
    squares.append(number ** 2)` 
```

当构建一个理解时，我们需要考虑两件事。我们试图添加到新集合中的值，以及允许我们从原始集合中获取项目的循环结构。

在上面的代码中，我们想要添加到新集合中的值是`number ** 2`，或者是`number`值的平方。我们通过迭代`numbers`列表来获取这些数字，从列表中一次获取一个条目。如我们所见，我们使用的循环是:

现在我们有了这些组成部分，我们可以把理解放在一起。在这种情况下，我将使用列表理解，因为我们正在尝试制作一个新的列表。

我们的出发点是:

```py
`numbers = [1, 2, 3, 4, 5]
squares = []` 
```

现在，在方括号内，我们首先放入我们想要添加的值，然后是循环。

```py
`numbers = [1, 2, 3, 4, 5]
squares = [number ** 2 for number in numbers]` 
```

### 2)使用字典理解从下面的字典创建新字典，其中每个值都是标题大小写。

```py
`movie = {
    "title": "thor: ragnarok",
    "director": "taika waititi",
    "producer": "kevin feige",
    "production_company": "marvel studios"
}` 
```

让我们把它写成一个常规的`for`循环:

```py
`movie = {
    "title": "thor: ragnarok",
    "director": "taika waititi",
    "producer": "kevin feige",
    "production_company": "marvel studios"
}

updated_movie = {}

for key, value in movie.items():
    updated_movie.update({key: value.title()})` 
```

记住，默认情况下，我们只在遍历字典时获取键，所以如果我们想要键和值，我们需要使用`items`方法。

现在，让我们像以前一样分解它。我们要添加的是`key: value.title()`，其中`for`循环是`for key, value in movie.items()`。

我们现在可以把它放在`updated_movie`旁边的花括号里，就像这样:

```py
`movie = {
    "title": "thor: ragnarok",
    "director": "taika waititi",
    "producer": "kevin feige",
    "production_company": "marvel studios"
}

updated_movie = {key: value.title() for key, value in movie.items()}` 
```

当然，因为我们正在使用一个理解，如果我们愿意，我们可以重用相同的名称。

```py
`movie = {
    "title": "thor: ragnarok",
    "director": "taika waititi",
    "producer": "kevin feige",
    "production_company": "marvel studios"
}

movie = {key: value.title() for key, value in movie.items()}` 
```