# 第 29 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-29-exercise-solutions/>

这是我们在 Python 系列 [30 天中](https://blog.teclado.com/30-days-of-python/)[第 29 天练习](/30-days-of-python/python-30-day-29-decorators)的解决方案。在检查解决方案之前，请确保您亲自尝试了该练习！

### 1)制作一个装饰器，它调用给定的函数两次。你可以假设这些函数不返回任何重要的东西，但是它们可以接受参数。

你可以随便称呼你的室内设计师，但我在这里要说`double`。

我们知道我们需要马上做的事情很少。我们需要我们的装饰器接受一个函数作为参数，并且我们需要返回一个对我们在装饰器的函数体中定义的函数的引用。

使用`wraps`保存原始的函数名和文档也是一个好主意。

```
`from functools import wraps

def double(func):
    @wraps(func)
    def inner():
        pass

    return inner` 
```

因为我们的函数可以接受参数，所以我们应该使用`*args`、`**kwargs**`将`inner`函数定义为接受任意数量的位置和关键字参数。然后我们应该在调用`inner`中的函数时传递这些参数。

```
`from functools import wraps

def double(func):
    @wraps(func)
    def inner(*args, **kwargs):
        func(*args, **kwargs)
        func(*args, **kwargs)

    return inner` 
```

### 2)假设您有一个名为`books`的列表，您的应用程序中有几个函数与它进行交互。写一个 decorator，使得你的函数只有在`books`不为空时才运行。

让我们从设置我们通常的装饰样板开始。我还将创建一个名为`books`的列表。

```
`from functools import wraps

books = []

def requires_content(func):
    @wraps(func)
    def inner(*args, **kwargs):
        pass

    return inner` 
```

这里的实际逻辑相当简单。我们只需要检查`inner`里面`books`的真值，只有条件求值为`True`时才调用函数。

```
`from functools import wraps

books = []

def requires_content(func):
    @wraps(func)
    def inner(*args, **kwargs):
        if books:
            func(*args, **kwargs)

    return inner` 
```

我们可能还应该返回`func`的返回值，以防函数返回什么。

```
`from functools import wraps

books = []

def requires_content(func):
    @wraps(func)
    def inner(*args, **kwargs):
        if books:
            return func(*args, **kwargs)

    return inner` 
```

### 3)编写一个名为`printer`的装饰器，让任何被装饰的函数打印它们的返回值。如果给定函数的返回值是`None`，`printer`应该什么都不做。

这个练习与上一个非常相似:我们只需要一个基于函数返回值的条件，而不是某个外部变量。

让我们再次从样板文件开始。

```
`from functools import wraps

def printer(func):
    @wraps(func)
    def inner(*args, **kwargs):
        pass

    return inner` 
```

这里的第一步是实际调用`inner`中的函数。

```
`from functools import wraps

def printer(func):
    @wraps(func)
    def inner(*args, **kwargs):
        return_value = func(*args, **kwargs)

    return inner` 
```

现在我们需要检查存储在`return_value`中的值。

我们在这里不只是检查`return_value`的真值，这一点非常重要，因为函数可能会返回大量其他假值。例如，`0`、`False`或`[]`。

```
`from functools import wraps

def printer(func):
    @wraps(func)
    def inner(*args, **kwargs):
        return_value = func(*args, **kwargs)

        if return_value is not None:
            print(return_value)

    return inner` 
```

注意检查`None`时的习语是使用`is`和`is not`，而不是`==`和`!=`。这是可行的，因为`None`的所有实例在内存中实际上都是相同的值。