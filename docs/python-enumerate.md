# Python 的枚举函数

> 原文：<https://blog.teclado.com/python-enumerate/>

Python 的`enumerate`函数是一个很棒的工具，它允许我们在迭代的值旁边创建一个计数器:例如，作为循环的一部分。

`enumerate`接受一个 iterable 类型作为它的第一个位置参数，例如一个列表、一个字符串或一个集合，并返回一个包含许多元组的 enumerate 对象:iterable 中的每一项都有一个元组。每个元组包含一个整数计数器和一个来自 iterable 的值。

`enumerate`还带有一个可选的附加参数`start`，它可以作为关键字或位置参数提供。如果没有提供起始值，`enumerate`将从零开始计数，这使得它成为跟踪每个项目索引的完美工具。

发现`enumerate`被使用的最常见地方是在类似 for 循环的地方，枚举对象[中的元组将](https://blog.teclado.com/destructuring-in-python/)析构为两个独立的循环变量。

```py
friends = ["Rolf", "John", "Anna"]

for counter, friend in enumerate(friends, start=1):
	print(counter, friend)

# 1 Rolf
# 2 John
# 3 Anna 
```

然而,`enumerate`的使用不仅限于循环；例如，我们也可以利用`enumerate`作为列表理解的一部分，或者甚至作为参数传递给`dict`。

```py
friends = ["Rolf", "John", "Anna"]
friends_dict = dict(enumerate(friends))  # {0: 'Rolf', 1: 'John', 2: 'Anna'} 
```

## 包扎

如果你有兴趣学习更多像这样酷的技巧，查看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)，或者加入我们的 [Discord 服务器](https://discord.gg/BBWwyMq)。我们很希望你能来！

下面还有一个表格，可以注册我们的邮件列表。如果你喜欢我们所做的，这是一个很好的方式来保持我们所有内容的更新，我们还与我们的订户分享我们课程的折扣代码，确保他们总是在我们的课程上获得最好的交易。