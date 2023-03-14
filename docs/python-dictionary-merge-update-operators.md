# Python 3.9 中的字典合并和更新运算符

> 原文：<https://blog.teclado.com/python-dictionary-merge-update-operators/>

# 介绍

Python 3.9 于 2020 年 10 月 5 日发布，它引入了一些简洁的功能和优化，包括 PEP 584，内置类`dict`中的 Union 运算符；所谓的字典合并和更新操作符。

在这篇博文中，我们将讨论新的操作符，看看与早期的合并和更新字典的方法相比，使用它们有什么优点或缺点。

# 字典合并运算符

给定两本或更多的字典，我们把它们合并成一本。

让我们从一个简短的例子开始，展示合并两个字典的老方法:

```py
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}

z = {**x, **y}
print(z)
# {'key1': 'value1 from x', 'key2': 'value2 from y', 'key3': 'value3 from y'} 
```

这种合并创建了一个全新的`dict`对象`z`，在这里我们解包来自`x`和`y`的每个值。这种合并两本词典的方式感觉不自然，也不明显。如果两个字典有相同的关键字，字典`x`的值会被字典`y`的值覆盖。

[据圭多:](https://mail.python.org/archives/list/python-ideas@python.org/message/K4IC74IXE23K4KEL7OUFK3VBC62HGGVF/)

> *“我为 PEP 448 感到抱歉，但即使你在更简单的
> 上下文中了解 d，如果你问一个典型的 Python 用户如何将两个 dicts
> 合并成一个新的，我怀疑许多人会想到`{**d1, **d2}`。我知道当这个帖子开始的时候，我
> 自己已经忘记了！如果你问
> 一个学过一些东西(比如序列连接)的新手，他们
> 更有可能会猜 d1+d2。*

下面的例子演示了新的字典合并操作符，`|`:

```py
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}

z = x | y
print(z)
# {'key1': 'value1 from x', 'key2': 'value2 from y', 'key3': 'value3 from y'} 
```

但是请记住，merge 操作符创建新的字典，并保持两个合并的字典不变:

```py
# before merging
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}
print(id(x))  # 2670466407744
print(id(y))  # 2670466407808

# after merging
x | y
print(id(x))  # 2670466407744
print(id(y))  # 2670466407808

# assigning the expression to the variable `z`
z = x | y
print(id(z))  # 2670466542912
print(z is x)  # False
print(z is y)  # False 
```

如果表达式没有赋给变量，它将会丢失。

相同的概念适用于遗留合并方法:

```py
# before merging
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}
print(id(x))  # 2670466407744
print(id(y))  # 2670466407808

# after merging
{**x, **y}
print(id(x))  # 2670466407744
print(id(y))  # 2670466407808

# assigning the expression to the variable `z`
z = {**x, **y}
print(id(z))  # 2670466553600
print(z is x)  # False
print(z is y)  # False 
```

所以我们已经看到它们有相似的行为，使用合并操作符`|`允许我们写更干净的代码，但是除此之外，它有什么好处呢？

这些操作符也已经在其他几个标准库包中实现，比如`collections`。

为了演示合并操作符`|`的用处，让我们看看下面这个使用 [defaultdict](https://blog.teclado.com/python-30-day-26-standard-library/) 的例子:

```py
from collections import defaultdict

user_not_found_message = 'Could not find any user matching the specified user id.'

ceo = defaultdict(
    lambda: user_not_found_message,
    {'id': 1, 'name': 'Jose', 'title': 'Instructor'}
)

author = defaultdict(
      lambda: user_not_found_message,
      {'id': 2, 'name': 'Vlad', 'title': 'Teaching Assistant'}
) 
```

通过使用双星号`**`，可以合并两个字典，但是该方法不知道类对象，所以我们将使用传统的字典:

```py
print({**author, **ceo})
# {'id': 2, 'name': 'Jose', 'title': 'Author', 'title': 'Instructor'}

print({**ceo, **author})
# {'id': 1, 'name': 'Vlad', 'title': 'Teaching Assistant'} 
```

合并操作符`|`的强大之处在于它知道类对象。因此，将返回一个`defaultdict`:

```py
print(author | ceo)
# defaultdict(<function <lambda> at 0x000002212125DE50>, {'id': 2, 'name': 'Jose', 'title': 'Instructor'})

print(ceo | author)
# defaultdict(<function <lambda> at 0x000002212127A3A0>, {'id': 1, 'name': 'Vlad', 'title': 'Teaching Assistant'}) 
```

注意:操作数的顺序非常重要，因为根据它们的排列顺序，它们的行为会有所不同。在上面的例子中，我们使用了两种布局，所以后面的键和值会覆盖前面的。

使用新的字典合并操作符`|`的另一个优点是有遵循这个语法的链式表达式:`dict4 = dict1 | dict2 | dict3`，相当于`dict4 = {**dict1, **dict2, **dict3}`。

让我们展示一个实际的例子:

```py
basic_data = {'id': 1, 'name': 'Vlad'}
get_role = {'title': 'Teaching Assistant'}
details = {'country': 'Denmark', 'active': True}

vlad_info = basic_data | get_role | details
print(vlad_info)
# {'id': 1, 'name': 'Vlad', 'title': 'Teaching Assistant', 'country': 'Denmark', 'active': True} 
```

# 字典更新操作符

在下面的例子中，字典`y`正在更新字典`x`，演示了`.update()`方法:

```py
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}

print(x.update(y))
# None

print(x)
# {'key1': 'value1 from x', 'key2': 'value2 from y', 'key3': 'value3 from y'} 
```

字典`x`被更新，由于内置`.update()`方法设计的本质，它就地操作[。字典得到更新，但是方法返回`None`。](https://blog.teclado.com/python-updating-dictionaries/)

使用**更新操作符**、`|=`，我们可以用更简洁的语法实现相同的功能:

```py
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}

print(x |= y)
# SyntaxError: invalid syntax

print(x)
# {'key1': 'value1 from x', 'key2': 'value2 from x'}

x |= y
print(x)
# {'key1': 'value1 from x', 'key2': 'value2 from y', 'key3': 'value3 from y'} 
```

然而，与传统的`.update()`方法相比，新的字典**更新操作符**有助于防止误用，如果我们在 print 语句中使用它，字典不会得到更新。在下一行中，按照所需的语法，字典被成功更新。

让我们看看如何用遗留的`.update()`方法或者用**更新操作符** `|=`更新字典，既不改变对象的 **id** ，也不创建一个新的:

```py
# before update
x = {"key1": "value1 from x", "key2": "value2 from x"}
y = {"key2": "value2 from y", "key3": "value3 from y"}
print(id(x))  # 2627652603136
print(id(y))  # 2627652603200

# after update
x |= y
print(id(x))  # 2627652603136
print(id(y))  # 2627652603200

# after legacy update
x.update(y)
print(id(x))  # 2627652603136
print(id(y))  # 2627652603200 
```

另一个例子是通过使用**更新操作符** `|=`来扩展具有元组列表的字典:

```py
author = {'id': 1, 'name': 'Vlad'}
author |= [('title', 'Teaching Assistant')]

print(author)
# {'id': 1, 'name': 'Vlad', 'title': 'Teaching Assistant'} 
```

上面的例子是遗留`.update()`方法的语法糖:

```py
author = {'id': 1, 'name': 'Vlad'}
new_key = dict([('title', 'Teaching Assistant')])
author.update(new_key)

print(author)
# {'id': 1, 'name': 'Vlad', 'title': 'Teaching Assistant'} 
```

除了新的字典更新操作符`|=`提供的更好的语法之外，使用它的另一个好处是更安全的字典更新，当在`print`中使用它时，抛出一个`SyntaxError`而不是`None`。

**旧版本**
字典更新操作符`|=`和合并操作符`|`是 Python 3.9 中的新特性，所以如果你试图在早期版本中使用它们，你会遇到类似这样的错误，所以一定要更新到最新版本:

```py
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for |=: 'dict' and 'dict' 
```

### 摘要

新的操作符并不是要取代现有的合并和更新方式，而是对它们进行补充。一些主要的要点是:

*   **合并操作符**，`|`，是类感知的，提供了更好的语法，它创建了一个新的对象。
*   **更新操作符**、`|=`，就地操作[、](https://blog.teclado.com/python-updating-dictionaries/)，在常见错误发生之前捕捉它们，并且不创建新对象。
*   运算符是 Python 3.9 中的新特性

如果你正在学习 Python，并且对这类内容感兴趣，一定要在 [Twitter](https://twitter.com/tecladocode) 上关注我们，或者注册我们的邮件列表，以获得所有最新内容。如果你感兴趣，在这一页的底部有一个表格。

我们也刚刚对我们的[完整 Python 课程](https://www.udemy.com/course/the-complete-python-course/)进行了一次大的更新，所以如果你有兴趣达到 Python 的高级水平，可以去看看。我们有一个 30 天的退款保证，所以你真的没有什么损失，尝试一下。我们很希望你能来！