# 第 22 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-22-exercise-solutions/>

以下是我们对 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 22 天](/30-days-of-python/python-30-day-22-iterators)练习的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)下面你会发现一个包含几个充满数字的元组的列表。使用`map`函数计算每个元组中数字的总和。使用手动迭代打印由结果`map`对象提供的前两个结果。

```
`numbers = [(23, 3, 56), (98, 1034, 54), (254, 344, 5), (45, 2), (122, 63, 74)]` 
```

首先，让我们创建我们的`map`对象。在这种情况下，我们不需要任何复杂的东西，比如 lambda 表达式或来自`operator`模块的东西:我们可以只使用`sum`函数。

```
`numbers = [(23, 3, 56), (98, 1034, 54), (254, 344, 5), (45, 2), (122, 63, 74)]
totals = map(sum, numbers)` 
```

现在我们有了一个分配给`totals`的`map`对象，我们只需要将这个`map`对象传递给`next`就可以从中获得一个项目。这是可能的，因为`map`对象是迭代器。

```
`numbers = [(23, 3, 56), (98, 1034, 54), (254, 344, 5), (45, 2), (122, 63, 74)]
totals = map(sum, numbers)

print(next(totals))  # 82
print(next(totals))  # 1186` 
```

请注意，在这一点上还没有计算最后三个总和。

### 2)编写一个程序来创建一个时间表，列出在 30 天内的某一天哪些员工将锁上商店。您应该列出日期、员工姓名和星期几。

在这个练习中，我们被告知有三名员工每天晚上轮流锁门。例如，如果我们从员工 A 开始，他们将在第一天关闭，然后是员工 B，接着是员工 c。然后，循环从员工 A 的第 4 天开始。

为了创建这种循环行为，我们将在`itertools`模块中使用一个非常有用的函数，名为`cycle`。`cycle`是一种特殊类型的迭代器，它将按照某种预定义的顺序为我们提供无限多的结果。

如果我们看一下文档，我们可以看到函数签名是这样的:

```
`itertools.cycle(iterable)` 
```

这告诉我们,`cycle`使用一个 iterable 来定义应该循环的值。

记住这一点，让我们导入模块并创建两个`cycle`迭代器:一个用于雇员，一个用于一周中的日子。

```
`import itertools

employees = itertools.cycle(["Peter", "Fiona", "Carl"])
days = itertools.cycle(["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"])` 
```

现在我们有了这个，剩下的解决方案就很简单了。我们只需要创建一个有 30 次迭代的`for`循环，这可以通过迭代一个`range`来完成。然后我们可以使用这个`range`中的值作为天数，我们可以使用`next`从两个周期中获取值。

```
`import itertools

employees = itertools.cycle(["Peter", "Fiona", "Carl"])
days = itertools.cycle(["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"])

for day_number in range(1, 31):
    print(f"Day {day_number} ({next(days)}): {next(employees)} closes.")` 
```

这种解决方案的一个非常好的地方是，我们可以通过修改传递给 employees `cycle`的列表来添加或删除员工。我们不需要做任何其他的改变。

```
`import itertools

employees = itertools.cycle(["Peter", "Fiona", "Carl", "Helen"])
days = itertools.cycle(["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"])

for day_number in range(1, 31):
    print(f"Day {day_number} ({next(days)}): {next(employees)} closes.")` 
```

我认为，考虑如何在不使用任何迭代器的情况下做到这一点也是一个很好的练习。