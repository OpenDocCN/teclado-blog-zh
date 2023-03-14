# 第 8 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-8-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 8 天练习](/30-days-of-python/python-30-day-8-while-loops)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)使用`while`循环编写一个简短的猜谜游戏程序。应该提示用户猜一个在`1`和`100`之间的数字，每次猜完后你应该告诉他们他们的猜测是太高还是太低。循环应该保持运行，直到用户正确地猜出数字。

首先，我们需要确定目标数量。您可以使用`random`模块随机生成这个值，但是这里我们只是硬编码一些值。假设`47`是我们的目标数字。

创建`while`循环时，我们有两种选择。我们可以编写一个循环条件，这样当用户正确猜出数字时循环就会停止，或者我们可以有一个显式的无限循环，在特定条件下用一个`break`语句来中断循环。

第一种方法如下所示:

```
`target_number = 47

guess = int(input("Enter a number: "))

while guess != target_number:
    print("Wrong!")
    guess = int(input("Enter a number: "))

print("You guessed correctly!")` 
```

在这个版本中，我们需要在循环之外定义`guess`，因为我们需要能够在循环的第一行引用它。我们还需要在每次迭代结束时重新定义`guess`，这样用户就可以为每次迭代选择一个新的数字。

如果用户猜对了，我们就跳出循环，只有这样我们才能命中这个`"You guessed correctly!"`字符串。

另一种方法如下所示:

```
`target_number = 47

while True:
    guess = int(input("Enter a number: "))

    if guess == target_number:
         print("You guessed correctly!")
         break
    else:
        print("Wrong!")` 
```

在这个版本中，所有的循环流都由循环中的条件语句控制。我们有一个 if 语句来检查用户是否得到了正确的数字，在这种情况下，我们可以手动执行循环。

Python 3.8 中还有第三种选择，使用[赋值表达式](https://blog.teclado.com/python-assignment-expressions/)。

这些选项中的任何一个都是完全有效的，所以选择你喜欢的。我将在这里使用第一个版本。

现在我们已经有了循环的框架，我们需要给用户一些关于他们猜测的反馈。我们需要告诉他们这个数字是太高还是太低。我们可以用条件语句来实现。

```
`target_number = 47

guess = int(input("Enter a number: "))

while guess != target_number:
    if guess > target_number:
        print("Too high!")
    else:
        print("Too low!")

    guess = int(input("Enter a number: "))

print("You guessed correctly!")` 
```

就这样，我们有了一个可行的猜谜游戏。

### 2)使用一个循环和`continue`关键字打印出字符串`"Python"`中的每个字符，除了`"o"`。

您可以使用任何您喜欢的循环，但是当试图对集合中的每一项做一次时，我们通常应该使用`for`循环。

让我们从创建测试字符串开始，并写出循环的大致轮廓:

```
`sample_string = "Python"

for character in sample_string:
    print(character)` 
```

目前，这将打印每个字符，但是我们需要从这个输出中排除`"o"`。

检查`"o"`相当简单。我们已经一个接一个地得到了字符，所以我们只需检查一个给定的字符是否是`"o"`。如果是，我们可以使用 continue 关键字开始一个新的迭代，而不用打印`"o"`。

```
`sample_string = "Python"

for character in sample_string:
    if character == "o":
        continue

    print(character)` 
```

### 3)创建一个程序，打印出 1 到 100 之间的所有质数。

对于解决方案，我将利用今天帖子中的实现:

```
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

我们只需要修改这个解决方案，以便可以检查多个号码。

我将采用的方法是创建一个外部循环，它迭代一个`range`。这个`range`将定义我们想要测试的数字。然后我们可以把数字一次一个地输入这个内部循环，看看它们是否是质数。

一个重要的细节是，我不会从`1`的`range`开始。我打算从`2`开始。我们一会儿会谈到原因。

```
`for dividend in range(2, 101):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            print(f"{dividend} is not prime!")
            break
    else:
        print(f"{dividend} is prime!")` 
```

假设我们提供`1`作为这个内部循环的红利。这意味着我们正在迭代`range(2, 1)`。这个序列包含什么？

答案是什么都没有。当我们试图迭代这个序列时，Python 将实现它的空，所以我们运行内部循环的零次迭代。

在这种情况下，会发生什么？嗯，`else`子句仍然运行，因为我们在内部循环中没有遇到`break`语句。这意味着`1`将被错误地报告为质数。

为了避免这种情况，我们从`2`开始序列，因为我们知道`1`不是质数。

我们还确保了`2`将被列为质数，因为`range(2, 2)`也是一个空序列。因此，我们跳过循环体，直接进入`else`子句。

因为我们只需要打印出质数，所以我们可以稍微整理一下我们的解决方案:

```
`for dividend in range(2, 101):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            break
    else:
        print(dividend)` 
```

现在我们只提供质数的输出，并且只打印数字本身。

如果您愿意，也可以创建一个数字列表并打印出来，如下所示:

```
`primes = []

for dividend in range(2, 101):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            break
    else:
        primes.append(dividend)

print(primes)` 
```

您也可以通过使用`join`让它变得更好。只是别忘了我们只能连接字符串集合！

```
`primes = []

for dividend in range(2, 101):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            break
    else:
        primes.append(str(dividend))

print(", ".join(primes))` 
```