# Python 的命名元组

> 原文：<https://blog.teclado.com/pythons-namedtuples/>

嗨，欢迎来到这个关于命名夫妇的简短帖子！namedtuples 是奇妙的 [collections](https://docs.python.org/3.7/library/collections.html) 模块的一部分，它们允许我们使用名称来访问元组元素，而不是索引，这使得我们的代码更具可读性。我个人认为他们绝对是惊人的，所以让我们看看他们的行动！

## 定义一个`namedtuple`

为了使用一个`namedtuple`，我们需要为它定义一个蓝图。例如，如果我们想要一个名为`Student`的`namedtuple`，其中的元素是学生的姓名、年龄和主要教师，它可能看起来像这样:

```py
from collections import namedtuple

Student = namedtuple("Student", ["name", "age", "faculty"]) 
```

我们来分析一下。

首先需要注意的是，我们在这里做一个赋值，命名惯例是使用大写字母作为变量名，就像我们在定义一个类一样。

在赋值的右边，我们有`namedtuple`和一组括号，在里面，我们有`Student` `namedtuple`的所有配置。

括号内的第一项是我们要赋值的变量的名称，第二项是`Student`中元素的键列表。

## 创建一个`namedtuple`的实例

既然我们已经定义了蓝图，我们可以在代码中使用`Student`。例如，我们可能会这样做:

```py
from collections import namedtuple

Student = namedtuple("Student", ["name", "age", "faculty"])

names = ["John", "Steve", "Mary"]
ages = [19, 20, 18]
faculties = ["Politics", "Economics", "Engineering"]

students = [
    Student(*student_details)
    for student_details in zip(names, ages, faculties)
] 
```

这里我们从三个不同的列表中提取了一些相关的数据，并且我们使用了一个[列表理解](https://blog.teclado.com/python-list-comprehensions/)和 [zip](https://blog.teclado.com/python-zip/) 来创建一个`Student`对象的列表。

在上面的例子中，我们使用了`*`语法来析构`zip`对象中的每个元组，但是我们有一些其他的选择。例如，我们可以使用关键字参数来定义一个`namedtuple`的值:

```py
example_student = Student(name="James", age=18, faculty="Music") 
```

## 访问`namedtuple`中的元素

回到我们前面的学生列表的例子，我们可能希望以某种方式处理这个列表，以确定我们的数据。例如，我们可能想使用`max`来查找列表中年龄最大的学生。

我们可以这样做:

```py
oldest_student = max(students, key=lambda student: student.age) 
```

这里我们可以看到第一个访问命名元组中的值的例子。我们需要使用这个`.`语法通过键访问值，将键的名称放在点之后。

在上面的代码中，我们为`max`设置了一个自定义键，以便它根据每个元组中的`age`元素来比较我们的元组。你可以在[文档](https://docs.python.org/3.7/library/functions.html?highlight=built#max)中了解更多。

使用常规元组或列表，可能看起来更像这样:

```py
oldest_student = max(students, key=lambda student: student[1]) 
```

既然我们不得不使用索引，lambda 函数的含义就不那么清楚了。我们必须回过头来参考原始数据，看看索引`1`处到底是什么。

如果出于某种原因，你想使用一个带有`namedtuple`的索引，这仍然有效，所以你可以这样做。

为了结束上面的例子，让我们打印出`oldest_student`:

```py
Student(name='Steve', age=20, faculty='Economics') 
```

输出实际上非常易读，但是为了让用户看起来不那么像代码，我们可以使用 f 字符串:

```py
print(f"{oldest_student.name} ({oldest_student.age}) - {oldest_student.faculty}")

# Steve (20) - Economics 
```

如你所见，命名元组使得我们访问的值非常清楚，使得我们的代码可读性很强。每个人都应该更经常地使用它们！

## 包扎

这就是这篇关于命名夫妇的短文。我觉得取名组合很棒，希望现在你也是。我强烈建议将它们添加到您经常使用的类型中，因为它们对于提高代码可读性有很大的潜力。

如果你喜欢这篇文章，你可能想看看我们的[完整的 Python 课程](https://www.udemy.com/course/the-complete-python-course/)，或者加入我们的[不和谐](https://discord.gg/BBWwyMq)！我们随时准备帮助您将 Python 推向新的高度！

请确保您也注册了下面的邮件列表，因为我们会为我们的订户发布折扣代码，确保您总能在我们的课程中获得最优惠的价格。

编码快乐！