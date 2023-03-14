# Python 中的列表理解

> 原文：<https://blog.teclado.com/python-list-comprehensions/>

列表理解是我最喜欢的 Python 语法之一。我们可以使用 list comprehension 从另一个 iterable 创建一个 list。它快速、简洁，最重要的是，易于阅读。

在处理代码时，我们经常会遇到这样的模式:

```py
numbers = [1, 2, 3, 4, 5]
doubled_numbers = []

for number in numbers:
	doubled_numbers.append(number * 2) 
```

这种方法功能完善，但与列表理解版本相比:

```py
numbers = [1, 2, 3, 4, 5]
doubled_numbers = [number * 2 for number in numbers] 
```

不仅代码更简洁，我们实际上也使它更易读。一个巨大的进步！

列表理解也允许一个以上的`for`子句。我们可以用它来找出两个骰子的所有可能的掷骰子组合，例如:

```py
roll_combinations = [(d1, d2) for d1 in range(1, 7) for d2 in range(1, 7)] 
```

对于一句俏皮话来说还不错！

你可以在官方文档中找到更多关于列表理解的信息。

## 包扎

如果你想学习更多像这样酷的技巧，一定要查看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)，并加入我们的 [Discord 服务器](https://discord.gg/BBWwyMq)。我们很希望你能来！

如果你对更多类似的内容感兴趣，一定要注册我们下面的邮件列表。这不仅是了解我们最新内容的好方法，我们还定期为我们的订户发布折扣代码，确保他们获得我们课程的最佳价格。