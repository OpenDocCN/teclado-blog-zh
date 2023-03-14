# 编码面试:找到最长的单词

> 原文：<https://blog.teclado.com/coding-interview-problems-find-the-longest-word-in-a-paragraph/>

在本帖中，我们将讨论另一个常见的编码面试问题:找出一段文字中最长的单词。这是一个很好的问题，因为我们很容易忘记一些重要的细节，而且显而易见的解决方案不一定是最好的。

## 解决问题

对于这个特殊的问题，我们将想象我们已经得到了下面的字符串 *lorem ipsum* 文本。为了节省空间，我们假设这个字符串被用作名为`base_string`的变量的值。

> Lorem ipsum 疼痛静坐 amet，结果肥胖精英。卢图斯·伊普斯姆阁下和轻松体面。马蒂斯来了投票箱。半兽人不见了，他妈的身份证不见了。Integer 会得到 nibh vel mattis elementum。Nam 是精英，sodales vitae augue nec，nibh 的四个载体。阿利奇乌斯车，爱琴海盟渴，爱琴海上。Fusce 会生气的。病态的骚扰激起了对 vel ullamcorper 的仇恨。Donec 的仇恨。他妈的尼采，他妈的罗伦姆车队和。aenean pretium 我告诉 dapibus。Ut id 没有妓女，有限公司想要，vestibulum enim。唐克康茂德杜伊洛斯，不会抓到半兽人的。S7-1200 可编程控制器。都督摆姿势，李欧不穿染色剂，望吐沫横放仇恨，渴欲吐野巴士，精英伏侍。悲伤的维塔奎利奥。

*Lorem ipsum* 只是排版印刷中用来模拟实际文本内容的一种常见的虚设文本。它包括各种长度的单词，以及标点符号，所以它非常适合我们的目的。

那么，我们如何找到最长的单词呢？

由于我们的段落目前是一个单独的字符串，我们可能需要找到一种方法将它分解成单独的单词。这将让我们在不同的单词之间进行直接比较，让我们发现一个给定的单词是否比另一个更长。

有几种方法可以进行实际的比较。

我们可以创建一个名为类似于设置为空字符串的`longest_word`的变量，然后迭代所有单词，将当前`longest_word`的长度与我们在迭代中检查的单词进行比较。如果新单词更长，我们可以用新单词替换`longest_word`的值。对段落中的每个单词重复这个过程，我们将得到段落中最长的单词，它被绑定到`longest_word`变量。

另一个选择是我们使用`max`函数，它接受一个 iterable，并从集合中返回一个条目。它将返回集合中最大的一个成员，但是我们必须为`max`提供一些配置，以便它知道给定上下文意味着什么。

这两种解决方案都有一个潜在的小问题，即它们只返回一个单词。如果两个单词长度相同呢？我们是提供这些最长单词中的一个作为解决方案，还是提供所有最长单词的集合？如果我们必须提供所有最长的单词，那么我们将需要使用不同的方法。

现在，让我们来看看这个问题的第一部分代码。这些解决方案存在一个问题，我稍后会谈到这个问题。

## 我们的第一个解决方案

