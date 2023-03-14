# 第 11 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-11-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 11 天练习](/30-days-of-python/python-30-day-11-sets)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)创建一个空集，并将其赋给一个变量。

请记住，我们不能使用`{}`创建空集，因为 Python 将此视为空字典。

而是要写`set()`。

现在我们姑且称我们的布景为`s`:

### 2)使用几个`add`调用或一个`update`调用向您的空集合添加三个项目。

我只是要给我们的集合添加数字。使用`add`方法，我们可以这样做。

```py
`s = set()

s.add(1)
s.add(2)
s.add(3)` 
```

或者我们可以使用一个循环从一个`range`对象中获取项目:

```py
`s = set()

for number in range(1, 4):
    s.add(number)` 
```

我们也可以像这样使用`update`方法:

```py
`s = set()
s.update(range(1, 4))` 
```

这些基本都是等价的。

### 3)创建第二集合，该第二集合包括至少一个与第一集合共有的元素。

对于第二个集合，我将包括几种不同类型的值:

```py
`random_values = {"r", 1, ("Python", "C", "Rust")}` 
```

### 4)求两个集合的并、对称差、交。打印每个操作的结果。

在这个练习中，我将直接打印操作的结果，因为每个方法调用都会给我们一个新的集合。

```py
`s = set()
s.update(range(1, 4))

random_values = {"r", 1, ("Python", "C", "Rust")}

print(s.union(random_values))
print(s.symmetric_difference(random_values))
print(s.intersection(random_values))` 
```

在我的例子中，输出是:

```py
`{1, 2, 3, ('Python', 'C', 'Rust'), 'r'}
{2, 3, 'r', ('Python', 'C', 'Rust')}
{1}` 
```

### 5)使用`range`创建一个数字序列，然后要求用户输入一个数字。通知用户他们的号码是否在您指定的范围内。

在这种情况下，我将使用`range(27, 54)`，它包括从`27`到`53`的所有整数。具体的`range`没那么重要。

现在我们需要让用户输入一个数字。记住这个数字将以字符串形式返回，所以我们需要将它转换成整数。

```py
`numbers = range(27, 54)
user_number = int(input("Enter a number: "))` 
```

现在我们只需要使用`in`关键字来验证这个数字是否在这个`range`中:

```py
`numbers = range(27, 54)
user_number = int(input("Enter a number: "))

if user_number in numbers:
    print("Your number is in the valid range.")
else:
    print("Your number is outside of the valid range.")` 
```

扩展任务告诉用户他们的猜测是太高还是太低。

如果用户的号码不在指定的号码序列内，我们可以对照`range`中的第一个值来检查用户的号码。如果低于这个数字，用户的价值就太低了。否则它一定超过了`range`中的最高值。

```py
`numbers = range(27, 54)
user_number = int(input("Enter a number: "))

if user_number in numbers:
    print("Your number is in the valid range.")
else:
    if user_number < numbers[0]:
        print("Your number is too low.")
    else:
        print("Your number is too high.")` 
```