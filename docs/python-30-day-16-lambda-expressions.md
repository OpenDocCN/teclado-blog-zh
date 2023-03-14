# 第 16 天:第一类函数和 Lambda 表达式

> 原文：<https://blog.teclado.com/python-30-day-16-lambda-expressions/>

欢迎来到 Python 系列 [30 天的第 16 天！今天我们将讨论一级函数的概念。Python 中的所有函数都是一级函数，我们将稍微讨论一下这意味着什么，以及它为什么有用。](https://blog.teclado.com/30-days-of-python/)

我们还将看到使用 lambda 表达式定义简单函数的不同方式。

## 什么是一流的功能？

当一种编程语言中的函数被像对待其他类型的函数一样对待时，该语言被称为具有一级函数。据说他们是这种语言的“一等公民”。

一般来说，这意味着我们可以将函数作为参数传递给其他函数，从函数中返回它们，并将它们赋给变量。

在 Python 中，函数确实是一等公民，我们可以做我上面列出的所有事情。让我们看一些例子。

## 将函数分配给变量

首先，我们来谈谈将函数赋给变量，因为这实际上是在 Python 中创建函数的一个至关重要的部分。

当我们使用`def`定义函数时，我们为函数提供了一个名称。当我们这样做的时候会发生两件事。

这个函数是根据我们的规范命名的。

回顾第 13 天的[，当我们打印出全局名称空间时，我们发现了如下条目:](/30-days-of-python/python-30-day-13-return-statements/)

```
`<function add at 0x7fae512021f0>` 
```

函数名成为函数表示的一部分。

Python 用这个名字为我们创建了一个变量，它把函数赋给这个名字。

这实际上是两件非常不同的事情，我们可以通过将一个函数赋给不同的变量名，并打印出与这些不同名称相关的值来演示这一点。

```
`def add(a, b):
    print(a + b)

adder = add

print(add)
print(adder)` 
```

如果我们运行代码，我们将得到类似如下的输出:

```
`<function add at 0x7f11bc6451f0>
<function add at 0x7f11bc6451f0>` 
```

这里有几件有趣的事情需要注意。

首先，这两个函数是完全相同的对象。我们可以看到这一点，因为它们的内存地址(在最右边)是相同的。

其次，当我们打印函数的字符串表示时，这两个函数仍然被称为`add`。第二个输出没有变成这样:

```
`<function adder at 0x7f11bc6451f0>` 
```

这是因为我们用来引用函数的变量名和函数名是两码事。

通过执行以下操作，我们可以非常清楚地展示这一点:

```
`def add(a, b):
    print(a + b)

adder = add
del add

add(5, 4)  # NameError` 
```

这里我们已经删除了变量名`add`，所以我们不能再用这个名字调用函数。如果我们尝试，我们会得到一个`NameError`:

```
`Traceback (most recent call last):
  File "main.py", line 8, in <module>
    add(5, 4)
NameError: name 'add' is not defined` 
```

尽管如此，我们仍然可以调用`adder`，函数的名字仍然是`add`:

```
`def add(a, b):
    print(a + b)

adder = add
del add

adder(5, 4)   # 9
print(adder)  # <function add at 0x7f03780ae1f0>` 
```

## 作为参数的函数

既然我们可以将函数赋给变量，那么我们也可以将函数作为参数传入是有意义的。为什么？因为形参实际上只是变量，而实参是我们赋给那些变量的值。

让我们看一个有趣的例子，我们可以把一个函数作为参数传入。

Python 有一个名为`max`的函数，它可以让我们在一些 iterable 中找到最大值。

对于元素具有自然顺序的 iterable 来说，它非常容易使用:我们只需将 iterable 传递给 max，它就会发挥它的魔力。

```
`numbers = [56, 3, 45, 29, 102, 30, 73]
highest_number = max(numbers)

print(highest_number)  # 102` 
```

然而，并不是每组值都有明显的顺序。例如，假设我有一个这样的字典列表:

```
`students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]` 
```

实际上，我们可以按字母顺序或平均成绩来排序。然而，在这种情况下，`max`实际上是试图使用`>`操作符将整个字典与另一个字典进行比较，这甚至都不是合法的操作。

在这种情况下，我们可以通过为名为`key`的关键字参数提供一个函数来帮助`max`函数。iterable 中的每一项都调用这个函数，每一项都传递给这个函数，一次一项。

这个函数所期望的是一个可排序的值，它可以用来确定给定项目的顺序。

例如，我们可以提供这样一个函数:

```
`def get_grade_average(student):
    return student["grade_average"]` 
```

该函数接受一个字典，并返回与该字典的`"grade_average"`键相关联的值。在我们的例子中，这是一个整数，可以使用`>`合法地比较整数。

我们现在可以这样做:

```
`def get_grade_average(student):
    return student["grade_average"]

students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]

valedictorian = max(students, key=get_grade_average)
print(valedictorian)` 
```

这里我们提供了`get_grade_average`函数作为`max`的参数，`max`在内部调用这个函数来确定列表中条目的顺序。然后，它正确地将该词典作为平均分数最高的学生返回:

```
`{"name": "Lauren", "grade_average": 92}` 
```

还有其他函数和方法需要类似的函数。例如，`sorted`函数和`sort`方法都接受键，键必须是函数。

## 从其他函数返回函数

在这一点上，我认为很明显我们*可以从其他函数中返回函数，但问题是，我们为什么要这样做？*

在这里，我想向您展示一个使用字典的`get`方法的模式，它将允许我们让用户选择一个函数来运行。方法实际上只是函数的特殊类型，所以`get`将作为一个很好的演示。

首先，我们需要一些函数，所以我将使用我们在第 12 天的[练习中定义的运算符函数。](/30-days-of-python/python-30-day-12-exercise-solutions/)

```
`def add(a, b):
    print(a + b)

def subtract(a, b):
    print(a - b)

def multiply(a, b):
    print(a * b)

def divide(a, b):
    if b == 0:
        print("You can't divide by 0!")
    else:
        print(a / b)` 
```

这些都是很好的例子，因为它们都有两个论点，而且主题相似。

下一步是定义这样一个字典:

```
`operations = {
    "a": add,
    "s": subtract,
    "m": multiply,
    "d": divide
}` 
```

值得注意的是，我们在这里并没有调用函数:我们只是使用它们的变量名来引用函数。

现在我们有了一个字典，其中的键是字符串，与这些键相关联的值是函数。我们现在可以要求用户从这些选项中进行选择，我们将通过在字典中查找他们的选项来为他们运行该函数:

```
`def add(a, b):
    print(a + b)

def subtract(a, b):
    print(a - b)

def multiply(a, b):
    print(a * b)

def divide(a, b):
    if b == 0:
        print("You can't divide by 0!")
    else:
        print(a / b)

operations = {
    "a": add,
    "s": subtract,
    "m": multiply,
    "d": divide
}

selected_option = input("""Please select one of the following options:

a: add
s: subtract
m: multiply
d: divide

What would you like to do? """)

operation = operations.get(selected_option)

if operation:
    a = int(input("Please enter a value for a: "))
    b = int(input("Please enter a value for b: "))

    operation(a, b)
else:
    print("Invalid selection")` 
```

当我们调用`get`时，我们在字典中查找用户的字符串，并且(假设没有出错)我们得到的是一个函数。然后，我们可以将这个函数赋给一个变量名，并像调用任何其他函数一样调用它。

如果确实出错了，我们有一些保护措施，因为当没有找到键时，`get`方法返回`None`,我们可以使用一个条件语句来检查这一点，以确保我们没有得到一个 falsy 值。如果我们做了，我们知道我们没有得到一个函数，所以我们可以让用户知道他们的选择是无效的。

## λ表达式

Lambda 表达式是定义简单函数的另一种语法。lambda 表达式的一个非常有用的特性是，它们是*表达式*，它们评估的值是我们想要创建的函数。

相反，使用`def`关键字的函数定义是语句。它们没有一个值，这就是为什么 Python 不厌其烦地为我们指定函数的名称。这样做的一个主要限制是，我们总是必须将我们的函数与我们想要使用它们的地方分开定义。

例如，我们必须在使用`max`中的关键函数之前定义它:

```
`def get_grade_average(student):
    return student["grade_average"]

students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]

valedictorian = max(students, key=get_grade_average)
print(valedictorian)` 
```

使用`def`时，无法将函数定义为`max`调用的一部分。然而，当使用 lambda 表达式时，我们没有这样的限制，这使得它们对于像 key 函数这样的通常只有一行函数体的东西非常方便。

好的，那么我们如何写一个 lambda 表达式呢？

任何 lambda 表达式的第一部分都是关键字`lambda`。在 lambda 关键字之后，我们可以选择为我们想要创建的函数指定任何参数。与常规函数定义不同，这些值没有放在括号内。lambda 表达式的这一部分用冒号封闭。

冒号后面是一个表达式，这个表达式就是我们想要从函数中返回的。

让我们看几个例子，从之前的`add`函数开始。

这里我们有两个参数，`a`和`b`，它们位于`lambda`关键字之后，冒号之前。冒号标志着我们想要定义的参数的结尾，后面的所有内容都是我们想要从函数中返回的。在这种情况下，我们要返回的是`a + b`。

为了便于比较，下面是使用`def`编写的相同函数:

```
`def add(a, b):
    return a + b` 
```

现在让我们看看我们用作`max`的关键函数的`get_grade_average`:

```
`def get_grade_average(student):
    return student["grade_average"]` 
```

这个函数接受一个参数`student`，我们期望它是一个字典。它返回订阅表达式的结果，该表达式在提供的字典中查找与键`"grade_average"`相关联的值。

以 lambda 表达式的形式编写，该函数如下所示:

```
`lambda student: student["grade_average"]` 
```

我们可以在我们的小应用程序中使用它来查找分数最高的学生，如下所示:

```
`students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]

valedictorian = max(students, key=lambda student: student["grade_average"])
print(valedictorian)` 
```

如您所见，lambda 表达式只是替换了对函数的引用，因为 lambda 表达式本身产生了一个函数。

我们可以很好地使用 lambda 表达式的另一个地方是我们之前的运算符映射字典:

```
`def divide(a, b):
    if b == 0:
        return "You can't divide by 0!"
    else:
        return a / b

operations = {
    "a": lambda a, b: a + b,
    "s": lambda a, b: a - b,
    "m": lambda a, b: a * b,
    "d": divide
}

selected_option = input("""Please select one of the following options:

a: add
s: subtract
m: multiply
d: divide

What would you like to do? """)

operation = operations.get(selected_option)

if operation:
    a = int(input("Please enter a value for a: "))
    b = int(input("Please enter a value for b: "))

    print(operation(a, b))
else:
    print("Invalid selection")` 
```

这里我们唯一不能替换的函数是`divide`，因为`divide`里面有一个条件语句。Lambda 表达式仅限于单个表达式，不能包含语句。

记住，Python 中的所有函数都是一级函数，所以如果我们愿意，可以将我们用 lambda 表达式创建的函数赋给变量。

```
`add = lambda a, b: a + b

print(add(7, 8))  # 15` 
```

一般来说，这不是很有用。在这种情况下，用`def`定义函数通常更合适。它们更具可读性，在这种情况下，通过编写 lambda 表达式节省几行代码并不是一个很好的权衡。

### 风格注释

非常小心，不要在代码中滥用 lambda 表达式。虽然我们可以编写非常长且复杂的表达式作为返回值，但这不是一个好主意。

如果您需要一个执行任何真正复杂逻辑的函数，最好使用`def`来定义一个函数，并将操作分解成更简单的步骤，以便您可以使用描述性变量和注释来记录正在发生的事情。

## 练习

1)使用`sort`方法按照学生姓名的字母顺序排列以下列表:

```
`students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]` 
```

这里你需要传入一个函数作为键，这取决于你是使用 lambda 表达式，还是用`def`定义一个函数。

2)将以下函数转换为 lambda 表达式，并将其赋给一个名为`exp`的变量。

```
`def exponentiate(base, exponent):
    return base ** exponent` 
```

3)打印您在之前的练习中使用 lambda 表达式创建的函数。创建的函数的名称是什么？

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-16-exercise-solutions/)