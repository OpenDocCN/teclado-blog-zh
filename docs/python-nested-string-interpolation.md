# Python 中的嵌套字符串插值

> 原文：<https://blog.teclado.com/python-nested-string-interpolation/>

在上周的 Python 片段中，我们看了一些使用 Python[格式规范迷你语言](https://docs.python.org/3/library/string.html#format-specification-mini-language)的数字格式化选项。本周我们将继续格式化，但这一次我们将看看嵌套字符串插值。

## 字符串插值基础

就像我们都在同一页上一样，字符串插值是我们把一个字符串放在另一个字符串里面。例如，像这样:

```py
name = "Jose"
print(f"Hello, {name}")  # Hello, Jose 
```

我们也可以使用`format`方法:

```py
name = "Jose"
print("Hello, {}".format(name))  # Hello, Jose 
```

结果是完全一样的，f 字符串只是 Python 3.6 以后的新语法。

在两个版本中，我们都采用字符串`"Hello, {}"`，其中花括号是一个占位符。然后我们插入分配给变量`name`的字符串，替换占位符花括号。

## 嵌套字符串插值

有趣的是，我们实际上可以在这些占位符花括号中执行字符串插值。现在，你到底为什么要这么做？

首先，它允许我们在使用格式规范迷你语言时动态设置格式选项。例如，也许我们正在动态地创建包含编号方案的文件名，并且我们希望我们的程序尽可能地通用。因此，我们可能不想硬编码编号方案。

我们将在这里使用一个简化的例子，其中文件名的其他方面永远不会改变，但是您也可以很容易地替换这些方面。

首先是硬编码版本，这样我们就知道要比较什么了:

```py
number_of_files = 3

for file_number in range(1, number_of_files + 1):
	print(f"image{file_number:03}.png")

# image001.png
# image002.png
# image003.png 
```

现在是嵌套版本:

```py
number_of_files = 3
number_digits = int(input("How many digits are used in the numbering scheme? "))

for file_number in range(1, number_of_files + 1):
	print(f"image{file_number:0{number_digits}}.png") 
```

现在如果用户输入 5，我们将得到这样一个命名方案:`image00001.png`。

我们可以对此进行各种改进，比如为用户输入添加一个`try` / `except`块来捕捉无效值，但这足以演示这个概念。

## 用`format`嵌套字符串插值

我们也可以用`format`方法执行相同类型的嵌套字符串插值。

如果我们使用位置参数，嵌套的占位符被认为是直接跟在它们所在的占位符后面。所以上面的例子使用了位置参数，看起来像这样:

```py
number_of_files = 3
number_digits = int(input("How many digits are used in the numbering scheme? "))

for file_number in range(1, number_of_files + 1):
	print("image{:0{}}.png".format(file_number, number_digits)) 
```

我认为这有点令人困惑，所以如果出于某种原因你必须使用`format`，我会选择这样的关键字参数:

```py
number_of_files = 3
number_digits = int(input("How many digits are used in the numbering scheme? "))

for file_number in range(1, number_of_files + 1):
	print("image{number:0{padding_amount}}.png".format(
		number=file_number,
		padding_amount=number_digits
	)) 
```

这样就不会混淆什么属于哪里。也就是说，在这种情况下，如果可以使用的话，f 弦显然是更干净的选择。

## 包扎

我希望你喜欢这个 Python 片段帖子，并且学到了一些新东西。这是本周的一个小众话题，但这是很多人不知道的事情，在某些情况下肯定非常有用。

和往常一样，如果你想提升你的 Python 技能，我们建议你看看我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)！它带你从完全的初学者，到高级的 Python 概念，所以这是一个真正深入 Python 编程的好方法。

您可能也想注册我们下面的邮件列表，因为我们每个月都会张贴优惠券，这样您就可以在我们所有的课程中获得最优惠的价格。