# Python 中的十进制与浮点

> 原文：<https://blog.teclado.com/decimal-vs-float-in-python/>

在 Python 中处理十进制数时，我们通常会求助于浮点数。对于大多数目的来说，比如简单的划分，Floats 为我们提供了很好的服务，但是它们也有一些限制，对于某些用例来说，这些限制会成为难以置信的问题。他们只是不够精确。然而，浮点并不是我们唯一的选择，今天我们将看看`decimal`模块，以及为什么你可能会使用它。

## 快速浏览二进制

在我们能够正确理解浮点数的问题之前，我们需要快速回顾一下数字是如何用二进制表示的。

我们在日常生活中使用的数字系统是以 10 为基数的数字系统，也称为十进制数字系统。我们使用十个不同组合的独特数字来代表所有数字。另一方面，二进制是以 2 为基数的数字系统，只使用两个唯一的数字:通常是 0 和 1。当数字存储在我们的计算机中时，它们是以二进制格式存储的。

二进制数可能看起来像这样:10101101，恰好是 173。

那么我们如何从 10101101 中得到 173 呢？

二进制是 2 的幂，所以 10101101 中最右边的 1 代表 1×2⁰.然后我们向左移动一步，在这里我们找到一个 0。这个 0 代表 0 x 2，也就是 0 x 2。再向左移动一步，我们找到另一个 1，这次代表 1 x 2，也就是 4。每向左移动一步，功率增加 1。

总的来说，我们有这样的东西:

(1 × 2⁷) + (0 × 2⁶) + (1 × 2⁵) + (0 × 2⁴) + (1 × 2³) + (1 × 2²) + (0 × 2¹) + (1 × 2⁰)

那就是:

(1 x 128)+(0 x 64)+(1 x 32)+(0 x 16)+(1 x 8)+(1 x 4)+(0 x 2)+(1 x 1)

如果我们把这些都加起来，我们会得到 173。如你所见，数字的二进制表示比十进制表示要长得多，但是我们最终可以用这种方式表示任何整数。

### 二进制分数

我们已经快速复习了如何用二进制表示整数，但是分数呢？事实证明，它的工作方式是一样的，只是有负的力量。

例如，2⁻是，2⁻是，这意味着我们现在可以表示 0.75、0.5 和 0.25。使用逐渐增大的负幂，我们可以表示所有形式的十进制数。

然而，正如有些数字我们不能用有限的十进制数来表示(例如⅓)，也有一些数字我们不能用二进制来表示。例如，数字 0.1 没有有限的二进制表示。

## Python 中的浮点数

那么当我们用 Python 写`0.1`时会发生什么呢？让我们来看看:

```
print(f"{0.1:.20f}")  # 0.10000000000000000555 
```

对于那些不熟悉上面语法的人来说，`:.20f`是一种告诉 Python 我们想要这个浮点数小数点后 20 位的方式。我们有一个帖子，你可以在下面看看:

