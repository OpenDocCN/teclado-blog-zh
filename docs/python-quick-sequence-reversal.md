# Python 中的快速序列反转

> 原文：<https://blog.teclado.com/python-quick-sequence-reversal/>

Python 列表有一个方便的方法叫做`reverse`，但是它并不总是我们想要的。首先，我们不能在其他序列类型上使用它，比如元组和字符串，而且它还对原始序列进行就地修改。

使用切片，我们可以绕过这些限制，在将序列赋给一个新变量之前，反转我们喜欢的任何序列。如果我们想使用相同的变量名，当然可以，因为 Python 允许我们随意将名称绑定到新对象。

```py
friends = ["Rolf", "John", "Mary"]
friends_reversed = friends[::-1]
print(friends_reversed)  # ['Mary', 'John', 'Rolf']

greet = "Hello, World!"
print(greet[::-1])  # "!dlroW ,olleH" 
```

这种方法使用扩展片段向后遍历序列，创建一个包含所有原始序列元素的新序列。

虽然这一切都很好，只是因为我们可以做一些事情，并不意味着我们应该这样做。当处理列表时，你几乎总是想要使用可读性更好的`reverse`方法。你可以在这里找到它的文档[。](https://docs.python.org/3/tutorial/datastructures.html?highlight=list#more-on-lists)

对于其他序列类型，这种切片反转非常简洁，可能是最佳选择。

如果你想了解更多关于切片的知识，我们有两篇文章会更详细地介绍它们是如何工作的:[第一部分](https://blog.teclado.com/python-slices/)，[第二部分](https://blog.teclado.com/python-slices-part-2/)。我们还将在我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)中介绍切片！