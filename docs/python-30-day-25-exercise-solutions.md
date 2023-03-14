# 第 25 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-25-exercise-solutions/>

以下是我们对 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 25 天](/30-days-of-python/python-30-day-25-idiomatic-python)练习的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)编写一个函数，提示用户输入他们的名字，然后问候他们。如果在处理完字符串后，剩下一个空字符串，函数应该在输出中用“World”替换这个空字符串。

让我们从定义我们的函数开始。

我们需要放入函数体的第一件事是我们获取用户名的`input`调用。我要在同一行处理这个字符串。

```
`def greeter():
    name = input("Please enter your name: ").strip().title()` 
```

剩下唯一要做的就是打印问候。我们可以在 f 字符串中直接使用`or`来改变输出，这取决于`name`是否以空字符串结束。

```
`def greeter():
    name = input("Please enter your name: ").strip().title()
    print(f"Hello, {name  or  'World'}!")` 
```

注意不要在花括号内使用与外部字符串相同的引号类型。如果你这样做，你将提前终止你的字符串，这将导致一个错误。

### 2)编写一个函数来确定一个字符串是否只包含 ASCII 字母。

再一次，让我们从定义函数的框架开始。

```
`def is_ascii_letters(test_string):
    pass` 
```

对于这个练习，我认为我们最好的方法是导入`string`模块并利用其中定义的字符串常量。有一个叫做`ascii_letters`的包含了所有大写和小写的 ASCII 字母，这正是我们想要的。

然后我们可以循环遍历`test_string`并检查给定的字符是否在`ascii_letters`中。如果我们找到一个不在这个字符串常量中的字符，我们可以马上返回`False`。

```
`from string import ascii_letters

def is_ascii_letters(test_string):
    for character in test_string:
        if character not in ascii_letters:
            return False
    else:
        return True` 
```

请注意，我们在这里使用的是带有`for`循环的`else`子句，而不是循环中的条件语句。我们在[第八天](/30-days-of-python/python-30-day-8-while-loops/)谈到了这一点。

这是一个完美的解决方案，但是我们不需要实现这样一个冗长的循环结构。相反，我们可以将`all`函数与生成器表达式结合使用。

```
`from string import ascii_letters

def is_ascii_letters(test_string):
    return all(character in ascii_letters for character in test_string)` 
```

这在功能上是相同的，但是使用了我们在上周课程中学到的一些更高级的工具。

### 3)使用`random`模块中的`sample`函数创建三个列表，每个列表包含从`1`到`100`的十五个数字。将这些列表按降序排序(最大的先排序)，然后截断每个列表，使每个列表中只保留 5 个项目。

让我们从导入`sample`并生成我们的初始数字列表开始。`sample`函数需要一个总体，以及一个表示应该从总体中选择多少个数字的值。人口需要是一个序列，所以我们可以提供一个`range`。

```
`import random

numbers = [random.sample(range(1, 101), 15) for _ in range(3)]` 
```

这是一行相当密集的代码，但我认为它仍然足够可读。如果你愿意，你可以把人口抽取成一个变量，这样我们就不会在理解中到处都有范围。

```
`import random

population = range(1, 101)
numbers = [random.sample(population, 15) for _ in range(3)]` 
```

我们的下一步是对从`sample`返回的列表进行排序。我们可以使用`sorted`函数作为理解的一部分，在一个步骤中完成这一切。

因为我们希望数字以降序排列，所以我们需要为参数`reverse`提供一个值，传入`True`。

```
`import random

population = range(1, 101)
numbers = [sorted(random.sample(population, 15), reverse=True) for _ in range(3)]` 
```

然而，我认为这对于理解来说有点太多了，我们无论如何都要用一个`for`循环来迭代这个集合。

相反，我会像这样使用`sort`方法:

```
`import random

population = range(1, 101)
numbers = [random.sample(population, 15) for _ in range(3)]

for number_set in numbers:
    number_set.sort(reverse=True)` 
```

我们需要再次记住为`reverse`传入一个值，以便列表按降序排序。

现在我们可以使用`del`和 slice 语法来截断列表，作为同一个循环的一部分。

```
`import random

population = range(1, 101)
numbers = [random.sample(population, 15) for _ in range(3)]

for number_set in numbers:
    number_set.sort(reverse=True)
    del number_set[5:]` 
```

如果我们现在打印`numbers`，我们应该有三个列表，每个列表都有最高的五个数字。