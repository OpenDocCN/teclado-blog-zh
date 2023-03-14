# 第九天项目:信用卡验证器| Teclado

> 原文：<https://blog.teclado.com/python-30-day-9-project/>

欢迎来到蟒蛇系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 9](/30-days-of-python/python-30-day-9-enumerate-zip) 天项目！对于这个项目，我们将编写一个简单的信用卡验证器。当程序完成时，我们将能够确定给定的卡号是否有效。

这实际上比你想象的要容易得多，我将详细解释我们将要使用的算法来帮助你。你可以在下面找到这个解释，以及项目简介。之后，我们将在演练中展示一个模型解决方案，作为代码的一部分。

像往常一样，尽你最大的努力自己做这件事，但是如果你真的卡住了，跟着解决方案走，直到你觉得你可以自己完成它。请记住，如果你忘记了一些东西是如何工作的，你可以随时回头看看你的笔记，或者回到前几天的帖子！

## 算法的快速解释

我们将要用来验证卡号的算法被称为 [Luhn 算法](https://en.wikipedia.org/wiki/Luhn_algorithm)，或 Luhn 公式。这种算法实际上用于现实生活中测试信用卡或借记卡号码以及 SIM 卡序列号。

该算法的目的是识别可能输入错误的号码，因为它可以确定给定的号码是否可能是有效卡的号码。

我们将使用该算法的方式如下:

1.  删除卡号最右边的数字。这个数字被称为校验位，它将被排除在我们的大多数计算之外。
2.  颠倒剩余数字的顺序。
3.  对于这种倒序数字序列，取每个偶数索引处的数字(`0`、`2`、`4`、`6`等)。)并且翻倍。如果任何结果大于`9`，从这些数字中减去`9`。
4.  将所有结果相加，并添加校验位。
5.  如果结果能被`10`整除，则该数字为有效卡号。如果不是，则卡号无效。

让我们一步一步地看看这个有效的数字，这样我们就可以看到它的作用。我们要使用的号码是`5893804115457289`，这是一个有效的 Maestro 卡号，但不是一个正在使用的号码。

|数字|操作| | - | - | | 5893804115457289 |起始数字| | 589380411545728X |删除最后一位数字| | 827545114083985X |反转剩余数字| | 16214585218016318810X |偶数索引处的两位数| | 725585218073981X |超过 9 时减 9 |

现在，我们将这些数字相加，并添加校验数字:

```
`7 + 2 + 5 + 5 + 8 + 5 + 2 + 1 + 8 + 0 + 7 + 3 + 9 + 8 + 1 + 9` 
```

如果我们执行这一系列加法，我们得到`80`。`80`能被`10`整除，所以卡号有效。

## 案情摘要

你为这个项目编写的程序应该做到以下几点:

1)它应该能够接受来自用户的卡号。对于这个项目，您可以假设数字将以单个字符串的形式输入(即数字之间没有任何空格)。但是，您应该能够接受在字符串的开头或结尾带有空格的卡号。

如果你想挑战自己，你应该尝试在接受卡号的格式上更加灵活。

您可能希望将用户的输入转换成数字列表，这样会更容易处理。

2)程序应该使用上述 Luhn 算法验证该卡号。你应该自己实现这个算法。

在移除校验数字并反转卡号后，您将需要一个`for`循环来检查信用卡号。当你遍历每个数字时，你必须找到一种方法来确定一个数字是在奇数位置还是在偶数位置。记住如果你卡住了，你可以检查模型解！

3)一旦验证完成，程序应该通过向控制台打印一个字符串来通知用户卡号是否有效。

当您需要转到反转数字的步骤时，您可以使用`reversed`函数，它将接受任何序列类型:

```
`language = "Python"
numbers = [1, 2, 3, 4, 5]
letters = ("a", "b", "c", "d", "e")

language = reversed(language)    # 'nohtyP'
numbers = reversed(numbers)      # [5, 4, 3, 2, 1]
letters = reversed(letters)      # ('e', 'd', 'c', 'b', 'a')` 
```

`reversed`会给我们回一个懒人型(像`range`)，所以不能直接打印；但是，它是可迭代的。例如，我们可以在 for 循环中使用结果。

如果我们的数字在一个列表中，我们可以使用`reverse`方法。这直接修改了原始列表:

```
`numbers = [1, 2, 3, 4, 5]
numbers.reverse()

print(numbers)  # [5, 4, 3, 2, 1]` 
```

你可以使用你喜欢的任何一种技术。

测试您的解决方案时，您可以使用自己的卡号，也可以在网上找到用于测试支付方式的有效卡号。例如，Stripe 有一系列您可以使用的[测试卡号](https://stripe.com/docs/testing#cards)。

这样，你就可以开始练习了。祝你好运！

## 我们的解决方案

下面你会发现我们的解决方案的书面演练，但我们有一个链接到一个[视频版本](https://youtu.be/XGutqiC-REw)你更喜欢看代码视频。

像往常一样，我们将把解决方案分成小块，我建议你也这样做，定期测试以确保你没有在早期犯错误。如果你试着一口气写完所有的东西，你会很难找到这些早期的错误，因为它们被埋没了。

我们的第一步只是接受用户的卡号:

```
`card_number = input("Please enter a card number: ")` 
```

因为我们需要能够接受在开头或结尾添加了空格的卡号，所以我们需要对从用户那里得到的字符串进行少量处理。在这种情况下,`strip`就是我们所需要的，它会处理掉任何多余的空白。

```
`card_number = input("Please enter a card number: ").strip()` 
```

对于这个解决方案，我将从这个字符串创建一个列表，我这样做有几个原因。首先，`reverse`方法是一种非常干净的反转卡号的方式，其次，`pop`方法允许我非常干净利落地删除和存储校验位。

让我们从转换`card_number`开始:

```
`card_number = list(input("Please enter a card number: ").strip())` 
```

这里不需要用`split`，因为我只是想把字符串中的每个字符拆分成不同的列表项。如果我们将一个字符串传递给`list`函数，这是默认行为。

现在我们有了列表，让我们提取校验位并反转剩余的数字:

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()` 
```

现在我们有了反转的数字，我们需要在每一个偶数索引处取该数字并加倍。如果数字最终超过了`9`，我们需要从结果中减去`9`。

我认为解决这个问题最简单的方法是使用一个指数计数器。然后我们可以使用一个`for`循环来递增这个计数器，同时遍历`card_number`列表。

在`for`循环中，我们可以检查索引是否能被`2`整除。如果是，我们知道我们有一个均匀的指数。如果你需要复习如何检查一个数是否能被另一个数整除，我们在昨天的 Fizz Buzz 项目中讨论过。

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

index = 0

for digit in card_number:
    if index % 2 == 0:
        print("Even index")
    else:
        print("Odd index")

    # Increment the index counter for each iteration
    index = index + 1` 
