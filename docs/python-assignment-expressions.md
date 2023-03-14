# Python 中的赋值表达式

> 原文：<https://blog.teclado.com/python-assignment-expressions/>

Python 的新版本刚刚发布(Python 3.8)，这意味着我们[可以和](https://docs.python.org/3/whatsnew/3.8.html)玩一些新的玩具。Python 3.8 中一个有趣的新特性是赋值表达式，以及备受争议的“walrus operator”(`:=`)。在这篇文章中，我们将讨论什么是赋值表达式，以及如何使用它们。

赋值表达式是在 [PEP 572](https://www.python.org/dev/peps/pep-0572/) 中提出的，这是一个*争议极大的*提案。实际上，我认为赋值表达式是对语言的一个很好的补充；然而，在我们展示他们的能力之前，我怎么强调都不为过，他们是真的*容易被滥用。只要格外小心，不要因为使用它们而使代码更难阅读。*

那么，这些大惊小怪的到底是什么？

下面是我们的[完整 Python 课程](https://www.udemy.com/course/the-complete-python-course/)中的一段代码，来自我们练习基本循环和条件循环的练习:

```py
user_input = input('Enter q or p: ')

while user_input != 'q':
    if user_input == 'p':
        print("Hello!")

    user_input = input('Enter q or p: ') 
```

上面的代码相当简单。我们有这样一个 while 循环，它一直要求用户按下 **p** 或 **q** ，如果用户按下了 **p** ，我们就打印这个欢快的小“Hello！”消息。另一方面，如果用户按下 **q** ，循环结束。对于任何其他输入，我们什么都不做。

虽然这段代码没问题，但是我们确实有一些烦人的重复，因为我们必须通过定义`user_input`来开始循环。如果我们不这样做，`user_input`在循环的第一次迭代中是未定义的。

下面是使用赋值表达式的相同代码:

```py
while (user_input := input('Enter q or p: ')) != 'q':
    if user_input == 'p':
        print("Hello!") 
```

正如你所看到的，当我们定义循环条件的时候，我们可爱的小海象就在`user_input`旁边，但是实际上这里发生了很多事情，所以让我们来分解一下。

首先，`:=`实际上是做什么的？它本质上是一种特殊类型的赋值操作符。像往常一样，我们给一个变量名赋值，但是关键的区别是赋值被当作一个表达式。

表达式*计算出某个值*，在赋值表达式的情况下，它们计算出的值是赋值运算符`:=`之后的值。

赋值被当作一个表达式，这就是为什么我们可以直接在它后面使用`!= 'q'`:赋值表达式代表一个实际值。这与我们在 Python 中使用的赋值语句不同，后者根本不表示任何值。

正如您在上面的示例中看到的，我们有一个表达式，它构成了 while 循环的条件，如下所示:

```py
(user_input := input('Enter q or p: ')) != 'q' 
```

它读起来像这样，“`user_input`不等于字符串`'q'`，其中`user_input`是函数调用`input('Enter p or q: ')`的返回值”。

在我们的 while 循环中，这个表达式，包括赋值，在循环的每次迭代中都会被计算。这意味着对于循环的每次迭代，我们将要求用户输入一个值，该值将被赋给变量`user_input`，并与字符串`'q'`进行比较。

由于`user_input`只是一个普通的循环变量，我们仍然可以在循环中使用它，这就是 if 语句没有抱怨的原因。

这里语法的一个重要部分是赋值是用括号括起来的。这是基本上所有可能使用赋值表达式的情况下的一个要求，但是你可以在 PEP 的[这一节中读到更多。](https://www.python.org/dev/peps/pep-0572/#exceptional-cases)

除了删除一些行之外，赋值表达式还可以通过防止重复昂贵的操作来提高程序的效率。我认为这在列表理解这样的结构中最有效:

```py
results = [result for x in data if (result := costly_function(x)) >= 0] 
```

在这里，我们关心的是当我们从某个名为`data`的可迭代对象传递值时，这个`costly_function`生成的值。不再计算两次结果——一次在执行比较时，一次在将值添加到`results`列表时——我们现在只需将结果赋给一个变量并引用该变量。

另一种可能是这样的:

```py
results = []

for x in data:
    result = costly_function(x)

    if result >= 0:
        results.append(result) 
```

这要冗长得多，但是为了我们从传统的 for 循环中获得的那种清晰，这种冗长也许是值得的。这实际上是由你来决定赋值表达式是否足够易读来证明代码长度的减少。

除了他们关于很好地使用语法的建议之外，我真的建议看一看 PEP 中的一些例子。到处放赋值表达式是不值得的，因为这只会损害代码的可读性。这是一个有效但相当小众的工具，我们都需要非常小心。

## 包扎

这就是这篇关于赋值表达式的文章的全部内容！我们很想知道你对新语法的看法，所以请通过 [Twitter](https://twitter.com/tecladocode) 与我们联系，或者参加我们的 [Discord](https://discord.gg/BBWwyMq) 节目。

在不久的将来，我们将在我们的博客中添加大量内容，涵盖 Python 3.8 中有趣的新增内容，所以请注册下面的邮件列表，确保您及时了解我们的所有内容。我们还将在我们的[完整 Python 课程](https://www.udemy.com/course/the-complete-python-course/)中增加新的讲座，敬请关注！