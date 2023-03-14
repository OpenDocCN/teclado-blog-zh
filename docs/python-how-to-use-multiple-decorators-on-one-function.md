# Python:如何在一个函数上使用多个装饰器

> 原文：<https://blog.teclado.com/python-how-to-use-multiple-decorators-on-one-function/>

希望你已经学习了 Python 中的 decorators！这篇博文将讨论当你在一个函数上使用多个装饰器时会发生什么。

例如:

```
@user_has_permission
@user_name_starts_with_j
def double_decorator():
    return 'I ran.' 
```

装饰者最常用于扩展一个函数，在函数之前或之后添加一些东西。

装饰器内部发生的事情是它改变了它所装饰的功能。它改变该功能并将其转换成具有附加功能的新功能。

所以如果我们有这个:

```
@user_name_starts_with_j
def decorated_function():
    return 'I ran.' 
```

`decorated_function`不再仅仅是`return 'I ran.'`。现在它变成了装饰者返回的任何东西。希望装饰器返回使用`decorated_function`的东西，这样它仍然返回`'I ran.'`！

假设`user_name_starts_with_j`装饰器做两件事:

1.  它检查用户名(不管是什么)是否以字母`'j'`开头。
2.  如果是，那么它调用`decorated_function`。否则，它会打印一个错误。

从定义`decorated_function`的那一刻起，它就变成了一个做上述事情的函数。现在每当我们调用`decorated_function()`，如果用户名不是以`'j'`开头，我们就会得到一个错误。我们不用再用`user_name_starts_with_j`了。

## 两个装修工

当你有两个装修工时，同样的事情也适用。让我们以这段代码为例:

```
@user_has_permission
@user_name_starts_with_j
def double_decorator():
    return 'I ran.' 
```

首先，`@user_name_starts_with_j`修改`double_decorator`函数。

然后，`@user_has_permission`修改上一次修改的结果。

`double_decorator`现在已经变成了`user_has_permission`返回的函数。这是一个功能:

1.  检查用户是否有正确的权限。
2.  如果有，调用原始函数。否则，打印一个错误。

所以当我们调用`double_decorator()`的时候，上面的情况首先发生。如果用户有正确的权限，我们调用原始函数——这是前面转换的结果。

因此，我们检查用户名是否以 j 开头。如果是，我们调用原始函数(并返回`'I ran.'`)。否则，我们将打印另一个错误。

所以两个装饰者的检查顺序变成了:

1.  检查许可。
2.  检查用户名以 j 开头。
3.  运行原始功能。

正如您所看到的，装饰器运行的顺序与它们用来装饰函数的顺序相反——这是因为它们内部的工作方式。

## 代码示例

下面你可以使用示例代码。我用不同的用户名和访问级别定义了几个`user`变量，你可以试试。您也可以在浏览器中运行这段代码来查看输出！

感谢阅读！如果你想让你的 Python 技能更上一层楼，请查看我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)！