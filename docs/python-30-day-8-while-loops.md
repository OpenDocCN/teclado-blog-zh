# 第八天:While Loops 

> 原文：<https://blog.teclado.com/python-30-day-8-while-loops/>

欢迎来到 Python 系列 [30 天的第 2 周！](https://blog.teclado.com/30-days-of-python/)

本周一开始，我们将讨论另一种叫做`while`循环的循环。我们还将寻找一些新的方法来控制我们的循环流。

如果你是这个系列的新手，你可以在这里找到你错过的。

此外，我们还制作了第 6 天和第 7 天的简短回顾，您可以在此处查看来刷新您的记忆。

## `while`循环

在上一篇文章中，我们看了一下作为减少代码重复手段的`for`循环。`for`循环适用于两种重复动作:

1.  当我们想为某个组中的每个项目做些什么的时候。
2.  当我们想做某件事一定次数时。

然而，我们可能想要进行其他类型的重复。例如，如果我们想根据需要多次执行某个操作，直到满足某个条件，该怎么办？如果我们想永远一遍又一遍地执行某个动作呢？

对于这种重复动作，我们可以用一个`while`循环来代替。

一个`while`循环在许多方面都比一个`for`循环简单得多。我们只需要使用`while`关键字，后跟一些条件来测试。如果条件评估为[真值](/30-days-of-python/python-30-day-5-conditionals-booleans/)，循环将运行一次迭代，然后再次测试条件。

例如，我们可以编写一个`while`循环，它将一直运行，直到用户输入 10 或更高的值。

```py
`user_number = input("Please enter a number: ")

while int(user_number) < 10:
    print("Your number was less than 10.")
    user_number = input("Please select another number: ")

print("Your number was at least 10.")` 
```

首先，我们从循环外部收集一个初始数字。我们需要这样做，因为我们将名称`user_number`作为`while`循环条件的一部分，所以如果我们不预先定义这个名称，我们将得到一个`NameError`。

即使不是这样，我们仍然没有`user_number`的值，那么我们要检查什么呢？

一旦用户输入一个数字，我们就到达循环开始的那一行。我们检查条件，如果条件满足，我们将运行一次循环体。在上面的例子中，如果用户输入`9`或更少，循环体将会运行。

如果我们在循环体中结束，我们将结束打印关于数字小于`10`的消息，然后我们将要求用户输入另一个数字。注意，我们将这个新值赋给了`user_number`，这意味着当我们再次测试条件时，我们现在测试的是一个新值。

如果我们给这个变量取别的名字，我们只是一遍又一遍地检查原始值，当然如果值保持不变，什么都不会改变。这可能会让我们陷入困境，因为循环最终会无限迭代，而这并不是故意的。

当我们有一个像这样检查一个值的`while`循环时，确保我们可以在循环内改变这个值。

## 无限循环

有时候无限循环是我们想要的，有很多方法可以实现这一点。

与条件语句非常相似，我们不需要为循环条件使用比较运算符:我们可以只指定一个值，这个值的真实性将决定循环是否运行下一次迭代。

实际上我们可以使用任何我们想要的表达式，因为所有的表达式都要计算一些值，如果我们得到一个值，我们可以测试它的真值。我们可以使用函数调用，或者算术运算，也可以引用变量。天空是无限的。

当我们想要指定一个无限循环的时候，我们想要写一个表达式，这个表达式总是被计算为真值。一个很好的例子是布尔值`True`，如果我们测试它的真值，它总是会计算为`True`。

我不建议您运行这段代码，但是我们可以这样写:

```py
`while True:
    print("Hello there!")` 
```

这里将要发生的是，Python 将会*非常*地快速运行这个循环的多次迭代，我们将会以持续不断的`"Hello there!"`流打印到控制台而结束。

当我们像这样编写一个显式无限循环时，通常重要的是我们有一些方法来停止它，这通常是通过某种条件语句结合`break`来完成的。

例如，让我们创建一个简单文本菜单的框架。我们将有几个选项供用户选择，其中一个选项是关闭菜单。这个选项就是字符串`"q"`。

```py
`while True:
    selected_option = input("Please enter 'a', 'b', or 'c', or enter 'q' to quit: ")

    if selected_option == "a":
        print("You selected option 'a'!")
    elif selected_option == "b":
        print("You selected option 'b'!")
    elif selected_option == "c":
        print("You selected option 'c'!")
    elif selected_option == "q":
        print("You selected option 'q'! Quitting the menu!")
        break
    else:
        print("You selected an invalid option.")` 
```

我们可以想象，不是简单地打印用户的选择，这些分支中的每一个都将代表用户执行一些操作，然后当我们完成时，我们提示用户另一个选项。

如果用户输入一个无效选项，我们用一个`else`子句捕捉所有这些情况，并通知用户他们的错误。

如果用户输入`"q"`，我们让用户知道我们正在退出菜单，然后我们使用一个`break`语句来打破循环。

注意，对于这种类型的循环，我们不需要在循环之外定义任何东西，因为我们没有依赖于某个变量的条件。

### 风格注释

如果你开始看别人的代码，你可能会看到一些像下面这样写的循环，用`1`作为循环条件:

```py
`while 1:
    ... some actions here ...` 
```

这是可行的，因为`1`是一个真值，就像所有非零数字一样。虽然它有效，但我个人建议不要使用它，出于同样的原因，我不建议你编写这样的循环:

`1`和`"llama"`都是真实值，但它们远没有写`while True`那么明确。显式，并使我们的代码尽可能容易阅读，比节省写三个字符更重要。

## `continue`关键字

我们已经多次使用了`break`语句，但是它不是我们控制循环流的唯一选择。我们可以使用的另一个选项是`continue`关键字。

虽然`break`允许我们退出循环，但是`continue`允许我们跳过当前迭代的循环体的剩余部分。

例如，让我们创建一个只打印偶数的循环:

```py
`for number in range(10):
    if number % 2 != 0:
        continue
    print(number)` 
```

对于循环的每次迭代，我们使用[模运算符](https://blog.teclado.com/pythons-modulo-operator-and-floor-division/)来确定当前数字是否能被`2`整除。如果不是，我们会遇到这个`continue`语句，它会立即将我们带到循环的下一次迭代。这意味着我们跳过循环体的剩余部分，并且不打印数字。

因此，输出将如下所示:

虽然这个例子在`for`循环中使用了`continue`语句，但是它们当然也可以用于`while`循环。

在我看来,`continue`语句通常并不那么有用，但是它们确实可以帮助简化复杂条件下的代码。

## 使用带有循环的`else`子句

回到第 5 天，我们在条件语句的上下文中看了一下`else`子句，但是我当时说我们也可以在其他结构中使用`else`。`for`循环和`while`循环就是这些结构中的一种。

循环上下文中的`else`子句有点奇怪，似乎没有太大意义。它当然不像使用`if`语句那样直观。我认为当我们在循环中使用`else`时，它是一个“不中断”子句。

我之所以说把`else`理解为“不中断”是有好处的，是因为只有在循环执行期间没有遇到`break`语句时，附加到循环的`else`子句才会运行。

例如，让我们写一个循环来判断一个数是否是质数。质数是只能被自身和`1`整除的数。比如`2`、`3`、`5`、`7`、`11`、`13`都是质数。

我们确定一个东西是否是质数的一种方法是用它除以它前面的每个数。如果这些除法都不产生整数结果，我们知道这个数是质数。

不过，我们不必检查每个数字。如果我们找到一个产生整数结果的除法，我们知道这个数不是质数，所以我们不需要进一步检查。因此，我们可以打破这个循环。

如果我们只要发现一个数不是质数就中断，这意味着如果我们完成循环，并因此没有遇到`break`语句，我们就有一个质数。这些也是触发`else`子句的条件。

```py
`# Get a number to test from the user
dividend = int(input("Please enter a number: "))

# Grab numbers one at a time from the range sequence
for divisor in range(2, dividend):
    # If user's number is divisible by the curent divisor, break the loop
    if dividend % divisor == 0:
        print(f"{dividend} is not prime!")
        break
else:
    # This line only runs if no divisors produced integer results
    print(f"{dividend} is prime!")` 
```

我们也可以用一个`while`循环做类似的事情:

```py
`# Get a number to test from the user, and set the initial divisor to 2
dividend = int(input("Please enter a number: "))
divisor = 2

# Keep looping until the divisor equals the number we're testing
while divisor < dividend:
    # If user's number is divisible by the curent divisor, break the loop
    if dividend % divisor == 0:
        print(f"{dividend} is not prime!")
        break

    # Increment the divisor for the next iteration
    divisor = divisor + 1
else:
    # This line only runs if no divisors produced integer results
    print(f"{dividend} is prime!")` 
```

有更复杂的方法来寻找质数，但是这些方法作为演示已经足够了。如果你喜欢挑战，看看你能否让他们更有效率。

如果你感兴趣的话，我们还有一篇简短的博文，里面有更多使用`else`的例子。

## 练习

祝贺你完成第一周！

希望这不会太难，但是如果你对一些概念有困难，下面的练习应该有助于巩固一切。如果你遇到困难，我们总是在我们的 [Discord 服务器](https://discord.gg/BBWwyMq)上提供帮助，所以请随时过来询问任何问题。

1)使用`while`循环编写一个简短的猜谜游戏程序。应该提示用户猜一个在`1`和`100`之间的数字，每次猜完后你应该告诉他们他们的猜测是太高还是太低。循环应该保持运行，直到用户正确地猜出数字。

2)使用一个循环和`continue`关键字打印出字符串`"Python"`中的每个字符，除了`"o"`。

请记住，字符串是集合，它们是可迭代的，所以您可以迭代字符串，这将一次产生一个字符。

3)使用前面的一个例子，或者完全是你自己的解决方案，创建一个程序，打印出 1 到 100 之间的所有质数。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-8-exercise-solutions)

## 额外资源

在 Python 3.8 中，我们有了一个新的语法，叫做*赋值表达式*，我们可以在 while 循环中使用它。如果你想了解更多，请查看[我们关于赋值语句的帖子](https://blog.teclado.com/python-assignment-expressions/)。