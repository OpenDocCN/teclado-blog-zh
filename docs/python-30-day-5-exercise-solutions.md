# 第 5 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-5-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 5 天练习](/30-days-of-python/python-30-day-5-conditionals-booleans)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)尝试使用`==`模拟`is`操作员的行为。

为了解决这个问题，我们确实需要弄清楚这两个操作符之间的区别。`==`是*值相等*的比较运算符，而`is`是*同一性*的比较运算符。换句话说，`is`检查两个值是否完全相同。我们通过比较它们的内存地址来确定这一点。

我们可以使用`id`函数找到给定对象的内存地址。

如果我们想验证两个列表是完全相同的列表，我们可以这样做:

```py
`list_1 = [1, 2, 3, 4, 5]
list_2 = [1, 2, 3, 4, 5]

print(id(list_1) == id(list_2))  # False` 
```

以下是两个列表相同的示例:

```py
`list_1 = [1, 2, 3, 4, 5]
list_2 = list_1

print(id(list_1) == id(list_2))  # True` 
```

我们可以通过检查使用`is`时是否得到相同的结果来验证我们的答案:

```py
`list_1 = [1, 2, 3, 4, 5]
list_2 = list_1

print(list_1 is list_2)  # True` 
```

### 2)尝试使用`is`运算符或`id`函数来研究以下各项之间的差异:

```py
`numbers = [1, 2, 3, 4]
new_numbers = numbers + [5]

numbers = [1, 2, 3, 4]
numbers.append(5)` 
```

对于这个练习，我建议对第一段代码使用`is`，对第二段代码使用`id`。第二个我不会用`is`的原因是因为我们只有一个变量，所以很难做比较。

对于第一段代码，我们可以简单地检查`numbers`是否与`new_numbers`是同一个列表，如下所示:

```py
`numbers = [1, 2, 3, 4]
new_numbers = numbers + [5]

print(numbers is new_numbers)  # False` 
```

这种情况下是`False`。通过使用`+`操作符，我们创建了一个新列表，然后我们将这个新列表分配给`new_numbers`。

对于第二段代码，我们可以在`append`调用前后找到`numbers`的`id`:

```py
`numbers = [1, 2, 3, 4]
print(id(numbers))  # 140220707722752

numbers.append(5)
print(id(numbers))  # 140220707722752` 
```

我们通过人工检查可以看到，这两个数字是相同的。如果你想在这里使用`is`,我们需要引用原始的`numbers`列表，并附加一个变量名，如下所示:

```py
`numbers = [1, 2, 3, 4]
numbers_copy = numbers

numbers.append(5)

print(numbers is numbers_copy)  # True` 
```

在这种情况下，`numbers_copy`和`numbers`作为相同的列表开始。当我们修改`numbers`时，我们直接修改两个名字所指的列表。两个列表都会更新，并且不会创建新列表。通过查看上面的`id`解决方案，我们可以看到列表被直接修改。

### 3)要求用户输入一个数字。告诉用户数字是正数、负数还是零。

在这个练习中，我们需要使用一个条件语句。

我们的第一步应该是从用户那里获取数字，并使用`float`函数将其转换成浮点数。如果愿意，您可以将用户输入转换为整数，但是我们只能处理整数。

```py
`user_number = float(input("Please enter a number: "))` 
```

既然我们能够将用户的号码存储在这个变量中，我们就可以针对几种不同的情况对其进行检查。

我的条件语句有三个分支。最初，我要检查数字是否大于`0`。我觉得这是最有可能的结果，所以把这个放在第一位是有道理的。接下来，我将使用一个`elif`子句来检查数字是否小于`0`。最后，我将使用一个`else`子句，只有当一个数字既不大于`0`，也不小于`0`时才会触发。换句话说，数字正好是`0`。

```py
`user_number = float(input("Please enter a number: "))

if user_number > 0:
    print(f"{user_number} is positive!")
elif user_number < 0:
    print(f"{user_number} is negative!")
else:
    print(f"You entered 0!")` 
```

虽然这里可以使用三个独立的条件语句，比如:

```py
`user_number = float(input("Please enter a number: "))

if user_number > 0:
    print(f"{user_number} is positive!")
if user_number < 0:
    print(f"{user_number} is negative!")
if user_number == 0:
    print(f"You entered 0!")` 
