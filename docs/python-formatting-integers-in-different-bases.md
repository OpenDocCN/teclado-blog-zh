# 在 Python 中格式化不同基数的整数

> 原文：<https://blog.teclado.com/python-formatting-integers-in-different-bases/>

在最近的几篇 Python 片段文章中，我们已经开始了一场格式化狂欢。首先我们看了使用[格式化规范迷你语言](https://docs.python.org/3/library/string.html#format-specification-mini-language)的数字的一些[特殊格式化选项，然后我们看了](https://blog.teclado.com/python-formatting-numbers-for-printing/)[嵌套字符串插值](https://blog.teclado.com/python-nested-string-interpolation/)。本周我们将在不同的基础上处理打印数字，最后我们将制作一个小小的颜色转换器。

与我们所有的特殊格式化选项一样，我们从一个包含一些用于字符串插值的花括号的字符串开始。我们在花括号里面加了一个冒号，然后选项跟在后面。在我们的例子中，我们只需添加一个字母，而哪个字母取决于我们想用哪个基数来表示我们的数字:

```py
base_10 = 231

print(f"This is the number in binary: {base_10 :b}")
# This is the number in binary: 11100111 
```

在这里，我们在花括号内添加了`:b`，这将我们的`base_10`数字格式化为该数字的二进制表示。

让我们再看几个例子:

```py
base_10 = 231

print(f"This is the number in octal: {base_10 :o}")
# This is the number in octal: 347

print(f"This is the number in hexadecimal: {base_10 :x}")
# This is the number in hexadecimal: e7

print(f"This is the number in uppercase hexadecimal: {base_10 :X}")
# This is the number in uppercase hexadecimal: E7 
```

我们也可以这样将另一种基数的数转换成十进制数:

```py
base_16 = int("E7", base=16)

print(f"This is the number in decimal: {base_16 :d}")
# This is the number in decimal: 231 
```

您可以在这里找到列出各种演示类型的表格:[https://docs . python . org/3/library/string . html # format-specification-mini-language](https://docs.python.org/3/library/string.html#format-specification-mini-language)

## RGB 到十六进制颜色转换器

现在我们已经看了一些选项，让我们使用它们吧！我们将创建一个快速的 RGB 到十六进制的颜色转换器。RGB 值是这样的:`(11, 255, 68)`。第一个值表示颜色中红色的量，第二个值表示绿色的量，第三个值表示蓝色的量。

十六进制颜色以类似的方式工作，但是十六进制颜色不是使用三个逗号分隔的值，而是使用成对的十六进制值，它们一起形成 6 个字符串。通常这串字符前面都有一个`#`。一个例子可能是`#e3f102`，它也可以用大写字母写成`#E3F102`。它们完全一样。再一次，颜色被分解成红色、绿色和蓝色，每一对值代表给定颜色的数量。

为了从 RGB 转换到十六进制，我们必须分别转换每个通道，确保每个十六进制值有两个数字。然后我们必须将所有的值粘在一起，并在前面加上`#`符号。

我们必须注意的一种情况是 RGB 通道值小于 16。因为十六进制是以 16 为基数的，所以小于 16 的数字可以用一个数字来表示。例如，`12`的十六进制值就是`c`。在我们的代码中，我们需要将其表示为`0c`，因此我们将使用 Python 的格式化语言在任何只有一个数字的十六进制值前添加一个零。

其语法是`:02x`，这意味着将数字表示为十六进制(`x`)，用两位数显示数字(`2`)，如果数字少于两位数，则用零填充数字(`0`)。

```py
red = 12
green = 205
blue = 81

hex_red = f"{red:02x}"
# 0c

hex_green = f"{green:02x}"
# cd

hex_blue = f"{blue:02x}"
# 51 
```

这需要大量的代码重复，所以让我们将 RGB 值转换成一个元组，并对其进行迭代以转换每个值。为此我们可以使用[列表理解](https://blog.teclado.com/python-list-comprehensions/):

```py
rgb = (12, 205, 81)
hex_colours = [f"{channel:02x}" for channel in rgb]

# ['0c', 'cd', '51'] 
```

现在我们已经有了十六进制颜色对的列表，我们可以使用`join`方法([参见文档](https://docs.python.org/3/library/stdtypes.html#str.join))将它们连接起来，将它们变成一个连接的字符串。然后我们可以将这个字符串附加到`"#"`上。

```py
rgb = (12, 205, 81)
hex_color = "#" + "".join([f"{channel:02x}" for channel in rgb])

print(f"Converted {rgb} to {hex_color}")
# Converted (12, 205, 81) to #0ccd51 
```

## 包扎

这星期到此为止！我希望你喜欢这个迷你项目，也希望你学到了一些关于格式化数字的新知识。

如果你对提高你的 Python 技能感兴趣，我们很乐意邀请你参加我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)。我们认为这是一个很棒的课程，但我们有 30 天的退款保证，以防课程不适合你。

您也可以注册下面的邮件列表，以便及时了解我们所有的内容，并定期获得我们所有课程的折扣券。