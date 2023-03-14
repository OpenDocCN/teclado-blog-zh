# 第 7 天:锻炼解决方案

> 原文：<https://blog.teclado.com/python-30-day-7-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 7 天练习](/30-days-of-python/python-30-day-7-split-join)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)要求用户输入他们的名和姓来响应一个提示。使用`split`提取名称，然后将每个名称分配给不同的变量。

首先，我们需要从用户那里获得信息。

```py
`names = input("Please enter your full name: ")` 
```

现在我们有了名字，我们需要把它们分成名和姓。我们可以用`split`方法做到这一点。

请记住，函数调用是一个表达式，在`input`的情况下，该表达式的值是一个字符串，包含用户为响应我们的提示而键入的字符。由于`input`调用的结果是一个字符串，我们可以在`input`调用中直接调用`split`方法。

```py
`names = input("Please enter your full name: ").split()` 
```

现在`names`包含一个字符串列表，这是从`split`返回的内容。

现在我们需要将名字分成两个变量，我将称之为`given_name`和`surname`。

```py
`names = input("Please enter your full name: ").split()

given_name = names[0]
surname = names[1]` 
```

在第九天，我们将学习一种更好的方法，敬请期待。

### 2)使用`join`方法以`1 | 2 | 3 | 4 | 5`的格式打印列表`[1, 2, 3, 4, 5]`。

这个解决方案有一个棘手的部分，即原始列表项是数字。如果我们想使用`join`方法，我们需要一个字符串集合。因此，我们的第一步是将每个数字转换成一个字符串。

有很多方法可以做到这一点，但我将选择一个简单的`for`循环方法。在这里，我将遍历原始列表中的条目，并将一个字符串版本追加到一个新列表中。

```py
`base_numbers = [1, 2, 3, 4, 5]
processed_numbers = []

for number in base_numbers:
    processed_numbers.append(str(number))` 
```

现在我们有了字符串集合，我们可以将这个集合传递给`join`方法。我们必须在一个字符串上调用`join`，这个字符串代表我们想要放在集合中字符串之间的字符。在这种情况下，我们希望这个字符串是`" | "`。

```py
`base_numbers = [1, 2, 3, 4, 5]
processed_numbers = []

for number in base_numbers:
    processed_numbers.append(str(number))

print(" | ".join(processed_numbers))` 
```

### 3)下面是简短的报价列表。每个引号都是一个字符串，但是每个字符串实际上都在开头和结尾包含引号字符。使用切片，从每个字符串中提取文本，去掉多余的引号，并打印每个引号。

```py
`quotes = [
    "'What a waste my life would be without all the beautiful mistakes I've made.'",
    "'A bend in the road is not the end of the road... Unless you fail to make the turn.'",
    "'The very essence of romance is uncertainty.'",
    "'We are not here to do what has already been done.'"
]` 
```

因为我们需要为每一项做些什么，所以让我们从建立一个 for 循环开始，按原样打印报价:

```py
`quotes = [
    "'What a waste my life would be without all the beautiful mistakes I've made.'",
    "'A bend in the road is not the end of the road... Unless you fail to make the turn.'",
    "'The very essence of romance is uncertainty.'",
    "'We are not here to do what has already been done.'"
]

for quote in quotes:
    print(quote)` 
```

每个引号都有两个我们想要删除的字符:一个在开头，一个在结尾。因此，我们的 slice 希望从第一个索引中获取所有内容，并且应该获取到最后一个项目之前的所有内容，但不包括最后一个项目。

我们可以这样写切片:`[1:-1]`。

现在，我们只需要在打印时将每个报价切片:

```py
`quotes = [
    "'What a waste my life would be without all the beautiful mistakes I've made.'",
    "'A bend in the road is not the end of the road... Unless you fail to make the turn.'",
    "'The very essence of romance is uncertainty.'",
    "'We are not here to do what has already been done.'"
]

for quote in quotes:
    print(quote[1:-1])` 
```

这里我们可以使用的另一个可能的解决方案是调用`strip`，传入一个单引号。

```py
`quotes = [
    "'What a waste my life would be without all the beautiful mistakes I've made.'",
    "'A bend in the road is not the end of the road... Unless you fail to make the turn.'",
    "'The very essence of romance is uncertainty.'",
    "'We are not here to do what has already been done.'"
]

for quote in quotes:
    print(quote.strip("'"))` 
```

### 4)让用户输入一个单词，然后打印出单词的长度。

这里的第一步是得到用户的话，并清理一下。例如，我们希望确保我们没有计算任何空白。

```py
`word = input("Please enter a word: ").strip()` 
```

一旦我们有了一个单词，得到它的长度是非常容易的。我们只需将它传递给`len`函数并打印结果:

```py
`word = input("Please enter a word: ").strip()

print(len(word))` 
```

对于扩展任务，我们需要找到总字符数和单词数。我们的原始代码已经考虑了字符数。我们只需要改变一些变量名。

```py
`sample_string = input("Please enter a word: ").strip()

character_count = len(sample_string)` 
```

现在我们需要找到一个字数。这里我们将采用一种非常简单的方法，我们将使用`split`方法来拆分`sample_string`。这将给我们一个使用空格作为分隔符创建的字符串列表。

如果我们找到这个列表的长度，我们将得到一个粗略的字数。

```py
`sample_string = input("Please enter a word: ").strip()

character_count = len(sample_string)
word_count = len(sample_string.split())` 
```

最后，我们需要打印结果:

```py
`sample_string = input("Please enter a word: ").strip()

character_count = len(sample_string)
word_count = len(sample_string.split())

print(f"Character count: {character_count}")
print(f"Word count: {word_count}")` 
```