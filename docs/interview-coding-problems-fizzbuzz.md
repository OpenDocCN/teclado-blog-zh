# 编码面试问题:FizzBuzz

> 原文：<https://blog.teclado.com/interview-coding-problems-fizzbuzz/>

FizzBuzz 是一种儿童计数游戏，常用于教除法。这也是一个非常常见的编码面试问题，因为这是对几种基本编程模式的一个很好的测试。如果你第一次参加 Python 面试，理解如何解决这样的问题非常重要。

## 游戏规则

一般来说，FizzBuzz 是一组人一起玩的，玩家轮流说出一个序列中的下一个数字，一次数一个。如果这个数能被三整除，玩家必须说“嘶嘶”，而不是这个数本身；如果数字能被 5 整除，玩家必须说“嗡嗡”；如果这个数字能被三*和五*整除，玩家必须说，“嘶嘶嗡”。

因此，前 10 次迭代如下所示:

```
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz 
```

通常答错会导致玩家被淘汰，游戏继续进行，直到只剩下一个人。

## 将游戏编入代码

在编写采访代码时，一个常见的请求可能是打印 FizzBuzz 的前 100 个条目。解决方案本身非常简单，只需要非常基本的工具，但是人们在很多地方会犯错误。

我们可以确定的一件事是，我们将需要某种循环，因为我们需要多次执行给定的动作。严格来说，我们选择 for 循环还是 while 循环并不重要，但是 for 循环可能更合适，因为我们希望执行特定次数的迭代。在这种情况下，我们将使用`range()`来生成一个数字列表供我们迭代。

现在我们将忘记循环体，写下`pass`来表明它是故意为空的。

```
for number in range(1, 101):
    pass 
```

在构建我们的循环时，需要记住两件事:

1.  在编程中，我们通常从零开始计数，但在正常生活中，我们通常从一开始计数。我们需要确保我们的循环从 1 开始，而不是 0，除非面试官说不是这样。
2.  我们提供给`range()`的限额是**不包括**。我们使用`range(1, 100)`是不够的，因为 100 不会出现在我们的输出中。

### 循环逻辑

现在我们已经建立了 for 循环，我们需要决定如何正确地处理用适当的单词替换某些数字。

