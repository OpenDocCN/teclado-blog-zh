# 扩展 Python 列表

> 原文：<https://blog.teclado.com/python-extending-lists/>

在本周的 [Python 片段帖子](https://blog.teclado.com/tag/python-snippets/)中，我们来看看列表的`extend`方法。

`extend`很像`append`方法，但是`extend`不是添加一个值，而是允许我们在给定列表对象的末尾添加几个元素。

让我们首先定义几个列表:

```py
l_1 = [1, 2, 3, 4]
l_2 = [5, 6, 7, 8] 
```

我们将使用`extend`将来自`l_2`的值加到`l_1`的末尾:

```py
l_1.extend(l_2)

print(l_1)  # [1, 2, 3, 4, 5, 6, 7, 8] 
```

正如我们所看到的，`extend`方法在进行就地操作，所以它修改了原始列表。除此之外，它的表现非常类似于对列表使用`+`操作符，所以我们为什么要关心`extend`？

嗯，`extend`可以接受任何 iterable，而使用类似于`+`的东西来执行串联只有在两个对象都是列表时才有效。所以我们可以这样做:

```py
[1, 2, 3] + [4, 5, 6] 
```

而下面就给我们一个`TypeError`:

```py
[1, 2, 3] + (4, 5, 6) 
```

另一方面，使用`extend`,一切都很好:

```py
l_1 = [1, 2, 3, 4]
t_1 = (5, 6, 7, 8)

l_1.extend(t_1)  # [1, 2, 3, 4, 5, 6, 7, 8] 
```

## 包扎

`extend`到此为止！我希望你学到了一些新东西，我希望你能在自己的代码中找到使用`extend`的地方。

如果你刚刚开始学习 Python，或者你只是想提高自己，请查看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！我们将带您从初级水平到一些非常高级的 Python，所以这是一个发展您的 Python 技能的好地方。

你可能也想看看我们下面的邮件列表，因为我们为我们的订户定期发布折扣代码，这样他们就可以在我们所有的课程中获得最优惠的价格。