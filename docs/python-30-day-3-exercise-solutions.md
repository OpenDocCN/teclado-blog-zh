# 第 3 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-3-exercise-solutions/>

以下是我们在 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中[第 3 天练习](/30-days-of-python/python-30-day-3-string-formatting)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)使用下面的变量，打印`"Hello, world!"`。

```
`greeting = "Hello, world"` 
```

在这个练习中，我们要做出选择。我们如何将这个感叹号添加到我们要打印的字符串中呢？

我们有三个主要选项:

1.  我们可以使用串联来添加一个只包含`"!"`到`greeting`的字符串。
2.  我们可以使用传入`greeting`的`format`方法。
3.  我们可以采取与选项 2 类似的方法，但是使用 f 弦

首先，我们来看看`format`，因为我认为`format`是目前为止这种情况下最差的选择。

```
`greeting = "Hello, world"

print("{}!".format(greeting))` 
```

我不喜欢这个选项的原因是它非常冗长，而且它也让我们很不清楚我们实际上在改变什么。感叹号就藏在那里。

可能是一个非常有用的工具，但在这种情况下就有点过头了。

接下来，让我们看看 f 弦，在我看来，它提供了一个更好的解决方案。

```
`greeting = "Hello, world"

print(f"{greeting}!")` 
```

这里发生的事情要少得多，这使得阅读起来容易得多。我认为这是一个非常好的选择。

然而，我认为我最喜欢的版本是将`greeting`和`"!"`连接起来。

```
`greeting = "Hello, world"

print(greeting + "!")` 
```

我喜欢这个解决方案，因为它很直观。我们并不真的需要了解很多代码来理解这种情况下发生了什么。

### 2)询问用户的姓名，然后问候用户，将他们的姓名作为问候的一部分。名称应该是大写的，并且不应该被任何多余的空格包围。

首先，我们需要知道这个人的名字。这意味着我们需要`input`函数。

```
`name = input("Please enter your name: ")` 
```

不要忘记在你的提示后面加一点空格，这样用户就不会直接写在你的提示文本旁边。它使东西更容易阅读。

下一步是处理用户名。我们需要删除任何多余的空白，并将其转换为标题的情况。这意味着我们需要`strip`和`title`方法，我们可以在从`input`返回的字符串上直接调用这个方法。

```
`name = input("Please enter your name: ").strip().title()` 
```

如果您愿意，您可以在另一行上这样做:

```
`name = input("Please enter your name: ")
name = name.strip().title()` 
```

现在我们有了用户的名字，我们必须放入某种问候字符串。在这种情况下，我将使用 f 弦:

```
`name = input("Please enter your name: ").strip().title()

print(f"Hello, {name}!")` 
```

### 3)将字符串`"I am "`和整数`29`连接起来，产生一个字符串`"I am 29"`。

首先，让我们将这个整数存储在一个变量中，这样我们就可以操作这个值。

现在我们有一个值可以引用，我们可以这样做:

```
`age = 29
print("I am " + str(age))` 
```

如果你不想过多地打乱`print`调用，你可以使用一些额外的变量。这两者都是完全合理的，例如:

```
`age = str(age)

print("I am " + age)` 
```

```
`age = 29
output_string = "I am " + str(age)

print(output_string)` 
```

### 4)使用字符串插值法格式化并打印以下信息。

```
`title = "Joker"
director = "Todd Phillips"
release_year = 2019` 
```

对于本练习，我们的目标输出是这样的:

```
`Joker (2019), directed by Todd Phillips` 
```

在这里，我认为最好的方法是使用 f 弦。由于数据已经被很好地命名，我们不必担心 f 字符串的类型，解决方案相对简单。

```
`title = "Joker"
director = "Todd Phillips"
release_year = 2019

print(f"{title} ({release_year}), directed by {director}")` 
```

请记住，花括号内的所有内容都被认为是 f 字符串的代码，我们要插入的每个值都应该在自己的花括号内。