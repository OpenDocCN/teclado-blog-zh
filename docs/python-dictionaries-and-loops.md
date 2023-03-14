# Python 字典和循环

> 原文：<https://blog.teclado.com/python-dictionaries-and-loops/>

在本周的 [Python 片段 post](https://blog.teclado.com/tag/python-snippets/) 中，我们来看看字典和一些可以用来以不同方式迭代字典的方法。

首先让我们来看看当我们正常地迭代一个字典时会发生什么:

```py
example_dict = {
    "name": "James",
    "age": 25,
    "nationality": "British"
}

for x in example_dict:
    print(x)

# name
# age
# nationality 
```

默认情况下，我们只是在迭代一个字典时得到键。这可能是我们想要的，但是如果我们想要的是价值观呢？嗯，我们可以用`values`的方法。

```py
for value in example_dict.values():
	print(value)

# James
# 25
# British 
```

我们只需调用正在迭代的字典上的`values`,并获取每个键的值。

那么我们能同时得到两个吗？绝对的！

不使用`values`方法，我们可以调用`items`方法。这将为字典中的每个键提供一个元组，该元组按顺序包含键和值。

一般来说，我们会想要将这个元组分解成单独的值，如下所示:

```py
for key, value in example_dict.items():
    print(key, value)

# name James
# age 25
# nationality British 
```

然而，这并不是必须的，我们可以很容易地将元组分配给一个循环变量，而不是将它分成几个组成部分。

## 包扎

这就是这篇关于迭代字典的短文的全部内容。我们在这篇文章中讨论的工具是 Python 开发中绝对重要的一部分，因此非常值得熟悉。

我希望你学到了新的东西，如果你对提升你的 Python 游戏感兴趣，请查看[完整的 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！您可能还想注册我们的邮件列表，以获取我们所有课程的折扣代码！