# 更新 Python 字典

> 原文：<https://blog.teclado.com/python-updating-dictionaries/>

在本周的 [Python 片段帖子](https://blog.teclado.com/tag/python-snippets/)中，我们将看看 Python 字典的`update`方法。使用`update`方法有三种主要方式，它允许我们用新的键和值来扩展字典。

我们要看的第一个方法是使用另一个字典对象。我们可以在字典上调用`update`方法，并将第二个字典对象作为参数传入。然后，这些字典将合并成一个字典对象:

```py
student = {
    "name": "Rolf",
    "age": 15
}

grades = {
    "english": "A",
    "maths": "B",
    "science": "A*"
}

student.update(grades)
# {'name': 'Rolf', 'age': 15, 'english': 'A', 'maths': 'B', 'science': 'A*'} 
```

注意，这是一个就地操作，因此原始字典被该操作修改。如果你需要保存原版词典，最好复制一份。

使用`update`方法的第二个选项是传入一个包含键值对的 iterable。这些对依次存储在某种可迭代对象中，比如元组或列表。

```py
student = {
    "name": "Rolf",
    "age": 15
}

grades = [("english", "A"), ("maths", "B"), ("science", "A*")]

student.update(grades)
# {'name': 'Rolf', 'age': 15, 'english': 'A', 'maths': 'B', 'science': 'A*'} 
```

如你所见，我们得到了完全相同的结果。

用`update`扩展字典的最后一种方法是使用关键字参数。

```py
student = {
    "name": "Rolf",
    "age": 15
}

student.update(english="A", maths="B", science="A*")
# {'name': 'Rolf', 'age': 15, 'english': 'A', 'maths': 'B', 'science': 'A*'} 
```

这里每个关键字代表一个键，值是我们赋予该关键字的值。结果又是一样的。

在使用带有这些语法选项的`update`方法时要记住的一点是，如果一个键已经存在，这不会引发异常。相反，该项的值将被更新。换句话说，旧的价值观将被取代。

## 包扎

这一次到此为止！如您所见，`update`方法乐于接受多种不同形式的数据，这使得它非常通用。这也是我们经常使用的方法，所以我希望你能在自己的代码中尝试使用`update`的不同方式。

如果你想进一步提升你的 Python 技能，你可能想看看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！我们最近发布了一个重大更新，我们每天都在改进课程。我们很希望你能来！

如果你喜欢我们的内容，我也推荐你注册下面的邮件列表。这不仅能让您及时了解我们所有的内容，我们还与我们的订户共享折扣代码，以便他们可以在我们所有的课程中获得最优惠的价格。