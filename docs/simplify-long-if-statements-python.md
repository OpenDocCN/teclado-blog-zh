# 如何简化 Python 中的长 if 语句

> 原文：<https://blog.teclado.com/simplify-long-if-statements-python/>

如果语句是很好的工具，但是随着分支数量的增长，它们很快变得难以使用。在本文中，我们将探索 if 语句的两种替代方案:

*   [“比赛...案例"](https://blog.teclado.com/python-match-case/)，Python 3.10 语言新特性。
*   字典，在处理用户输入时特别有用。

## 本文的场景:一个很长的 if 语句

假设您有一个像这样的 if 语句(假设已经定义了`average`函数):

```py
numbers = [1, 4, 16, 20]
action = input(f"What would you like to do with {numbers}?")  # e.g. add

if action == "add":
    print(sum(numbers))
elif action == "avg":
    print(average(numbers))
elif action == "max":
    print(max(numbers))
else:
    print("Action not recognized") 
```

## 使用火柴...case 来简化 if 语句链

在[比赛中...案例"](https://blog.teclado.com/python-match-case/)，我们仍然需要告诉 Python 不同的选项是什么，以及在每种情况下做什么:

```py
def average(seq):
	return sum(seq) / len(seq)

numbers = [1, 4, 16, 20]
action = input(f"What would you like to do with {numbers}? ")

match action:
	case "add":
		print(sum(numbers))
	case "avg":
		print(average(numbers))
	case "max":
		print(max(numbers))
	case _:
		print("Operation not recognized.") 
```

虽然这看起来好一点，但仍然有很多相同的重复。每个条件分支都有复制的`print()`函数，并且有大量的关键字。

总的来说，代码块的长度是相同的。

此外，最大的问题仍然存在:随着您添加更多选项，分支条件也会增加。

## 使用字典简化长的 if 语句

您可以将用户的选项存储在一个字典中，而不是使用 log if-elif 链或长的 match-case 链:

```py
options = {
    "add": sum,
    "avg": average,
    "max": max
} 
```

然后你可以问用户他们想要使用哪个选项(从字典中):

```py
options = {
    "add": sum,
    "avg": average,
    "max": max
}
numbers = [1, 4, 16, 20]

action = input(f"What would you like to do with {numbers}?")  # e.g. add 
```

这样，我们可以直接从字典中检索函数:

```py
options = {
    "add": sum,
    "avg": average,
    "max": max
}
numbers = [1, 4, 16, 20]

action = input(f"What would you like to do with {numbers}?")  # e.g. add

operation = options.get(action) 
```

因为字典将字符串映射到函数，所以`operation`变量现在将包含我们想要运行的函数。

剩下的就是运行`operation(numbers)`来得到我们的结果。如果用户输入了`'add'`，那么`operation`将是`sum`功能。

我们还应该做一些错误检查，以确保我们不会试图运行一个不存在的函数，如果用户输入的东西不是字典的键之一。

```py
options = {
    "add": sum,
    "avg": average,
    "max": max
}
numbers = [1, 4, 16, 20]

action = input(f"What would you like to do with {numbers}?")  # e.g. add

operation = options.get(action)

if operation:
    operation(numbers)
else:
    print("Action not recognized") 
```

您仍然需要 one if 语句，以防用户选择字典中没有关键字的内容，但这是一个不会随时间增长的单分支 if 语句。

另一个好处是，通过使用字典键，您可以很容易地告诉用户哪些选项是可用的:

```py
option_texts = '|'.join(options.keys()
action = input(f"What would you like to do with {numbers}? ({option_texts}) ")
# Would show "What would you like to do with [1, 4, 16, 20]? (add|avg|max) " 
```

## 结论

在这篇文章中，我们看到了两种简化 if 语句的方法:使用“匹配”...格”和使用字典。

如果你想学习更多关于 Python 的知识，可以考虑参加我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)，它将带你从初级到高级(包括 OOP、web 开发、异步开发等等！).我们有一个 **30 天退款保证**，所以你真的没有什么损失去尝试一下。我们很希望你能来！

照片由[克里斯汀·休姆](https://unsplash.com/@christinhumephoto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)