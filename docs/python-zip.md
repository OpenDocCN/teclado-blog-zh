# 在 Python 中使用 zip

> 原文：<https://blog.teclado.com/python-zip/>

Python 的`zip`函数是一个未被充分利用的极其强大的工具，尤其是在循环中处理多个集合时。在这篇文章中，我们将展示几个很酷的方法，你可以使用`zip`来大大改进你的 Python 代码。

## 什么是`zip`

`zip` is 函数允许我们将两个或多个可迭代对象组合成一个可迭代对象。顾名思义，它将来自不同 iterables 的值交织在一起，创建一个元组集合。

例如，列表`[1, 2, 3]`和`["a", "b", "c"]`将产生一个包含`(1, "a")`、`(2, "b")`和`(3, "c")`的`zip`对象。

## 何时使用`zip`

在[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)中，我们经常看到新生在一个循环中使用两个独立的集合时，他们倾向于这样做:

```py
names = ["John", "Anne", "Peter"]
ages = [26, 31, 29]

for i in range(len(names)):
	print(f"{names[i]} is {ages[i]} years old.") 
```

我当然能理解为什么有人会写这样的东西。通过生成索引集合，我们可以使用列表的稳定排序来访问每个集合中的正确值。我们可以使用这种技术来组合尽可能多的集合。

然而，这种情况是`zip`的主要候选。使用`zip`，我们可以将`names`和`ages`组合成一个闪亮的新`zip`对象。然后我们可以迭代`zip`对象，我们也可以利用一些[析构](https://blog.teclado.com/destructuring-in-python/)，允许我们为循环变量使用漂亮的描述性名称。

```py
names = ["John", "Anne", "Peter"]
ages = [26, 31, 29]

for name, age in zip(names, ages):
	print(f"{name} is {age} years old.") 
```

## 反向使用`zip`

似乎这还不够，实际上还可以做得更多。它真的一直在给予。

使用`*`操作符，我们可以分解一个`zip`对象，或者实际上任何集合的集合。例如，我们可能有这样的东西:

```py
zipped = [("John", 26), ("Anne", 31), ("Peter", 29)] 
```

我们可以将`*`操作符与`zip`结合使用，将它拆分回`names`和`ages`:

```py
zipped = [("John", 26), ("Anne", 31), ("Peter", 29)]
names, ages = zip(*zipped)

print(names)  # ("John", "Anne", "Peter")
print(ages)   # (26, 31, 29) 
```

## 包扎

是一个非常棒的工具，可以去除循环中的索引之类的东西，它还可以用来将嵌套的集合分解成特定的字段。

如果你有兴趣学习更多像这样酷的东西，在 [Twitter](https://twitter.com/TecladoCode) 上关注我们，并考虑参加我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)。我们最近更新了所有新的部分，所以它比以往任何时候都更有价值！

我们在下面还有一个表格，用于注册我们的邮件列表，我们在那里张贴所有课程的常规折扣代码，确保您获得尽可能好的交易。