解决这个问题最简单的方法可能是对一个给定的数字进行一系列除法运算，找出它的因子。Python 有一个方便的运算符叫做 modulo `%`，它(对于正数)返回欧几里德除法的余数部分。模的对应项`//`返回商。你可以在[我们关于这个话题的帖子](https://blog.teclado.com/pythons-modulo-operator-and-floor-division/)中读到更多关于这些运营商的信息。

如果一个数能被三整除，我们取模运算的结果应该是`0`，所以我们现在可以写一个简单的 If 条件。

```
if number % 3 == 0:
    pass 
```

如果您愿意，您可以使用标准除法和`is_integer()`方法来代替:

```
if (number / 3).is_integer():
    pass 
```

检查 FizzBuzz 的方法有很多。我们可以用`and`来检查一个数是否能被`3`和`5`整除，或者你可能意识到任何能被`3`和`5`整除的数都能被`15`整除:

```
if number % 3 == 0 and number % 5 == 0:
    print("FizzBuzz")

if number % 15 == 0:
    print("FizzBuzz") 
```

### 避免常见错误

到目前为止一切顺利，但是如果我们不小心的话，很容易落入其他陷阱。我在学生中经常看到的一个问题是这样的:

```
for number in range(1, 101):
    if number % 3 == 0:
        print("Fizz")
    if number % 5 == 0:
        print("Buzz")
    if number % 15 == 0:
        print("FizzBuzz")
    if number % 3 != 0 and number % 5 !=0:
        print(number) 
```

这里其实有几个问题。第一个是我们在第 15 次迭代中得到一个不期望的输出:

```
...
13
14
Fizz
Buzz
FizzBuzz 
```

我们触发了所有的 if 条件，因为我们忘记了将这些条件绑定到一个块中。我们应该使用`elif`来确保只有一个条件被触发。同样，我们没有必要对不符合任何特殊条件的数字进行最后的检查。我们用`else`就可以了。

```
for number in range(1, 101):
    if number % 3 == 0:
        print("Fizz")
    elif number % 5 == 0:
        print("Buzz")
    elif number % 15 == 0:
        print("FizzBuzz")
    else:
        print(number) 
```

这解决了我们的一些问题，并使我们的代码更整洁，但现在我们有了一个新问题:

```
...
13
14
Fizz 
```

我们从不触发“FizzBuzz”，因为任何满足“FizzBuzz”的数字也满足我们的第一个“Fizz”条件。我看到我们的学生犯了一个非常普遍的错误，不仅仅是这个特定的问题。

我们必须确保我们的“嘶嘶”状态排在第一位，保证我们不会过早地意外触发“嘶嘶”或“嗡嗡”状态。

```
for number in range(1, 101):
    if number % 15 == 0:
        print("FizzBuzz")
    elif number % 3 == 0:
        print("Fizz")
    elif number % 5 == 0:
        print("Buzz")
    else:
        print(number) 
```

现在一切都按预期运行了，运行我们的程序将会给出 FizzBuzz 的前 100 个结果。厉害！

## 替代解决方案

与编程中的所有事情一样，解决这个问题的方法不止一种，所以我将在这里展示几个其他的解决方案。如果你有解决问题的不同方法，我们很乐意在推特上听到。

### 嵌套的`if`语句

处理 for 循环内部逻辑的一种方法是将条件块放在其他条件块中。我们可以测试任何能被`3`整除的东西，看看它是否也能被`5`整除，帮助我们避免我们之前看到的阻止“FizzBuzz”打印到控制台的排序问题。

```
for number in range(1, 101):
    if number % 3 == 0:
        if number % 5 == 0:
            print("FizzBuzz")
        else:
            print("Fizz")
    elif number % 5 == 0:
        print("Buzz")
    else:
        print(number) 
```

### 串并置

一种完全不同类型的解决方案包括根据特定条件将“Fizz”和“Buzz”附加到空字符串上。

我们用一个空字符串开始每次迭代，如果这个数能被`3`整除，就追加“Fizz ”,如果这个数能被`5`整除，就追加“Buzz”。请注意，这两个 if 语句是独立的，因此它们总是运行的。这意味着在“FizzBuzz”事件中,“Fizz”和“Buzz”都被附加到字符串中，不需要额外的检查。

一旦字符串准备好了，我们可以检查它是否还是空的，如果是，就打印出数字:

```
for number in range(1, 101):
    string = ""

    if number % 3 == 0:
        string += "Fizz"
    if number % 5 == 0:
        string += "Buzz"

    if string == "":
        print(number)
    else:
        print(string) 
```

通过利用 Python 的`or`操作符，可以缩短对空字符串的最后检查，该操作符总是返回一个操作数，而不是`True`或`False`。我们将在[的另一篇文章](https://blog.teclado.com/logical-comparisons-in-python-and-or/)中详细讨论。

```
for number in range(1, 101):
    string = ""

    if number % 3 == 0:
        string += "Fizz"
    if number % 5 == 0:
        string += "Buzz"

    print(string or number) 
```

### 三元字符串串联

这个解决方案很像上面的解决方案，但是使用 Python 稍微有点晦涩的三元运算符语法来执行字符串连接。

```
for number in range(1, 101):
    string = "Fizz" if number % 3 == 0 else ""
    string += "Buzz" if number % 5 == 0 else ""

    if string == "":
        print(number)
    else:
        print(string) 
```

我们也可以在这里对`or`使用相同的技巧。

```
for number in range(1, 101):
    string = "Fizz" if number % 3 == 0 else ""
    string += "Buzz" if number % 5 == 0 else ""

    print(string or number) 
```

## 包扎

FizzBuzz 是一个有趣的挑战，是新手程序员的理想选择。我希望这个小步骤对你有所帮助，我们希望看到你对这个问题的解决方案。

如果你刚刚开始编程，一定要在 [Twitter](https://twitter.com/TecladoCode) 上关注我们，以了解我们所有的最新内容，也许可以考虑查看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)，或者加入我们的 [Discord 服务器](https://discord.gg/BBWwyMq)。我们很希望你能来！

你也可以注册我们下面的邮件列表。我们定期为我们的订户发布折扣代码，因此这是一个很好的方式来确保您始终获得我们课程的最佳优惠。