Python 包含一个名为`split`的方便的字符串方法。你可以在[官方文件](https://docs.python.org/3.7/library/stdtypes.html#str.split)中读到关于`split`的内容。

`split`将允许我们使用一个分隔符字符串将一个字符串分成一系列条目。每当在字符串中遇到这个分隔符，`split`就会创建一个断点。这些断点之间的所有内容都将成为结果列表中的一个项目。

因此，我们可以这样做:

```
word_list = base_string.split(" ")
# ["Lorem", "ipsum", "dolor", "sit", "amet,", ... etc.] 
```

眼尖的读者可能已经注意到了一个问题。`amet`后的逗号包含在相应的列表项中。所有有标点符号的单词都是如此，这就造成了一个严重的问题。

我们的文本实际上有两个最长的单词，但其中一个在句尾，因此有一个附加的句号。包括标点符号在内，这个单词错误地成为段落中唯一最长的单词。

如果我们有像引号这样的标点符号，问题可能会更严重，因为包含两个标点符号可能会使一个单词比真正最长的单词更长，尽管实际上可能是一个更短的字符。随着标点符号的复合，情况只会变得更糟。

很明显，我们必须对这些多余的标点符号做些什么。

我们可以利用另一个叫做`strip`的字符串方法来解决这个问题。同样，你可以在[官方文件](https://docs.python.org/3.7/library/stdtypes.html#str.strip)中找到关于`strip`如何工作的细节。

`strip`允许我们从字符串的任意一端删除字符，它会一直删除这些字符，直到遇到一个与它被指示删除的字符不匹配的字符。默认情况下，它删除空白字符，但我们可以提供一个标点符号的集合。

使用[列表理解](https://blog.teclado.com/python-list-comprehensions/)，我们可以迭代`word_list`并纠正标点符号问题:

```
word_list = base_string.split(" ")
processed_words = [word.strip(".,?!\"\':;(){}[]") for word in word_list] 
```

这对于我们的示例字符串来说是绝对可行的，但这也不是一个很好的解决方案。首先，不得不使用这些转义字符是丑陋的，但更重要的是，这组标点符号是不完整的，要使其完整是非常繁琐的。

现在我们不做任何事情来过滤掉例如波浪符号(`~`)，或者百分号，或者散列符号。到目前为止，还有一个我们根本没有考虑的问题:数字。数字很容易超过我们最长的单词，给我们错误的结果。

与其删除我们不想要的东西，也许我们可以走另一条路，只添加我们想要的东西。输入 RegEx。

## 使用正则表达式

正则表达式是一种方法，通过它我们可以定义在字符串中查找的模式。正则表达式是一个非常宽泛的主题，当然太大了，无法在这里涵盖，但是在文档中有一个关于在 Python [中使用正则表达式的教程。](https://docs.python.org/3/howto/regex.html)

为了使用 Python 中的正则表达式，我们需要导入`re`模块，所以这是我们的第一步:

```
import re 
```

接下来，我们将获取我们的`base_string`变量，包含我们的 *lorem ipsum* 文本，并作为参数提供给我们在`re`模块中找到的`findall`函数。与`base_string`一起，我们还将提供一个使用 RegEx 语法的模式，如下所示:

```
import re

word_list = re.findall("[A-Za-z]+", base_string) 
```

RegEx 是出了名的神秘，但是我们这里的模式相对简单。它说我们在寻找基本拉丁字母表中的任何字母字符。我们不关心大小写；我们希望模式包含任意数量的这些字符，如符号`+`所示。

我们得到的`word_list`变量现在将包含一个全是字母字符串的列表。在遍历我们的字符串时，任何时候遇到不属于我们定义的模式的字符，`findall`都认为是单词的结尾。

如果你问我，我会说这是非常好的东西。

## 另一个小问题

不幸的是，现在庆祝还为时尚早。我们当前的实现又有一个问题:单词中的标点符号。

考虑这样一种情况:

> 约翰的狗还没有吃它的食物。

我们可能会提出一个很好的论点，即“约翰的”和“还没有”是单个词，在这种情况下，我们有一个问题。现在，如果我们对这个句子运行代码，我们会得到以下结果:

```
import re

base_string = "John's dog hasn't eaten its food."
word_list = re.findall("[A-Za-z]+", base_string)

# ['John', 's', 'dog', 'hasn', 't', 'eaten', 'its', 'food'] 
```

这可能完全没问题，但是如果你的面试官要求你把“约翰的”当成一个单词会怎么样呢？在这种情况下，“约翰的”是一个带有标点符号的 5 个字母的单词。

最好的答案可能是结合我们的方法。我们可以根据空格分割字符串，然后检查每个单词的标点符号。然而，这一次，我们将使用另一个名为`sub`的`re`函数执行替换，而不是拆分字符串。

`sub`接受一个模式、该模式的替换字符串和要搜索的字符串。

```
import re

base_string = "John's dog hasn't eaten its food."
word_list = base_string.split(" ")
processed_words = [re.sub("[^A-Za-z]+", "", word) for word in word_list]

# ['Johns', 'dog', 'hasnt', 'eaten', 'its', 'food'] 
```

这里我们的模式和以前完全一样，除了我们添加了一个特殊的字符，`^`，它本质上是说匹配任何不在这个模式中的东西。因为我们的模式只包括基本的拉丁字母，所以所有的标点都将与这个模式匹配。我们的替换字符串是一个空字符串，这意味着只要我们找到匹配，匹配的字符就会被删除。

将此放入列表理解中，我们可以对`word_list`中的每个单词进行替换，给出一个经过适当处理的单词列表。

这样，我们终于可以开始计算单词的长度，并找到段落中最长的单词。

## 寻找最长的单词

正如我之前提到的，我们有几个选项来查找单词列表中最长的单词。

让我们从字符串替换方法开始，使用我们原来的段落:

```
import re

word_list = base_string.split(" ")
processed_words = [re.sub("[^A-Za-z]+", "", word) for word in word_list]

longest_word = ""
for word in processed_words:
	if len(longest_word) < len(word):
		longest_word = word

print(longest_word)  # consectetur 
```

在这个实现中，我们从一个空字符串开始，并开始遍历`processed_words`中的所有单词。如果当前的`word`比当前的`longest_word`长，我们替换最长的字。否则，什么都不会发生。

由于我们从一个空字符串开始，第一个单词最初会成为最长的字符串，但一旦有更长的单词单独出现，它就会被取代。

我们可以利用`max`函数来代替这种相当手工的方法。

```
import re

word_list = base_string.split(" ")
processed_words = [re.sub("[^A-Za-z]+", "", word) for word in word_list]

longest_word = max(processed_words, key=len)

print(longest_word)  # consectetur 
```

在这个方法中，我们必须传入`processed_words`作为第一个参数，但是我们还必须提供一个`key`。通过提供`len`函数作为关键字，`max`将使用不同单词的长度来确定哪个单词的值最大。

这两种方法都只提供一个单词，那么如果我们有多个长度相等的单词，我们如何获得所有最长的单词呢？

## 查找最长单词的列表

为了找到所有最长的单词，现在我们知道最长的单词实际上是什么，我们可以对我们的`processed_words`进行第二次遍历。我们可以用一个条件列表理解来做到这一点。

```
import re

word_list = base_string.split(" ")
processed_words = [re.sub("[^A-Za-z]+", "", word) for word in word_list]

max_word_length = len(max(processed_words, key=len))
longest_words = [word for word in processed_words if len(word) == max_word_length]

print(longest_words)  # ['consectetur', 'ullamcorper'] 
```

## 包扎

这样，我们就成功地找到了一个段落中最长的单词。当然，这不是解决这个问题的唯一方法，具体的问题也可能需要一些小的变化。你可能会被要求找出段落中最长单词的长度，但如果你能做到这里显示的版本，这个问题应该是轻而易举的。

我们希望看到你自己对这个问题的解决方案，所以请登录 [repl.it](https://repl.it/) 并在 [Twitter](https://twitter.com/TecladoCode) 上与我们分享你的解决方案。如果你喜欢这篇文章，如果你能和你的技术朋友分享，我们会非常感激。

我们将在周一再次见到你，届时我们将发布[第二部分](https://blog.teclado.com/python-itertools-part-2-combinations-permutations/)的`itertools`模块简介！如果你迫不及待，你可能想看看我们的[完整的 Python 课程](https://go.tecla.do/complete-python-sale)，在那里我们对这些主题进行了更深入的探讨。