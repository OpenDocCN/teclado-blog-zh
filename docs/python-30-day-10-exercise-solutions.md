# 第 10 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-10-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 10 天练习](/30-days-of-python/python-30-day-10-dictionaries)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)下面是描述相册的元组。将这个外部元组转换成一个有四个键的字典。

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

上面的元组包含四条信息:专辑名称、乐队名称、专辑发行年份和曲目列表。

我们字典的每个键都必须描述这个信息，所以我将使用下面的键:`"title"`、`"artist"`、`"year"`和`"tracks"`。如果您喜欢，可以随意选择不同的键名。

现在只需要将值映射到这些键上。

```py
`{
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}` 
```

请记住，每个键和值由冒号(`:`)分隔，每个键值对由逗号分隔。

### 2)迭代您在练习 1 中创建的字典的键和值。对于每个键和值，您应该打印键的名称，然后在旁边打印值。

首先，让我们将这个字典赋给一个变量，这样我们可以更容易地引用它。

```py
`album = {
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}` 
```

请记住，默认情况下，当我们迭代一个字典时，我们只会得到键。这不是我们在这种情况下想要的，因为我们想要键**和**的值。

获得键和值的最好方法是在我们想要迭代字典时调用字典上的`items`方法。

然后，只需要将这些值赋给两个变量，并在循环体中引用它们。

```py
`for key, value in album.items():
    print(f"{key}: {value}")` 
```

### 3)从您创建的词典中删除曲目列表和发行年份。完成后，向字典中添加一个新的键来存储发布日期。

我们有很多从字典中删除条目的选择。这里我将使用`del`关键字。

我们将再次使用变量名`album`作为字典。

```py
`album = {
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}` 
```

使用`del`，我们可以像这样去掉两个键:

```py
`album = {
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}

del album["year"]
del album["tracks"]` 
```

现在我们必须向字典中添加一个新的键和值。我准备叫它`release_date`，《月球背面》上映日期是 1973 年 3 月 1 日。

```py
`album = {
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}

del album["year"]
del album["tracks"]

album["release_date"] = "March 1st, 1973"` 
```

### 4)尝试检索您从字典中删除的一个值。这应该会给你一个`KeyError`。一旦您尝试了这一点，使用`get`方法重复该步骤以防止引发异常。

对于上面的字典，我们可以尝试访问`"year"`或`"tracks"`。哪个都无所谓。

首先，让我们试着打印与`"year"`键相关的值。

```py
`album = {
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}

del album["year"]
del album["tracks"]

album["release_date"] = "March 1st, 1973"

print(album["year"])` 
```

这将会给我们一个`KeyError`预期:

```py
`Traceback (most recent call last):
  File "main.py", line 24, in <module>
    print(album["year"])
KeyError: 'year'` 
```

让我们把这一行注释掉，这样就不会破坏我们的程序。现在让我们尝试使用`get`来访问`"year"`键。在这种情况下，我将指定一个默认值`"Unknown"`。

```py
`album = {
    "title": "The Dark Side of the Moon",
    "artist": "Pink Floyd",
    "year": 1973,
    "tracks": (
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
}

del album["year"]
del album["tracks"]

album["release_date"] = "March 1st, 1973"

# print(album["year"])

print(album.get("year", "Unknown")` 
```

现在，我们没有得到异常，而是将`"Unknown"`打印到控制台。