[https://blog . tecladocode . com/python-formatting-numbers-for-printing/](https://blog.teclado.com/python-formatting-numbers-for-printing/)

正如我们所见，我们实际上并没有得到`0.1`:我们得到的是`0.1`的近似值。不幸的是，有时一个接近的近似值还不够好。

这在进行比较时尤其普遍:

```
a = 10
b = a / 77
c = b * 77

if a != c:
    print("Things got weird...")

# Things got weird... 
```

如果我们打印`a`和`c`，我们可以看到发生了什么:

```
print(f"{a:.20f}")  # 10.00000000000000000000
print(f"{c:.20f}")  #  9.99999999999999822364 
```

这种近似误差会在一系列运算中变得非常复杂，这意味着我们实际上可能会在应该相同的数字之间产生非常显著的差异。

你可以在 Python 文档中读到更多关于这些问题的内容:[https://docs.python.org/3/tutorial/floatingpoint.html](https://docs.python.org/3/tutorial/floatingpoint.html)

## 输入`decimal`

正如本文开头提到的，Python 确实有另一种处理十进制数的方法，那就是`decimal`模块。与浮点不同，`decimal`模块中定义的`Decimal`对象不容易丢失精度，因为它们不依赖于二进制分数。

## 创建`Decimal`对象

在我们深入研究`Decimal`对象之前，让我们看看如何定义它们。实际上有几种方法可以做到这一点。

### 使用整数

创建一个`Decimal`对象的第一种方法是使用一个整数。在这种情况下，我们简单地将整数作为参数传递给`Decimal`构造函数:

```
import decimal

x = decimal.Decimal(34)  # 34 
```

我们现在可以像使用任何其他数字一样使用`x`，并且在执行数学运算时，我们将获得一个`Decimal`对象:

```
x = decimal.Decimal(34)

print(x + 7)         # 41
print(type(x + 7))   # <class 'decimal.Decimal'>

print(x // 7)        # 4
print(type(x // 7))  # <class 'decimal.Decimal'> 
```

### 使用字符串

可能有点令人惊讶的是，用分数成分制作`Decimal`对象的最简单方法之一是使用字符串。我们只需要将数字的字符串表示传递给`Decimal`，剩下的事情就交给它了:

```
x = decimal.Decimal("0.1")

print(x)            # 0.1
print(f"{x:.20f}")  # 0.10000000000000000000 
```

正如我们所看到的，在这里将`x`打印到 20 个小数位得到了 19 个零:我们不会像使用 float 时那样，在末尾随机出现一些 5。

如果您需要一个数字的精确十进制表示，使用字符串创建您的`Decimal`对象是一个非常简单的方法。

### 使用浮动

也可以从一个 float 创建一个`Decimal`对象，但是我一般不建议这样做。下面的例子应该清楚地说明了原因:

```
x = decimal.Decimal(0.1)
print(x)  # 0.1000000000000000055511151231257827021181583404541015625 
```

从 float 直接转换会导致我们的`Decimal`对象继承我们最初试图避免的所有不精确性。可能有些情况下，出于某种原因，您希望保留这种不精确性，但大多数时候，情况并非如此。

### 使用元组

也许创建`Decimal`对象最复杂的方法是使用元组，这种方法让我们对`Decimal`对象如何工作有了一些了解。

我们提供给`Decimal`的元组有三个部分。元组中的第一个元素要么是数字`0`要么是`1`，这代表数字的符号(无论它是正数还是负数)。第一个位置的 0 表示正数，而 1 表示负数。

元组中的第二项是另一个元组，它包含结果数中的所有数字。例如，数字`1.3042`有数字`(1, 3, 0, 4, 2)`。

元组中的第三个也是最后一个项目是指数。这告诉`Decimal`对象我们需要将小数点周围的数字移动多少位。`-3`的指数将导致数字向右移动 3 个空格，得到`13.042`，而`2`的指数将得到`1304200`。

数字`-13.042`的完整示例如下:

```
x = decimal.Decimal((1, (1, 3, 0, 4, 2), -3))
print(x)  # -13.042 
```

这里要注意的一件非常重要的事情是，我们确实需要外层元组周围的括号，因为`Decimal`期望这个元组作为单个参数。移除外部括号将导致错误。

虽然相对复杂，但是元组语法最适合以编程方式创建`Decimal`对象。

## `Context`物体

关于`decimal`模块的一个很酷的事情是它允许我们以各种方式定义`Decimal`对象的行为。我们可以指定精确度、指数极限，甚至舍入规则。

为了做到这一点，我们通常会使用`getcontext`函数，它允许我们查看和修改当前线程的`Context`对象。

让我们来看看默认的`Context`对象是什么样子的:

```
import decimal

print(decimal.getcontext())
# Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=-999999, Emax=999999, capitals=1, clamp=0, flags=[], traps=[InvalidOperation, DivisionByZero, Overflow]) 
```

如您所见，我们可以设置各种属性来更改`Decimal`对象的工作方式。

例如，我们可能决定需要 10 个有效数字的精度，我们可以这样设置:

```
import decimal

decimal.getcontext().prec = 10
x = decimal.Decimal(1) / decimal.Decimal(7)

print(x)  # 0.1428571429 
```

请注意，这个新的精度仅在数学运算期间相关。我们可以用任意精度定义一个`Decimal`对象，即使我们使用`getcontext`设置了较低的精度。

```
import decimal

decimal.getcontext().prec = 10

x = decimal.Decimal("1.045753453457657657")
y = decimal.Decimal(7)

print(x)      # 1.045753453457657657
print(y)      # 7
print(x / y)  # 0.1493933505 
```

正如我前面提到的，我们还可以定义如何对`Decimal`对象进行舍入，这非常有用。默认情况下，Python 使用银行家舍入法，这与我们在学校学到的舍入法有些不同。

如果你不熟悉的话，我们在这里讨论银行家的四舍五入:[https://blog.tecladocode.com/rounding-in-python/](https://blog.teclado.com/rounding-in-python/)

使用`getcontext`,我们可能会改变 Python 的舍入行为，使以`5`结尾的小数总是远离`0`,这就是我们在日常生活中的舍入方式:

```
import decimal

decimal.getcontext().prec = 1

x = decimal.Decimal(5)
y = decimal.Decimal(2)

print(x / y)  # 2

decimal.getcontext().rounding = decimal.ROUND_HALF_UP

print(x / y)  # 3 
```

使用常量在`decimal`模块中定义了许多不同的舍入行为。您可以在这里了解更多可用选项:[https://docs . python . org/3.7/library/decimal . html # module-decimal](https://docs.python.org/3.7/library/decimal.html#module-decimal)

## 一些有用的`decimal`方法

`decimal`模块提供了许多处理`Decimal`对象的简便方法。这里有一些你可能想看看。

### `sqrt`

`sqrt`方法允许我们以指定的精度获得任意`Decimal`的平方根。

```
import decimal

decimal.getcontext().prec = 15
x = decimal.Decimal(2).sqrt()

print(x)  # 1.41421356237310 
```

### `quantize`

`quantize`方法用于将`Decimal`对象更改为新的指数。例如，假设我们有一个数字`1.41421356`，我们想改变这个数字来匹配模式`1.000`，我们可以写如下:

```
import decimal

x = decimal.Decimal("1.41421356")
y = x.quantize(decimal.Decimal("1.000"))

print(y)  # 1.414 
```

使用`quantize`方法需要注意的一点是，如果最终值超过了定义的精度级别，它将引发一个`InvalidOperation`异常。

```
import decimal

decimal.getcontext().prec = 1

x = decimal.Decimal("1.41421356")
y = x.quantize(decimal.Decimal("1.000"))

# decimal.InvalidOperation: [<class 'decimal.InvalidOperation'>] 
```

### `as_tuple`

`as_tuple`方法给了我们一个`Decimal`对象的元组表示，就像我们使用元组语法创建一个对象一样。

```
import decimal

x = decimal.Decimal("1.41421356")
print(x.as_tuple())

# DecimalTuple(sign=0, digits=(1, 4, 1, 4, 2, 1, 3, 5, 6), exponent=-8) 
```

对于`Decimal`对象，还有许多其他方法可用，我建议看看文档的这一部分以了解更多:[https://docs . python . org/3.7/library/decimal . html # decimal-objects](https://docs.python.org/3.7/library/decimal.html#decimal-objects)

## 那么，我们是不是应该直接用`decimal`？

不一定。虽然`decimal`模块确实非常简洁，并且它具有绝对精度的优势，但是这种精度是有代价的。在这种情况下，成本是双重的。

首先，使用`decimal`模块比使用 floats 要困难得多。当我们看上面的`DecimalTuple`对象时，这一点就很明显了，而且在此之上我们还需要考虑上下文之类的东西。你只需要知道更多就可以使用这个模块。

然而，第二个问题可能更紧迫，这取决于性能。`decimal`操作可能比使用浮点的操作慢 3 倍左右，所以如果你正在处理应用程序中对性能敏感的部分，绝对精度并不重要，你可能想要避开`decimal`。如果您仍然在使用 Python 2，这一点尤其正确，因为操作可能会慢几百倍！

模块是我们工具箱中的一个很好的工具，但是我们应该始终意识到我们是否真的需要它来用于特定的应用。

## 包扎

这就是这篇关于浮点数和小数的文章。肯定还有很多东西需要学习，所以再一次看看文档吧！模块文档可能有点吓人，但希望我们在这里讨论的内容能让你做好充分的准备，一头扎进去。

如果你喜欢我们的帖子，并且想让你的 Python 更上一层楼，一定要看看我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)。