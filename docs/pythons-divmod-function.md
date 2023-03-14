# Python 的 divmod 函数

> 原文：<https://blog.teclado.com/pythons-divmod-function/>

在这篇简短的帖子中，我们将简要讨论 Python 中一个不太为人所知的内置函数:`divmod`。这是一种通过单一操作执行欧几里德除法的快速而简单的方法。坚持到文章的结尾，因为我们将向你展示如何使用`divmod`将秒的时间转换成小时、分钟和秒，以便于阅读。

我们在博客上讨论过几次欧几里德除法，最著名的是在我们关于模和底除法运算符的的[文章中。这是我们在学习小数和分数之前在学校里做的除法。](https://blog.teclado.com/pythons-modulo-operator-and-floor-division/)

我们说`2`进入`5`两次，剩下`1`，而不是执行`5 / 2`并得到`2.5`。通常这被写成`2`余数`1`，或者`2r1`。

如果我们将这些数字代入`divmod`，我们得到的就是:

```py
euclidean_divison = divmod(5, 2)
print(euclidean_divison)  # (2, 1) 
```

`divmod`返回一个元组，其中第一个值是商(整数结果)，第二个值是余数。因为我们有结果的严格排序，如果我们需要独立使用两部分，我们可以[析构](https://blog.teclado.com/destructuring-in-python/)元组:

```py
quotient, remainder = divmod(5, 2) 
```

要记住的一件事是，负数可能会产生一些意想不到的结果，我建议查看模和地板除法帖子，以获得关于为什么会出现这种情况的详细解释。

我们没有提到的一种情况是浮动。完全可以将浮点数与`divmod`一起使用，结果元组中的两个数字也将是浮点数:

```py
print(divmod(5.5, 2))  # (2.0, 1.5)
print(divmod(6.0, 2))  # (3.0, 0.0) 
```

## 时间转换器

正如我在本文开头提到的，我们可以使用`divmod`将像`8594`这样大的秒值转换成小时、分钟和秒，这对普通人来说更容易解析。我们也可以对其他单位使用同样的方法，例如英尺和英寸。

我们解决这个问题的方法是通过获取我们的初始秒值，我们称之为`raw_time`，首先把它分解成分和秒。我们将通过使用`divmod`执行欧几里德除法来实现这一点，其中`raw_time`除以 60。然后我们可以将结果元组析构为一个`minutes`值和`seconds`值:

```py
raw_time = 8594
minutes, seconds = divmod(raw_time, 60)  # (143, 14) 
```

现在我们有了`minutes`的值，我们可以执行相同的操作，这次使用`minutes`而不是`raw_time`。这将产生一个包含`hours`和`minutes`的元组，我们可以像前面一样对其进行析构:

一旦我们完成了，我们可以打印一些消息给我们的用户，让他们知道转换的结果:

```py
raw_time = 8594

minutes, seconds = divmod(raw_time, 60)
hours, minutes = divmod(minutes, 60)

print(f"{raw_time}s is {hours}h {minutes}m {seconds}s")
# 8594s is 2h 23m 14s 
```

## 包扎

今天到此为止！不是一个经常使用的函数，但是如果你需要商数和余数来进行一些运算，它比单独使用除法和模运算符要干净得多。

如果你是 Python 新手，或者你只是想提高你的 Python 知识，你可能会对我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/learn/?couponCode=BLOGGER)感兴趣。您可能也想订阅我们下面的邮件列表，因为我们每个月都会与我们的订户共享折扣代码。

我们最近还做了一个 6 小时的免费直播，从绝对基础开始，一直到面向对象编程！如果你对此感兴趣，可以点击下面的链接查看:

[https://www.youtube.com/watch?v=Chy-32X2tqk](https://www.youtube.com/watch?v=Chy-32X2tqk)