# Python 的“三元运算符”

> 原文：<https://blog.teclado.com/python-pythons-ternary-operator/>

欢迎来到另一篇 [Python 片段帖子](https://blog.teclado.com/tag/python-snippets/)。本周我们将简要了解一下 Python 中的条件表达式，有时被称为 Python 的三元运算符。

在 Python 中，条件表达式是一种有点晦涩的语法，但它们本质上允许我们根据某种条件给变量赋值。

让我们看一个简单的例子:

```
x = 6
value = x if x < 10 else "Invalid value"  # 6 
```

在这种情况下，我们有一些值绑定到变量`x`，我们检查`x`的值是否小于`10`。如果是，我们把号码分配给`value`；否则，我们指定字符串为`"Invalid value"`。

我们可以看到一种情况是`x`的值不小于下面的`10`:

```
x = 10
value = x if x < 10 else "Invalid value"  # Invalid value 
```

我们来分解一下语法。

首先，我们从条件为`True`时返回的值开始。在我们的例子中，这是`x`。然后我们有了跟随着一些条件的`if`关键字。在我们的例子中，这是一个使用小于运算符的比较，但是任何可以计算为布尔值的表达式都可以。在条件之后，我们使用`else`关键字，后跟如果条件评估为`False`时返回的值。

```
<value if condition True> if <condition> else <value if condition False> 
```

当谈到条件表达式时，要记住的一件事是，我们实际上需要所有的部分。如果我们不在乎这个条款，我们就不能简单地取消它。如果条件评估为`False`，我们必须明确定义一个值。

未能添加一个`else`子句会导致一个`SyntaxError`。

条件表达式可以通过追加到`else`子句来链接，但是语法已经够混乱了，所以我不建议这样做:

```
x = 16
value = x if x < 10 else "Invalid value" if x < 15 else "Super invalid value" 
```

那么，你应该在你自己的代码中使用条件表达式吗？大概不会。

就我个人而言，我认为条件和返回值的顺序非常不直观，很难理解这些条件表达式的逻辑。通常只使用 if 语句会清楚得多，即使它稍微长一点:

```
x = 10

if x < 10:
	value = x
else:
	value = "Invalid value" 
```

然而，这种结构在野外有很多例子，所以当它被用在其他人的代码中时，能够识别和理解它是很重要的。

## 包扎

这就是这篇文章的内容。希望你学到了一些新东西，我希望你能在自己的代码中找到一些条件表达式的用例。有时它们会非常有用。

如果你想进一步提升你的 Python 技能，我推荐你在 Udemy 上查看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！有超过 35 个小时的材料，以及几十个练习和项目，让您真正熟悉使用 Python。

我还建议注册您的邮件列表，因为我们会定期发布课程的折扣代码，确保您获得最优惠的价格。