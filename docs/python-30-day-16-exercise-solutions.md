# 第 16 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-16-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 16 天练习](/30-days-of-python/python-30-day-16-lambda-expressions)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)使用`sort`方法按照学生姓名的字母顺序排列以下列表。

```py
`students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]` 
```

这里我们需要注意的第一件事是,`sort`方法并不真正知道如何对这组数据进行排序。它真正做的只是使用`>`和`<`进行一系列比较。对于字典来说，这甚至不是一个有效的操作，所以它甚至不能给出如何对所有内容进行排序的最佳猜测。

即使它能够对字典进行排序，它应该使用什么信息呢？我们的词典包含几条数据。应该按名字排序吗？还是按成绩？也许字典的长度是相关的。它应该尝试按此排序吗？有这么多可能的因素，我们需要指示`sort`我们想要它做什么。我们用钥匙来做这个。

正如我们在今天的帖子中看到的，键是一个返回某个值的函数，像`sort`这样的东西可以用它来确定我们的值的顺序。在这种情况下，我们希望按字母顺序对我们的名字进行排序，所以我们真的希望我们的键根据与`"name"`键相关的字符串进行排序。

首先让我们用一个常规函数来做这件事:

```py
`def get_name(student):
    return student["name"]

students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]

students.sort(key=get_name)` 
```

如果我们打印`students`，我们现在应该按照以下顺序取回它们:

```py
`[
    {'name': 'Charlie', 'grade_average': 91},
    {'name': 'Hannah', 'grade_average': 83},
    {'name': 'Lauren', 'grade_average': 92},
    {'name': 'Peter', 'grade_average': 85},
    {'name': 'Rachel', 'grade_average': 79}
]` 
```

然而，我们在这里使用的密钥非常简单。它所做的只是从字典键中获取一个值。因此，在调用`sort`时，我们使用 lambda 表达式来定义函数更合适。

```py
`students = [
    {"name": "Hannah", "grade_average": 83},
    {"name": "Charlie", "grade_average": 91},
    {"name": "Peter", "grade_average": 85},
    {"name": "Rachel", "grade_average": 79},
    {"name": "Lauren", "grade_average": 92}
]

students.sort(key=lambda student: student["name"])` 
```

这个 lambda 表达式定义了一个与我们之前定义的相同的函数。它有一个参数，它期望这个参数是一个字典，它返回与字典中的`"name"`键相关联的值。

### 2)将以下函数转换为 lambda 表达式，并将其赋给一个名为`exp`的变量。

```py
`def exponentiate(base, exponent):
    return base ** exponent` 
```

在这种情况下，我们有一个带有两个参数的函数，我们的返回值是`base ** exponent`。因此，我们的 lambda 表达式如下所示:

```py
`lambda base, exponent: base ** exponent` 
```

这两个参数跟在关键字`lambda`后面，冒号标记参数列表的结尾。在冒号后面我们写返回值。

现在我们只需要将它赋给一个名为`exp`的变量，就像这样:

```py
`exp = lambda base, exponent: base ** exponent` 
```

### 3)打印您在之前的练习中使用 lambda 表达式创建的函数。创建的函数的名称是什么？

在这种情况下，我们必须小心不要调用我们的函数。我们想要打印函数本身，这可以通过打印`exp`来实现，因为这是我们为函数赋值的地方。

```py
`exp = lambda base, exponent: base ** exponent

print(exp)` 
```

我们得到的大概是这样的:

```py
`<function <lambda> at 0x7f3e9837c1f0>` 
```

正如我们所看到的，我们用`<lambda>`代替了普通的名字。这是因为我们用 lambda 创建的函数实际上没有名字。`<lambda>`实际上只是一个占位符，因为它不是 Python 中的合法名称。

如果你记得回到第 2 天，当我们谈到变量名时，我们发现 Python 名只能以字母或下划线开头，所以我们甚至不能创建一个名为`<lambda>`的变量。

因为我们使用 lambda 表达式创建的函数没有名字，所以它们有时被称为匿名函数。正如我们所看到的，这并不妨碍我们将它们赋给变量！