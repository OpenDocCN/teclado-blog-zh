# 第 13 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-13-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 13 天练习](/30-days-of-python/python-30-day-13-return-statements)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)定义一个接受两个数字的`exponentiate`函数。第一个是基数，第二个是把基数提高到多少的功率。该函数应该返回该操作的结果。

这个函数与我们在昨天的练习中定义的运算函数非常相似。我们只需要返回一个值，而不是打印它。

首先让我们定义函数并设置参数。

```
`def exponentiate(base, exponent):
    pass` 
```

Python 中的取幂运算符是`**`，我们只需要返回`base ** exponent`即可。

```
`def exponentiate(base, exponent):
    return base ** exponent` 
```

记住这里操作数的顺序很重要。你不能这样做:

```
`def exponentiate(base, exponent):
    return exponent ** base` 
```

### 2)定义一个`process_string`函数，该函数接收一个字符串并返回一个新的字符串，该字符串已被转换为小写，并已删除任何多余的空格。

至此，我们已经完成了大量的字符串处理，所以这应该是一个相当简单的练习。

首先，让我们再次定义你的函数的框架。这次我们只接受一个参数，我称之为`raw_string`。

```
`def process_string(raw_string):
    pass` 
```

现在我要在这个原始字符串上调用`strip`和`lower`。我们可以在同一步骤中返回结果:

```
`def process_string(raw_string):
    return raw_string.strip().lower()` 
```

### 3)编写一个函数，该函数接收一个包含演员信息的元组，并将该数据作为字典返回。

在这种情况下，我们被告知需要以下格式的数据:

```
`("Tom Hardy", "English", 42)` 
```

所以，首先我们有一个名字，然后是演员的国籍，然后是他们的年龄。

我们再次需要定义一个接受单个参数的函数。我将调用我的函数`dictify`，参数将是`actor`。

接下来，我将析构元组，这样我们就可以在构造字典时使用好的变量名。

```
`def dictify(actor):
    name, nationality, age = actor` 
```

最后，我将创建并返回新字典:

```
`def dictify(actor):
    name, nationality, age = actor

    return {
        "name": name,
        "nationality": nationality,
        "age": age
    }` 
```

### 4)编写一个函数，该函数接受一个数字，并根据该数字是否为质数返回`True`或`False`。

在本练习中，我将使用从[第 8](/30-days-of-python/python-30-day-8-while-loops/) 天开始的以下实现:

```
`dividend = int(input("Please enter a number: "))

for divisor in range(2, dividend):
    if dividend % divisor == 0:
        print(f"{dividend} is not prime!")
        break
else:
    print(f"{dividend} is prime!")` 
```

我们将得到一个参数，而不是从用户那里得到一个数字，所以`dividend`将成为一个参数。函数本身将被称为`is_prime`。

```
`def is_prime(dividend):
    pass` 
```

现在不用打印出`dividend`是否是质数，我们只需要返回`True`或`False`。

```
`def is_prime(dividend):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            return False
    else:
        return True` 
```

注意，我们不再需要`break`语句，因为`return`关键字已经结束了函数的执行，因此也结束了循环。

我们也不再需要`else`条款了。

```
`def is_prime(dividend):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            return False

    return True` 
```

我们需要注意的一些事情是数字`1`、`0`和任何负数。这些数字对于我们的参数都是有效值，但是它们会给出错误的输出。

一个简单的条件语句可以捕捉这些有问题的数字。

```
`def is_prime(dividend):
    if dividend < 2:
        return False

    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            return False

    return True` 
```