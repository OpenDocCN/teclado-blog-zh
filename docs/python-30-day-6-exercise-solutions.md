# 第 6 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-6-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 6 天练习](/30-days-of-python/python-30-day-6-for-loops)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)下面我们提供了一个元组列表，其中每个元组包含一个商店雇员的详细信息。以一种清晰易读的格式打印出每个员工在周末应得的工资。

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]` 
```

首先，我们试图对集合中的每一项执行一些操作，所以我们知道我们需要一个 for 循环。在本例中，我们试图为`employees`中的每一项打印一些内容。

在这种情况下，我的 for 循环将如下所示:

```py
`for employee in employees:` 
```

这里需要记住的一件重要事情是，Python 不会神奇地知道我们所使用的术语的含义。名称`employee`实际上只是一个常规的变量名，我们将用它来表示从`employees`列表中得到的每一项。

如果我们愿意，我们可以称它为`e`，或者`worker`:这只是一个名字。重要的是我们选择一些描述性的东西来使我们的代码易于理解。

现在我们已经建立了 for 循环的框架，我们需要在循环体中放一些东西。这是我们放置想要执行的动作的地方。

我想为每个员工做的第一件事是找出他们的总工资。这将是他们工作的小时数，乘以他们的时薪。这些是每个元组中的第二和第三项，我们将通过索引来访问这些值。

```py
`for employee in employees:
    total_pay = employee[1] * employee[2]` 
```

最后，我们需要打印信息。我将使用 f 弦，但如果您喜欢，也可以使用不同的方法。

```py
`for employee in employees:
    total_pay = employee[1] * employee[2]
    print(f"{employee[0]} has to be paid £{total_pay}")` 
```

如果我们想让它看起来更好一点，我们可以利用 Python 中一些额外的格式化选项。要将我们的`total_pay`值打印到两位小数，我们可以这样做:

```py
`for employee in employees:
    total_pay = employee[1] * employee[2]
    print(f"{employee[0]} has to be paid £{total_pay:.2f}")` 
```

我们会在我们的博客上更详细地讨论这个问题[。](https://blog.teclado.com/python-formatting-numbers-for-printing/)

### 2)对于上述员工，打印出时薪高于平均水平的员工。

这是一个我们必须分两步解决的练习。首先，我们需要实际计算平均工资，然后我们必须将每个员工的工资与我们计算的平均工资进行比较。

先说拿平均工资。为了计算平均值，我们需要两样东西。我们需要所有工资的总和，我们需要知道我们有多少员工。

工资是每个元组中的第三个项目，因此我们需要循环遍历`employees`列表，并以某种方式将每个元组的索引`2`处的项目加在一起。

我打算这样做:

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]

total = 0

for employee in employees:
    total = total + employee[2]` 
```

这里我创建了一个名为`total`的变量，它从`0`开始，然后我们每次将员工的工资加到这个总数中。到我们完事的时候，`total`包含了所有工资的总和。

接下来，我们需要知道我们有多少员工。我们可以为此使用另一个计数器。

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]

total = 0
count = 0

for employee in employees:
    total = total + employee[2]
    count = count + 1` 
```

这次计数器从`0`开始计数，我们在循环中每遇到一个新员工就加`1`。这意味着当我们完成时，counter 包含了我们列表中的雇员数量。

我们现在可以这样计算平均工资:

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]

total = 0
count = 0

for employee in employees:
    total = total + employee[2]
    count = count + 1

average_wage = total / count` 
```

现在我们需要将每个员工的工资与我们计算的平均工资进行比较。我们可以为此使用另一个循环，在循环中我们可以放一个条件语句来比较雇员的工资和平均工资。

如果员工的工资超过平均水平，我们可以打印出一条消息来表明这一点:

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]

total = 0
count = 0

for employee in employees:
    total = total + employee[2]
    count = count + 1

average_wage = total / count

for employee in employees:
    if employee[2] > average_wage:
        print(f"{employee[0]} earns more than average.")` 
```

我们可以做几件事情来使它变得更整洁。首先，我们可以使用`+=`操作符，就像这样:

这是这样写的简写:

```py
`total = total + employee[2]` 
```

我们可以在解决方案的几个地方使用它。

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]

total = 0
count = 0

for employee in employees:
    total += employee[2]
    count += 1

average_wage = total / count

for employee in employees:
    if employee[2] > average_wage:
        print(f"{employee[0]} earns more than average.")` 
```

其次，还有另一种方法可以找到集合中的项目数量。我们可以使用`len`函数。当我们将一个集合传递给`len`时，它会告诉我们里面有多少元素。

这意味着我们不再需要我们的`count`变量。

```py
`employees = [
    ("Rolf Smith", 35, 8.75),
    ("Anne Pun", 30, 12.50),
    ("Charlie Lee", 50, 15.50),
    ("Bob Smith", 20, 7.00)
]

total = 0

for employee in employees:
    total += employee[2]

average_wage = total / len(employees)

for employee in employees:
    if employee[2] > average_wage:
        print(f"{employee[0]} earns more than average.")` 
```

我们明天会更详细地讨论`len`。