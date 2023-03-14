# Python 对集合的对称差分方法

> 原文：<https://blog.teclado.com/python-symmetric-difference/>

欢迎来到另一篇 [Python 片段](https://blog.teclado.com/tag/python-snippets/)的帖子。今天我要讲一个不太为人所知的集合运算，叫做对称差。如果你对集合不是很熟悉，你可能想看看我们之前关于这个主题的帖子:[第 11 天:集合](https://teclado.com/30-days-of-python/python-30-day-11-sets/)，[集合运算符](https://blog.teclado.com/python-set-operators/)。

关于`difference`方法，许多学生没有意识到的一件事是，它会为相同的两个集合产生不同的输出，这取决于你在哪个集合上调用了该方法。

```
s1 = {1, 3, 4, 5, 7, 8}
s2 = {2, 3, 4, 6, 8, 9}

print(s1.difference(s2))  # {1, 5, 7}
print(s2.difference(s1))  # {9, 2, 6} 
```

当使用`-`操作符而不是方法语法时，情况也是如此。

`symmetric_difference`是不同的，它实际上与大多数人期望的`difference`非常相似。`symmetric_difference`返回没有出现在**和**集合中的所有值。

对于集合`s1`和`s2`，`s1`和`s2`的对称差相当于`s1.difference(s2)`和`s2.difference(s1)`的并集。

```
s1 = {1, 3, 4, 5, 7, 8}
s2 = {2, 3, 4, 6, 8, 9}

s3 = s1.symmetric_difference(s2)
s4 = s1.difference(s2) | s2.difference(s1)

print(s3)  # {1, 2, 5, 6, 7, 9}
print(s4)  # {1, 2, 5, 6, 7, 9} 
```

对于那些喜欢使用集合操作符而不是方法语法的人来说，对称差的操作符是`^`。

## 包扎

希望你学到了一些新东西，下周一定要回来查看另一个 Python 片段！如果你等不及了，为什么不看看我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)？有超过 35 个小时的材料，让你忙起来，随着测验，练习，和几个大项目！

如果您注册了下面的邮件列表，我们将不胜感激。这是了解我们所有内容的最佳方式，我们还会定期分享我们课程的折扣代码，确保您获得最优惠的价格。