```

如果我们有关联条件，我们应该避免这种情况。如果我们有一个带有几个分支的条件语句，Python 只检查分支，直到它找到一个真条件，这意味着我们可能会省去许多不必要的检查。

### 4)编写一个程序来确定员工是否应该加班。您应该询问用户该员工本周工作了多少小时，以及该员工的时薪。

在本练习中，我们需要确定该员工本周的工作时间是否超过 40 小时。如果他们这样做了，我们需要为他们超过 40 小时的工作时间支付 110%的时薪。

因为我们在这里要测试一个条件，所以条件语句是个好主意。

不过，我们的第一步是从用户那里获取价值。我将在这里获取员工的姓名，因为我想在输出中使用它。如果你愿意，可以忽略这个。

对于工作时间和小时工资，我将把用户输入转换成浮点数。这是因为我们可以合理地预计每小时工资大约为 16.50 英镑，人们的工作时间不到一小时。

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))` 
```

不要忘记在你的提示后面加空格！

现在我们有了需要的信息，我们可以开始研究逻辑了。首先，我将确定该用户的工作时间是否超过 40 小时，如下所示:

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))

if hours_worked > 40:
    print(f"{employee_name} worked more than 40 hours.")
else:
    print(f"{employee_name} worked less than 40 hours.")` 
```

如果这一阶段一切正常，我们就可以开始计算欠这个员工的工资了。让我们从他们工作少于 40 小时的情况开始，因为这是更简单的情况。

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))

if hours_worked > 40:
    print(f"{employee_name} worked more than 40 hours.")
else:
    owed_pay = hours_worked * hourly_wage
    print(f"{employee_name} is owed ${owed_pay}.")` 
```

在这个阶段，我们有一个小问题。考虑以下用户交互:

```py
`Enter the employee's name: John Smith
How many hours did John Smith work this week? 31.75
What is John Smith's hourly wage? 14.56
John Smith is owed $462.28000000000003.` 
```

这里的工资产出会变得很难看。有时我们有很多尾随零，但通常只有一个。如果我们要么没有十进制数，要么将输出限制在两位小数，那就太好了。

你可能也注意到了这里的结果有一点不准确。这是因为浮点数不能有限地表示所有的十进制数，所以对于一些十进制数，我们实际上得到的是接近的近似值，而不是精确的值。如果你有兴趣了解更多这方面的内容，你可以阅读这篇博文的第一部分。

我们有两个主要的选择来纠正这个问题。首先，我们可以通过将结果传递给`int`来丢弃数字的小数内容。这将为我们提供一个大概的数字，但会影响我们的准确性:

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))

if hours_worked > 40:
    print(f"{employee_name} worked more than 40 hours.")
else:
    owed_pay = int(hours_worked * hourly_wage)
    print(f"{employee_name} is owed ${owed_pay}.")` 
```

另一种方法是使用一些特殊的字符串格式语法。这确实是更高级的东西，但是我们可以像这样限制浮点数的小数位数:

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))

if hours_worked > 40:
    print(f"{employee_name} worked more than 40 hours.")
else:
    owed_pay = hours_worked * hourly_wage
    print(f"{employee_name} is owed ${owed_pay:.2f}.")` 
```

我们有另一篇博文更详细地介绍了这种语法。对于这个解决方案的其余部分，我将使用更简单的`int`方法。

既然我们已经解决了这个问题，我们可以开始计算加班时间了。我将分两个阶段来做这件事:

1.  我要计算`40`乘以员工的时薪。
2.  我将从工作时间中减去`40`，然后将结果乘以小时工资的 110%。

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))

if hours_worked > 40:
    regular_pay = 40 * hourly_wage
    overtime_pay = (hours_worked - 40) * hourly_wage * 1.1
    owed_pay = int(regular_pay + overtime_pay)

    print(f"{employee_name} is owed ${owed_pay}.")
else:
    owed_pay = int(hours_worked * hourly_wage)
    print(f"{employee_name} is owed ${owed_pay}.")` 
```

因为我们实际上在两个分支中都有相同的`print`调用，所以我们可以通过将`print`调用移出条件语句来稍微改进一下，如下所示:

```py
`employee_name = input("Enter the employee's name: ")
hours_worked = float(input(f"How many hours did {employee_name} work this week? "))
hourly_wage = float(input(f"What is {employee_name}'s hourly wage? "))

if hours_worked > 40:
    regular_pay = 40 * hourly_wage
    overtime_pay = (hours_worked - 40) * hourly_wage * 1.1
    owed_pay = int(regular_pay + overtime_pay)
else:
    owed_pay = int(hours_worked * hourly_wage)

print(f"{employee_name} is owed ${owed_pay}.")` 
```

就这样，我们结束了！