```

我们还可以利用今天学到的`enumerate`函数，这样我们就不必自己跟踪这个计数器了:

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        print("Even index")
    else:
        print("Odd index")` 
```

现在我们已经建立了循环的这一部分，我们可以用一些有用的东西替换这些`print`调用。

我们需要添加的第一件事是存储我们修改过的数字的地方。为此，我们将创建一个名为`processed_digits`的空列表。然后我们将通过从`for`循环中追加条目来填充这个列表。

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

processed_digits = []

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        print("Even index")
    else:
        print("Odd index")` 
```

对于奇数指数，我们的任务很简单。我们只需要将`digit`转换成一个整数(记住我们目前有一个字符串列表)，然后我们需要调用`append`将该整数添加到`processed_digits`。

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

processed_digits = []

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        print("Even index")
    else:
        processed_digits.append(int(digit))` 
```

对于偶数索引，我们需要执行几个不同的步骤。首先，我们需要将数字转换成整数，并将数值加倍。

然后我们需要检查这个操作的结果是否大于`9`。如果是，我们需要从结果中减去`9`。然后我们需要将这个数字添加到`processed_digits`中。

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

processed_digits = []

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        doubled_digit = int(digit) * 2

        # Subtract 9 from any results that are greater than 9
        if doubled_digit > 9:
            doubled_digit = doubled_digit - 9

        processed_digits.append(doubled_digit)
    else:
        processed_digits.append(int(digit))` 
```

厉害！现在我们有了处理过的数字的列表，所以剩下的就是把它们加在一起并检查结果。

为了将数字加在一起，我们将再次使用一个`for`循环。这次我们将创建一个名为`total`的变量，并且我们将为`check_digit`设置一个初始值。

对于循环的每一次迭代，我们都要在这个总数上再加一个数字，我们将得到所有已处理数字的总和，以及校验位。

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

processed_digits = []

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        doubled_digit = int(digit) * 2

        # Subtract 9 from any results that are greater than 9 
        if doubled_digit > 9:
            doubled_digit = doubled_digit - 9

        processed_digits.append(doubled_digit)
    else:
        processed_digits.append(int(digit))

total = int(check_digit)

for digit in processed_digits:
    total = total + digit` 
```

如果我们尝试我们的初始卡号(`5893804115457289`)，我们应该总共得到`80`。

虽然这种方法可行，但有一种更简单的方法可以将 iterable 中的数字相加。我们可以将 iterable 传递给`sum`:

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

processed_digits = []

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        doubled_digit = int(digit) * 2

        # Subtract 9 from any results that are greater than 9 
        if doubled_digit > 9:
            doubled_digit = doubled_digit - 9

        processed_digits.append(doubled_digit)
    else:
        processed_digits.append(int(digit))

total = int(check_digit) + sum(processed_digits)` 
```

这不仅更短，也更容易阅读和理解。

现在我们已经有了卡片数字的总和，我们只需要测试总和是否能被`10`整除。我们可以使用与测试偶数指数相同的方法。

```
`card_number = list(input("Please enter a card number: ").strip())

# Remove the last digit from the card number
check_digit = card_number.pop()

# Reverse the order of the remaining numbers
card_number.reverse()

processed_digits = []

for index, digit in enumerate(card_number):
    if index % 2 == 0:
        doubled_digit = int(digit) * 2

        # Subtract 9 from any results that are greater than 9 
        if doubled_digit > 9:
            doubled_digit = doubled_digit - 9

        processed_digits.append(doubled_digit)
    else:
        processed_digits.append(int(digit))

total = int(check_digit) + sum(processed_digits)

# Verify that the sum of the digits is divisible by 10
if total % 10 == 0:
    print("Valid!")
else:
    print("Invalid!")` 
```

就这样，我们结束了！我们有一个全功能的卡号验证器。

## 额外资源

如果你有兴趣了解更多关于我们上面使用的`sum`函数，你可以在[官方文档](https://docs.python.org/3/library/functions.html#sum)中找到信息。