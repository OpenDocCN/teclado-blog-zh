# 第 25 天:编写地道的 Python | Teclado

> 原文：<https://blog.teclado.com/python-30-day-25-idiomatic-python/>

欢迎来到 Python 系列 [30 天的第 25 天！今天我们将学习一些专业 Python 开发者使用的更常见的模式和技巧。](https://blog.teclado.com/30-days-of-python/)

这将帮助我们建立在我们已经学过的习惯用法的基础上，这样我们就可以开始编写真正专业的代码了！

## Pythonic 代码

在我们深入研究任何特定的模式之前，我只想花点时间谈谈“Pythonic 代码”。随着您继续学习 Python，您可能会经常听到这个术语。

关于“Pythonic”代码到底是什么有很多困惑，但是这个术语实际上很容易定义。当 Python 代码以采用 Python 语言的习惯用法的方式编写，并且符合特定的风格原则时，就可以说它是 Python 化的。

习语是一种语言、方言或一个民族特有的说话风格。

这里有一个与自然语言相当直观的类比。

让我们以英语为例。在英语中，我们有“再见”这个词，但是以英语为母语的人很少说“再见”。相反，我们喜欢诸如“cya”、“再见”和“回头见”之类的话。在大多数情况下，再见通常听起来有点尴尬和生硬。

我们编写 Python 代码的方式非常相似。有一些编写 Python 的方法是有意义的，并且在功能意义上是完全正确的，但是对于 Python“原生者”来说，这些方法看起来不太正确。

写 Python 代码就是写那些其他 Python 开发者认为正确的代码。

考虑到这一点，让我们学习一些能让我们看起来更像真正的毕达哥尼亚的东西。

## 作为条件的真值

这是我们在[第 5](/30-days-of-python/python-30-day-5-conditionals-booleans/) 天简要讨论过的，但我认为强化这种模式真的很值得。在接下来的部分中，我们也将基于这种模式进行构建。

快速提醒一下，在 Python 中，每个值都有一个关联的真值，当我们把它传递给`bool`函数时，这个真值决定了它的值是`True`还是`False`。

一般来说，Python 中的值计算为`True`，除了[少数例外](https://docs.python.org/3/library/stdtypes.html#truth-value-testing)。

异常的一个重要类别是空序列和集合，包括字符串、列表、`range`对象、字典等。

每个值都可以被认为是`True`或`False`这一事实给了我们很大的权力，因为我们可以使用任何值作为像`while`循环或条件语句这样的事情的条件。

因此，我们通常不需要编写这样的代码:

```py
`my_list = []

if len(my_list) == 0:
    print("The list is empty")
else:
    values = ', '.join(str(value) for value in my_list)
    print("The list contains: {values}")` 
```

相反，更 Pythonic 化的方法是测试`my_list`的真值。如果列表为空，它将计算为`False`，因此我们可以用它来控制条件语句的流程。

```py
`my_list = []

if my_list:
    values = ', '.join(str(value) for value in my_list)
    print("The list contains: {values}")
else:
    print("The list is empty")` 
```

这种方法对于检查函数或方法是否返回`None`也非常有用。

```py
`from operator import add, mul, sub, truediv

operations = {
    "add": add,
    "divide": truediv,
    "multiply": mul,
    "subtract": sub
}

selected_option = input("Select the operation to perform: ").strip().lower()

operation = operations.get(selected_option)  # returns None if the key is invalid

if operation:
    a = input("Please enter a value for the first operand: ")
    b = input("Please enter a value for the second operand: ")

    print(operation(a, b))
else:
    print("Invalid operation")` 
```

### 注意

有些情况下，我们必须显式地检查一些值，所以要明智地决定何时使用这种结构。

例如，如果`None`是我们特别寻找的值，我们应该检查值`is None`或`is not None`，而不是检查真值。这是因为`0`、`False`、`None`等很多从真值的角度来看都是一样的。

## 控制流的布尔运算符

如果您已经阅读了额外的资源，您可能已经遇到了`and`和`or`操作符，但是它们并不是我们在本系列中真正涉及的内容。

它们在 Python 中的基本用途是组合表达式以构成复合条件。

例如，我们可以这样做:

```py
`x = int(input("Please enter a number between 1 and 10: "))

if x > 0 and x < 11:
    print(x)
else:
    print("Invalid selection")` 
```

下面是用`or`写的同一个例子:

```py
`x = int(input("Please enter a number between 1 and 10: "))

if x < 1 or x > 10:
    print("Invalid selection")
else:
    print(x)` 
```

从这个意义上说，操作符相当简单。对于评估为真的`and`条件，两个操作数都必须为真。对于一个`or`条件，任何一个操作数为真就足够了。

但是，在 Python 中`and`和`or`有一个非常有趣的属性。它们不对`True`和`False`求值:而是对其中一个操作数求值。

在`and`的情况下，如果第一个操作数为 falsy，我们将取回该操作数；否则，我们得到第二个操作数。让我们考虑几个例子来看看这是如何工作的。

```py
`4 and 0           # evaluates to 0
[] and 3          # evaluates to []
True and False    # evaluates to False
True and True     # evaluates to True
4 and 5           # evaluates to 5` 
```

如果我们在类似条件语句的东西中使用`and`作为条件，这就像`and`评估为`True`或`False`一样。

在第一个操作数是 falsy 的情况下，我们已经知道表达式不可能是真的，所以我们只返回 falsy 值。然后依次评估为`False`。

如果第一个操作数是真的，我们就返回第二个操作数，而不去检查它。为什么？

因为如果它是真的，那么两个值都是真的，我们返回的真值将计算为`True`。如果第二个操作数改为 falsy，那么这两个值都不是真的，所以当 falsy 值计算为`False`时，我们得到的正是我们想要的。我们根本不需要检查第二个值。

`or`的工作方式基本相同，但是如果它是真的，它返回第一个操作数，否则它返回第二个操作数。这里有几个例子:

```py
`4 or 0            # evaluates to 4
[] or 3           # evaluates to 3
False or None     # evaluates to None
True or False     # evaluates to True
4 or 5            # evaluates to 4` 
```

让我们看看使用这个属性的一些方法。

### 使用`or`替换 falsy 值

使用`or`的一个常见方法是替换错误的值。例如，让我们考虑这样一种情况，当用户没有提供默认名称时，我们希望输入一个默认名称。

使用我们在上一节中学到的技巧，我们可能会尝试这样做:

```py
`name = input("Please enter your name: ").strip().title()

if not name:
    name = "John Doe"` 
```

然而，我们可以使用`or`操作符将其缩减为一行，如下所示:

```py
`name = input("Please enter your name: ").strip().title() or "John Doe"` 
```

这里我们说如果用户输入是真的(即不是空字符串)，就把它分配给`name`；否则，分配字符串`"John Doe"`。

### 使用`and`来改变我们处理返回值的方式

使用`and`并不常见，但是我们也可以用这个操作符做一些有趣的事情。

假设我们有这样一个函数:

```py
`def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return` 
```

这里我们有两种可能的返回值。我们要么得到一个表示除法结果的浮点数，要么得到`None`。

假设我想使用`is_integer`方法测试某个除法的结果是否是一个整数。然后我想通知用户这个数字是否是一个整数。

使用条件语句，我们必须做这样的事情:

```py
`def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return

a = 6
b = 0

result = divide(a, b)

if result:
    if result.is_integer():
        print(f"{a} / {b} produces an integer result.")` 
```

使用`and`，我们可以写成这样:

```py
`def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return

a = 6
b = 0

result = divide(a, b)

if result and result.is_integer():
    print(f"{a} / {b} produces an integer result.")` 
```

### 风格注释

虽然我认为了解这一点很重要，但我不一定建议你使用这种`and`方法，当然我也不建议你一直使用它。它的用例比`or`要有限得多。

一旦你理解了它，就相对容易理解正在发生的事情，但是对于不熟悉习语的人来说，它尤其不透明，我认为这通常比使用条件语句和`try`语句的组合更糟糕。

## 切片截断

假设我们有一个列表，我们想从列表中丢弃一些值。一个例子可能是，我们以某种方式过滤了一个列表，但是我们只想要前十个值。

使用切片的方法可能如下所示:

```py
`numbers = [1, 54, 2, -4, -65, 23, 97, 45, 14, 19, 73, -6, 31, 92, 3]

positive_numbers = sorted(number for number in numbers if number > 0)
positive_numbers = positive_numbers[:10]

print(positive_numbers)  # [1, 2, 3, 14, 19, 23, 31, 45, 54, 73]` 
```

这种方法非常冗长，而且这里有很多键入错误的机会，因为我们必须两次键入同一个变量名。如果我们有几个相似的名字，这很容易导致一个错误，我们错误地引用了两个不同的变量。

顺便说一句，这也是更喜欢这个的一个原因:

对此:

它的效率也不是很高，因为像这样分割一个列表会创建一个全新的列表。相反，更好的方法是从原始列表中删除项目。我们可以这样做:

```py
`numbers = [1, 54, 2, -4, -65, 23, 97, 45, 14, 19, 73, -6, 31, 92, 3]

positive_numbers = sorted(number for number in numbers if number > 0)
del positive_numbers[10:]

print(positive_numbers)  # [1, 2, 3, 14, 19, 23, 31, 45, 54, 73]` 
```

这不仅更短、更有效，而且可读性更好。它清楚地表明我们正在从`positive_numbers`列表中删除条目，而切片方法通过一个额外的赋值使这变得有点模糊。

当然，在某些情况下，切片是正确的解决方案。毕竟，我们并不总是想毁掉原来的收藏。

## 使用`in`检查多个值

我在审查学生代码时经常看到的一件事是这样的:

```py
`proceed = input("Would you like to continue? ").strip().lower()

if proceed == "y" or proceed == "yes" or proceed = "continue":
    ...` 
```

这是个好主意。他们试图捕捉用户可能尝试响应的许多不同方式，以便程序更适合用户。

然而，这种方法非常冗长，有一种更好的方式来编写这种代码。

```py
`proceed = input("Would you like to continue? ").strip().lower()

if proceed in ("y", "yes", "continue"):
    ...` 
```

如果你想检查多个值，使用关键字`in`会更短更干净。

## 练习

1)编写一个函数，提示用户输入他们的名字，然后问候他们。您应该通过删除任何空白并将字符串转换为标题大小写来处理字符串。

如果在处理完字符串后，剩下一个空字符串，函数应该在输出中用“World”替换这个空字符串。

2)编写一个函数来确定一个字符串是否只包含 ASCII 字母(大写或小写的 a 到 z)。

提示:您应该看看`string`模块中的常量。文档可以在这里找到[。](https://docs.python.org/3/library/string.html#string-constants)

3)使用`random`模块中的`sample`函数创建三个列表，每个列表包含从`1`到`100`的十五个数字。将这些列表按降序排序(最大的先排序)，然后截断每个列表，使每个列表中只保留 5 个项目。

您可以在这里找到我们的解决方案[。](/30-days-of-python/python-30-day-25-exercise-solutions)