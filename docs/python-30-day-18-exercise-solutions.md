# 第 18 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-18-exercise-solutions/>

以下是我们对 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[天 18 练习](/30-days-of-python/python-30-day-18-imports)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)导入`fractions`模块，并从浮点型`2.25`创建一个`Fraction`。

首先，我们需要导入`fractions`模块。在这种情况下，我将使用常规导入:

如果我们看一下`fractions`模块的[文档](https://docs.python.org/3/library/fractions.html#fractions.Fraction)，我们可以看到我们可以用许多不同的方式创建一个`Fraction`。

在相关文档的顶部，我们有这个签名，它描述了我们可以创建一个`Fraction`的各种方法。再往下一点也有很多例子。

签名看起来像这样:

```py
`fractions.Fraction(numerator=0,  denominator=1)
fractions.Fraction(other_fraction)
fractions.Fraction(float)
fractions.Fraction(decimal)
fractions.Fraction(string)` 
```

正如我们所见，其中一个选项是只传入一个浮点值，其余的由`Fraction`处理。

```py
`import fractions

fraction = fractions.Fraction(2.25)` 
```

如果我们打印`fraction`,我们会得到这样的结果:

### 2)仅从`math`模块中导入`fsum`函数，并使用它来计算以下一系列浮点数的总和。

```py
`numbers = [1.43, 1.1, 5.32, 87.032, 0.2, 23.4]` 
```

在这个练习中，我们需要做一个特定的导入，所以我们将使用`from ... import ...`语法。在这个实例中，我们想要从`math`模块导入，我们想要导入的是`fsum`。

现在我们有了`fsum`函数，我们可以把我们的数字列表传递给它，因为`fsum`和`sum`一样，接受一个可迭代的。

```py
`from math import fsum

numbers = [1.43, 1.1, 5.32, 87.032, 0.2, 23.4]
fsum(numbers)` 
```

如果你想确认一切正常，你可以打印结果。如果我的数学没错，答案应该是`118.482`。

### 3)使用别名导入`random`模块，并使用`randint`函数在`1`和`100`之间找到一个随机数。

同样，我们可以在[文档](https://docs.python.org/3/library/random.html#random.randint)中找到我们需要的所有信息。从函数签名中我们可以看到，`randint`接受两个参数，`a`和`b`，这两个参数决定了它可以选择的数字范围。

我们需要注意的一件非常重要的事情是:

> 返回一个随机整数 N，使得 a <= N <= b。

注意`N`可以等于`b`，这意味着停止值是包含的，不像`range`。

现在我们知道了如何使用`randint`，我们需要使用别名导入`random`。在这种情况下，我将使用`rand`。

现在我们可以用`rand.randint()`调用`randint`。

```py
`import random as rand

print(rand.randint(1, 100))` 
```

### 4)使用上面练习中的`randint`函数创建我们在第 8 天制作的猜谜游戏的新版本。这一次程序应该生成一个随机数，你应该告诉用户他们的猜测是太高还是太低，直到他们得到正确的数字。

我要稍微作弊一下，抄一下我从第[天第 8](/30-days-of-python/python-30-day-8-while-loops/) 写的解决方案。

```py
`target_number = 47

guess = int(input("Enter a number: "))

while guess != target_number
    print("Wrong!")
    guess = int(input("Enter a number: "))

print("You guessed correctly!")` 
```

这里我们需要做的唯一真正的改变是将`target_number`从静态整数改为由`randint`生成的动态值。

这是一个简单的修改，使游戏更加有趣！

```py
`import random as rand

target_number = rand.randint(1, 100)

guess = int(input("Enter a number: "))

while guess != target_number
    print("Wrong!")
    guess = int(input("Enter a number: "))

print("You guessed correctly!")` 
```