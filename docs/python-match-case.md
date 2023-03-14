# 使用“匹配...Python 3.10 中的“案例”

> 原文：<https://blog.teclado.com/python-match-case/>

这篇文章将向您概述什么是新的“匹配...case”语法在 Python 3.10 中及其用法。然后，我们将更深入地研究高级用法。

“比赛...case”语法类似于其他面向对象语言中的 switch 语句，它旨在使结构与大小写的匹配更加容易。

我们开始吧！

## 语法

“比赛...case "语法如下:

```py
def greeting(message):
	match message.split():
		case ["hello"]:
			print("this message says hello")
		case ["hello", name]:
			print("This message is a personal greeting to {name}")
		case _:
			print("The message didn’t match with anything") 
```

让我们浏览一下语法，看看它是如何工作的。

我们创建的函数接受一个名为`message`的参数。`match`关键字接受一个对象来比较列出的案例。

在我们的例子中，match 关键字接受一个字符串列表，这是`message.split()`操作的结果。为了进一步说明，假设我们这样调用函数:

```py
greeting("hello") 
```

这个函数首先会在所有的空格上分割这个字符串，并生成一个列表。对于上面的输入，`match`操作符将使用`["hello"]`的列表。然后将列表与每个案例进行比较。我们的第一个案例是:

```py
case ["hello"] 
```

我们的输入与此完全匹配，因此代码在这种情况下继续执行。

输出:

```py
this message says hello 
```

如果我们这样调用函数会怎么样:`greeting("hello George")`？

使用该输入，匹配操作符将使用一个列表`["hello", "George"]`来比较所有案例。第一种情况`case "hello"`不匹配，因为比较列表中有两个元素，而不是一个。

### 结构匹配

匹配运算符匹配给定表达式的结构。因此，由于 case 表达式的长度，我们的第一个 case 不匹配，即使比较表达式与列表中的第一个元素匹配。

第二种情况是`["hello", name]`。这是我们的输入匹配的情况。如果您没有给 Python 一个匹配的文字值，它将把比较表达式中的任何值绑定到 case 表达式中的变量名。所以在我们的例子中，`name`将被设置为`George`。并且这个 case 匹配(它有`"hello"`作为第一个元素，还有一个元素被绑定到`name`)，所以输出是:

```py
This message is a personal greeting to George 
```

现在我们试着这样调用函数:`greeting("hello George Johnson")`。

比较表达式变成了`["hello", "George", "Johnson"]`。现在让我们看一下每个案例。第一种情况失败，因为比较表达式中有 3 个元素，而不是 1 个。第二种情况以同样的方式失败；第二种情况希望看到长度为 2 的列表，其中第一个元素是`"hello"`。第一个元素实际上是`"hello"`，但是比较表达式有 3 个元素，所以这种情况不匹配。

剩下的唯一选项是下划线大小写，这是默认的匹配所有内容的大小写。可以把它想象成 switch 语句中的`default`案例。如果一个比较表达式不匹配任何其他东西，它将总是匹配`_`的情况。

### 下划线表示最后一种情况

任何低于此大小写的大小写都不会运行，因为所有大小写都将与下划线大小写匹配。这类似于 if 中的关键字`else`...否则。
`_`案例匹配一切，因为 Python 将`_`识别为有效的变量名。因此，就像我们匹配案例`["hello", name]`时一样，比较表达式将绑定到`_`名称。在我们的具体例子中，`_`变量将保存值`["hello", "George", "Johnson"]`。

因此，在我们最近的函数调用`greeting("hello George Johnson")`中，输出将是:

```py
The message didn’t match with anything 
```

## 高级用法

“比赛...case”语法是一个非常强大的工具，可以用来比较许多不同的表达式和值。如果您像我们在上面的例子中那样比较一个列表，那么您可以使用`match`的更多特性。

在`case`表达式中，您可以使用一个操作符将所有剩余的元素放入一个变量中。例如:

```py
comparison_list = ["one", "two", "three"]

match comparison_list:
    case [first]:
        print("this is the first element: {first}")
    case [first, *rest]:
        print("This is the first: {first}, and this is the rest: {rest}")
    case _:
        print("Nothing was matched") 
```

在这个代码片段中，第二种情况将匹配并执行，输出为:

```py
This is the first: one, and this is the rest: ["two", "three"] 
```

您还可以用两个或更多的结构组成 case 分支，如下所示:

```py
match comparisonList:
    case [first] | [first, "two", "seven"]:
        print("this is the first element: {first}")
    case [title, "hello"] | ["hello", title]:
        print("Welcome esteemed guest {title}")
    case [first, *rest]:
        print("This is the first: {first}, and this is the rest: {rest}")
    case _:
        print("Nothing was matched") 
```

第一个和第二个 case 由几个不同的表达式组成，比较表达式可以适合 case 分支的运行。这给了你一些灵活性来组成你的分支。

我们还将报道“比赛”...字典的语法。匹配运算符将检查比较表达式是否包含 case 表达式中的属性。例如:

```py
comparisonDictionary = {
    "John": "boy",
    "Jack": "boy",
    "Jill": "girl",
    "Taylor": "girl"
}

match comparisonDictionary:
    case {"John": "boy", "Taylor": "boy"}:
        print("John and Taylor are both boys")
    case {"John": "boy", "Taylor": "girl"}:
        print("Taylor is a girl and John is a boy")
    case _:
        print("Nothing matches") 
```

将输出:

```py
Taylor is a girl and John is a boy 
```

`match`操作符将检查 case 属性是否存在于输入字典中，然后检查值是否匹配。

### 重要的

在字典匹配中，`case`仍然匹配，即使输入字典的属性比`case`指定的属性多。

如果你想更深入地了解，请看这里的获取官方文档。

## 结案了。

总之，新的“匹配...case”运算符是 Python 开发人员在创建分支案例时可以利用的强大工具。有了它，您可以可靠地检查任何传入变量的结构，并确保您不会试图访问变量上不存在的内容。

如果你想学习更多关于 Python 的知识，可以考虑参加我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)，它将带你从初级到高级(包括 OOP、web 开发、异步开发等等！).我们有 30 天的退款保证，所以你试一试也不会有什么损失。我们很希望你能来！