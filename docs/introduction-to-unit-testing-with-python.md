# Python 单元测试简介

> 原文：<https://blog.teclado.com/introduction-to-unit-testing-with-python/>

测试是软件开发的重要部分。主要目的有两个:

1.  好的测试明确地描述了他们测试的东西是做什么的；和
2.  好的测试确保他们测试的东西随着时间的推移符合描述。

例如，假设你有一个函数，你想确保它按照你认为应该的方式工作。类似于这个反转列表的函数([来自我们之前的一篇博文](https://blog.teclado.com/coding-interview-problems-reversing-a-list-python/)):

```
def reverse_list(original: List):
	return original[::-1] 
```

尽管这是一个简单的函数，但您可能会对它提出许多问题:

*   它是否反转一个非零元素数的列表？
*   它是否反转了包含不同类型元素的列表？
*   它对空列表有效吗？
*   它是处理字符串和其他序列，还是只处理列表？
*   它是就地修改列表还是给你一个新的列表？
*   它对有重复条目的列表有效吗？

你可能已经知道了所有这些问题的答案(或者你可以通过启动一个 REPL 并进行测试来轻松找到答案)。但是请记住，测试的目的是描述，并使代码能够适应变化。

如果后来有人来修改这个函数，这些问题的答案可能会改变。您需要知道这种情况是否会发生，因为对其中一些的更改可能会破坏您的整个代码！

我现在要注意的是，在编写测试时考虑更多的问题是非常常见的。我们将为所有的问题编写测试！

## 用 unittest 编写 Python 测试

让我们从编写第一个测试开始。通常我的第一个测试会是一个肯定的测试，这意味着“这个函数做了我期望它做的事情吗？”。

```
from unittest import TestCase
from typing import List

def reverse_list(original: List) -> List:
	return original[::-1]

class ReverseListTest(TestCase):
	def test_reverses_nonempty_list(self):
		original = [3, 7, 1, 10]
		expected = [1, 3, 7, 10]
		self.assertEqual(reverse_list(original), expected) 
```

我们已经编写了第一个测试！

尽可能编写简单的测试很重要。通常在我的测试中，我定义我正在测试的东西的参数(变量`original`),结果`expected`，然后我比较两者。

## 使用 unittest 运行测试

默认情况下，当使用 unittest 时，您的文件应该被称为`test_*.py`。现在，比方说把你的文件叫做`test_reverse.py`。

默认情况下，测试方法的命名也应该遵循这种命名模式。注意，我的测试方法叫做`test_reverses_nonempty_list`。

保存文件后，您可以通过以下方式运行它:

*   如果使用 PyCharm，右击并按“用 unittest 运行测试...”。
*   如果使用终端，输入`python -m unittest test_reverse.py`。

```
python -m unittest test_reverse.py
.
----------------------------------------------------------------------
Ran 1 test in 0.000s 
```

你看到的是你的测试成功了！

## 用测试回答问题

让我们通过更多的测试来回答我们的其他问题:

*   它是否反转了包含不同类型元素的列表？
*   它对空列表有效吗？
*   它是处理字符串和其他序列，还是只处理列表？
*   它是就地修改列表还是给你一个新的列表？
*   它对有重复条目的列表有效吗？

### 它是否反转了包含不同类型元素的列表？

这个问题的答案似乎无关紧要。你可能会想:“我永远不会这么做，所以我为什么要在乎呢？”。

如果你*永远不会*去做这件事，也许你想要的是不要意外地做这件事。我们的函数应该检查元素是否都是相同的类型吗？由您决定应该在您的代码库中发生什么。出于这个例子的目的，让我们这样做，我们不能有混合类型。

```
from unittest import TestCase
from typing import List

def has_mixed_types(list_: List):
    first = type(list_[0])
    return any(not isinstance(t, first) for t in list_)

def reverse_list(original: List) -> List:
	if has_mixed_types(original):
		raise ValueError("The list to be reversed should have homogeneous types.")
	return original[::-1]

class ReverseListTest(TestCase):
	def test_reverses_nonempty_list(self):
		original = [3, 7, 1, 10]
		expected = [10, 1, 7, 3]
		self.assertEqual(reverse_list(original), expected)

	def test_reverses_varying_elements_raises(self):
		original = [3, 7, "a"]
		with self.assertRaises(ValueError):
			reverse_list(original) 
```

请注意，您不需要显式测试“当元素都是相同类型时，它不会引发错误”，因为这是从我们的第一次测试通过后推断出来的。

### 它对空列表有效吗？

这个问题是你可能要决定是否支持的另一个问题。

如果你想在用户试图撤销一个空列表时通知他们，你可以在原始列表的长度为 0 时引发一个错误。现在，我们允许它。

这个测试之所以在这里，是因为如果将来我们决定在一个空列表被传递时引发一个错误，这个测试应该会失败。然后我们会回来修复它，高兴地知道我们的函数以我们期望的方式工作。

```
def test_reverses_empty_list(self):
	original = []
	expected = []
	self.assertEqual(reverse_list(original), expected) 
```

### 它是处理字符串和其他序列，还是只处理列表？

写这个帖子的时候想到的这个问题！

使用切片反转将对字符串和其他序列以及列表起作用。也许这是你想允许的事情——但目前是不允许的。

因为我们的类型提示将`List`指定为参数，所以如果开发人员正在使用类型提示工具，那么应该传递列表或者向他们发出警告(这是应该的！).

如果你想允许其他序列，你可以从`typing`模块中使用`Sequence`而不是`List`。

### 它是就地修改列表还是给你一个新的列表？

这是很重要的一条！如果你调用一个函数，而你不确定它是否会修改链表或者给你一个新的，你肯定会遇到问题。

使用切片的列表反转会给你一个新的列表，但是如果你使用不同的反转方法，它可以在适当的位置修改。

这就是为什么编写测试是重要的:如果有人后来修改了函数，如果新函数修改了列表，我们应该有一个失败的测试。

```
def test_reversal_gives_new_list(self):
	original = [3, 5, 7]
	expected = [3, 5, 7]
	reverse_list(original)
	self.assertEqual(original, expected) 
```

通过检查`original`和`expected`是否相同，我们知道`original`被`reverse_list()`保持不变。

### 它对有重复条目的列表有效吗？

当列表有重复条目时，一些反转列表的方法可能不太适用，因此值得为此添加另一个测试——只要确保即使有重复的元素，反转也能得到预期的值。

```
def test_reversal_with_duplicates(self):
	original = [3, 4, 3, 3, 9]
	expected = [9, 3, 3, 4, 3]
	self.assertEqual(reverse_list(original), expected) 
```

## 更多问题

当你写代码和使用`reverse_list`函数时，你可能会遇到更多你想要回答的问题。

也许你会写一个与这个函数相关的 bug，或者也许你只是碰巧想到一些你想确定的事情。

多写测试！测试就像活的文档，它们帮助您确信您的代码按照您认为的方式工作，即使代码在发展和变化。

## 包扎

最终完整的代码可以在这里看到[。](https://gist.github.com/jslvtr/de28a530f00e7486b51efa3c17f44276)

感谢您的阅读。我希望你在这篇文章中学到了一些东西！

有什么问题吗？给我们发微博。考虑加入我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)以了解更多 Python 的优点，或者我们的[Python 自动化软件测试](https://www.udemy.com/automated-software-testing-with-python/?couponCode=BLOGGER)课程以更深入地关注测试 Python 和 web 应用程序。

如果你想要这些课程的折扣代码，请注册我们下面的邮件列表。每个月我们都与我们的用户分享折扣代码！

周一见，另一个 Python 片段！