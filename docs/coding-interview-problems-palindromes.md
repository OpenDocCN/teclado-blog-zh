# 编码面试问题:回文

> 原文：<https://blog.teclado.com/coding-interview-problems-palindromes/>

我们今天带着另一个常见的编码面试问题回来。这周我们要写一些代码来检查一个字符串是否是回文。回文是一个单词或句子，它的前后读起来是一样的，比如名字“Hannah”，或者句子“Never 奇数或偶数”。

## 解决问题

这个问题的复杂程度取决于我们是必须检查一个单词，还是还必须检查整个句子。在后一种情况下，我们必须去掉标点和空格，因为我们实际上只关心字母。我们将从这个问题的简单版本开始，然后我们将讨论完整的句子。

对于问题的两个版本，我们必须记住的一点是，大写和小写字符在技术上是不一样的，所以我们必须小心确保我们检查字母的大小写。

为此，我们可以使用`casefold`方法([参见文档](https://docs.python.org/3/library/stdtypes.html#str.casefold))。在这篇文章中，我们将假设所有的字符串都使用基本的拉丁字符，但是有办法扩展`casefold`来说明其他符号。我推荐看一下[这个 StackOverflow 回答](https://stackoverflow.com/a/29247821)了解更多细节。

现在我们已经有了这些警示，我们该如何解决这个问题呢？和所有类似的问题一样，有很多解决的方法。

我们可以选择的一个方法是将字符串转换成列表。我们想要这样做的原因是 Python 列表有一个内置的`reverse`方法。然后，我们可以使用`==`用这个新的反向版本对原始字符串进行比较。

在这个解决方案中需要注意的是，`reverse`对列表执行就地转换，所以我们要么需要复制一份列表进行比较，要么需要使用`join`将列表转换回字符串。

我们有一个更加手动的选项，即`for`循环。字符串本身是可迭代的，所以我们可以一次检查一个字符和另一个字符。

我们可以利用枚举函数(细节可以在[这里](https://blog.teclado.com/python-enumerate/)找到)在字符串中的每个字符旁边创建一个计数器，从 1 开始。然后，我们可以使用这个计数器通过负索引从字符串的另一端访问字符。例如，索引`-1`处的字符是字符串中的最后一个字符；索引`-2`处的项目是倒数第二个字符，依此类推。

我们要看的最后一个选项是使用切片。我们有几个关于切片的帖子，所以如果你不熟悉切片是如何工作的，我建议你看看下面的帖子:

[https://blog.tecladocode.com/python-slices/](https://blog.teclado.com/python-slices/)

[https://blog.tecladocode.com/python-slices-part-2/](https://blog.teclado.com/python-slices-part-2/)

## 使用列表和`reverse`

我们将从名字开始，汉娜。我们知道 Hannah 实际上是一个回文，它还包含一个大写字母，这意味着我们可以测试我们的 case folding 是否如预期的那样工作。我们还会用到第二个字符串，彼得，它不是回文。这只是为了测试我们的实现不会产生错误的结果。

既然我们要检查多个字符串，我们应该从定义一个函数开始。我们知道我们必须传入一个字符串进行测试，所以我们将创建一个`test_string`参数。

```
def check_if_palindrome(test_string):
	pass 
```

我们的第一步应该是将`test_string`转换成一个列表，我们称之为`characters`。然后我们可以在这个新列表上调用`reverse`方法。

```
def check_if_palindrome(test_string):
	characters = list(test_string)
	characters.reverse() 
```

然而，在我们创建这个新的`characters`列表之前，我们必须利用`casefold`来删除字符串中任何可能导致我们的比较出现问题的大写字母。

```
def check_if_palindrome(test_string):
	characters = list(test_string.casefold())
	characters.reverse() 
```

现在我们有了反转的字符，我们可以使用`join`将列表转换回字符串，然后我们可以将新的反转字符串与大小写折叠的原始字符串进行比较:

```
def check_if_palindrome(test_string):
	characters = list(test_string.casefold())
	characters.reverse()

	if "".join(characters) == test_string.casefold():
		print(f"{test_string} is a palindrome."
	else:
		print(f"{test_string} is not a palindrome." 
```

如果我们用`"Hannah"`和`"Peter"`测试我们的函数，我们可以看到一切都按预期工作:

```
def check_if_palindrome(test_string):
	characters = list(test_string.casefold())
	characters.reverse()

	if "".join(characters) == test_string.casefold():
		print(f"{test_string} is a palindrome.")
	else:
		print(f"{test_string} is not a palindrome.")

check_if_palindrome("Hannah")  # Hannah is a palindrome.
check_if_palindrome("Peter")   # Peter is not a palindrome. 
```

## 使用`for`回路和`enumerate`

再一次，我们将从定义一个单参数的函数开始。然而，这一次，我们不需要对列表做任何事情，因为我们将直接遍历字符串。

```
def check_if_palindrome(test_string):
	pass 
```

我们的`for`循环将利用`enumerate`，它将为`test_string`中的每个字符创建一个元组，包含一个计数器和当前字符。这些元组被收集在一个`enumerate`对象中，这就是我们的`for`循环将要迭代的对象。

因此，对于每次迭代，我们都将获得一个元组，我们可以将它解包为两个变量。我们在最近关于 Python 中的析构的文章中谈到了这一点。

默认情况下，`enumerate`的计数器从`0`开始，但是我们希望它从`1`开始，因为我们将使用这个值来访问使用负索引的字符。集合中的最后一项位于索引`-1`，而不是`-0`。我们可以通过将`start`参数值设置为`1`来正确配置计数器。

就像以前一样，我们还必须确保从字符串中删除任何大写字母，以便我们比较所有大小写相同的字符:

```
def check_if_palindrome(test_string):
	for index, letter in enumerate(test_string.casefold(), start=1):
		pass 
```

在 for 循环中，我们将使用一个`if`语句来执行一个测试，我们将一次比较一个字符。字符串的第一个字符将与字符串的最后一个字符进行比较，我们将在每次迭代中沿着字符串移动。

我们实际上要测试字符串是否是**而不是**一个回文，一会儿就有意义了。如果我们发现一个不匹配的字符，我们将立即执行`break`，因为没有必要检查任何其他字符。我们已经知道字符串不是回文。

```
def check_if_palindrome(test_string):
	for index, letter in enumerate(test_string.casefold(), start=1):
		if letter != test_string[-index].casefold():
			print(f"{test_string} is not a palindrome.")
			break 
```

那么，我们为什么要这样做呢？原因是我们需要匹配每一个字符，然后才能说某个东西实际上是一个回文。例如，仅仅匹配第一个字符是不够的。

如果我们执行匹配字符的检查，而不是不匹配的字符，我们必须跟踪所有字符都匹配的事实，这是我们不想做的大量家务。

相反，我们可以使用一个`for` / `else`结构。我们[以前已经写过这个](https://blog.teclado.com/python-more-uses-for-else/)，所以看看这对你来说是不是新的。

如果我们发现一个不匹配的字符，我们立即`break`循环，所以如果`for`循环运行完成，这意味着所有的字符都匹配。附加到`for`循环的`else`子句只有在循环完成而没有遇到`break`语句或异常时才会运行。

因此，我们可以把我们的“...是回文。”此`else`子句中的消息。

```
def check_if_palindrome(test_string):
	for index, letter in enumerate(test_string.casefold(), start=1):
		if letter != test_string[-index].casefold():
			print(f"{test_string} is not a palindrome.")
			break
	else:
		print(f"{test_string} is a palindrome."

check_if_palindrome("Hannah")  # Hannah is a palindrome.
check_if_palindrome("Peter")   # Peter is not a palindrome. 
```

非常重要的是，`else`关键字与`for`循环定义内联。它是**而不是`if`街区的**部分。

## 部分

在所有这些选项中，可能最优雅的解决方案是使用切片。再说一次，如果你不熟悉切片，请阅读之前链接的帖子。它们是一种令人敬畏的、多功能的工具，你会发现它们在任何地方都有用途。

我们的切片解决方案的关键是一点相当神秘的语法:`[::-1]`。它的作用是将整个序列颠倒过来。一般来说，切片最大的优点是它们不会修改原始集合。这让我们的生活*非常*轻松。

我们的解决方案本质上只是一个单独的`if`语句，其中我们比较两个大小写折叠字符串。

```
def check_if_palindrome(test_string):
	if test_string.casefold() == test_string[::-1].casefold():
		print(f"{test_string} is a palindrome.")
	else:
		print(f"{test_string} is not a palindrome.")

check_if_palindrome("Hannah")  # Hannah is a palindrome.
check_if_palindrome("Peter")   # Peter is not a palindrome. 
```

## 处理句子

既然我们已经介绍了许多解决单个单词的方法，让我们来考虑如何处理完整的句子。正如前面提到的，我们需要去掉所有的标点和空格，因为我们只关心字母本身。

当[在段落](https://blog.teclado.com/coding-interview-problems-find-the-longest-word-in-a-paragraph/)中寻找最长的单词时，在一个字符串中寻找所有的字母字符是我们之前在编码面试问题系列中处理过的事情。我建议你读一读这篇文章，因为我们涉及了很多类似这样的问题。

为了避免遗漏一些标点符号，当试图查找字符串中的所有字母字符时，正则表达式是我们的最佳选择。

Python 有一个处理正则表达式的模块叫做`re`，所以我们需要将它作为解决方案的一部分导入。具体来说，我们关注一个名为`sub`的函数，它允许我们用另一个字符串替换一个模式。在我们的例子中，我们将用一个空字符串`""`替换所有非字母字符。

现在让我们从函数定义开始:

```
def check_if_palindrome(test_string):
	pass 
```

首先，我们将使用`casefold`方法删除`test_string`中的所有大写字母。然后，我们可以将这个新的小写字符串传递给我们的`sub`函数。

`sub`函数将允许我们过滤掉`test_string`中的所有非字母字符。我们将使用的正则表达式模式是`[^a-z]+`。这看起来很神秘，但是它的意思是匹配任意数量的不是字母`a`到`z`的字符。注意，因为我们已经对`test_string`进行了装箱，所以我们不需要检查大写版本。

```
import re

def check_if_palindrome(test_string):
	letters_only = re.sub("[^a-z]+", "", test_string.casefold())

	print(letters_only) 
```

如果我们传入一个充满标点符号的字符串，我们将得到一个完全小写的字符串，只包含字母字符。

```
check_if_palindrome("NeVeR! - oDd! - Or! - eVEn!")
# neveroddoreven 
```

现在，我们可以利用我们的任何一个单词解决方案，只需稍加修改。我们已经对所有文本进行了大小写折叠，因此没有必要继续这样做:

```
import re

def check_if_palindrome(test_string):
	letters_only = re.sub("[^a-z]+", "", test_string.casefold())

	if letters_only == letters_only[::-1]:
		print(f"'{test_string}' is a palindrome.")
	else:
		print(f"'{test_string}' is not a palindrome.")

check_if_palindrome("Never odd or even.")
# 'Never odd or even.' is a palindrome.

check_if_palindrome("Python is awesome!")
# 'Python is awesome!' is not a palindrome. 
```

这就是全部了！

## 包扎

实际上，我认为这是一个非常有趣的问题，尤其是对于完整的句子回文。希望你学会了如何处理这种问题，以及很快很酷的新技能，我真的希望你在下一次编码面试中大放异彩！

如果你真的想提升你的 Python 技能，你应该试试我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)！我们真的很想在球场上看到你！

我们也有一个表格在下面，用于注册我们的邮件列表。我们为我们的课程张贴定期折扣代码，所以这是一个伟大的方式来确保您总是得到最好的交易。