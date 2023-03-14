# Python 中的字符串格式

> 原文：<https://blog.teclado.com/learn-python-string-formatting-in-python/>

对于新程序员来说，Python 中最令人困惑的事情之一是字符串格式化。不是因为它在 Python 中很难，而是因为这么多年来做这件事的方式已经改变了很多次。

下面是在 Python 中进行字符串格式化的反向时间表。建议您遵循第一种格式化方法。

所有的代码片段都要求用户输入他们的名字(例如`"Jose"`)，然后打印出一个问候语(`"Hello, Jose!"`)。

## f 弦

最现代的字符串格式化方法是 f-strings，Python3.6 中新增的。这是我推荐的字符串格式化方法！

```
user_name = input('Enter your name: ')  # Ask user for their name
greeting = f'Hello, {user_name}!'  # Construct a greeting phrase
print(greeting) 
```

f-string 将用作用域中的变量替换花括号内的内容。因此，变量中的`{user_name}`将被变量`user_name`的值替换。

## `.format()`

`.format()`方法是一种很好的字符串格式化方法，但是它几乎总是可以被 f 字符串代替。

这里有一个例子:

```
user_name = input('Enter your name: ')
greeting = 'Hello, {}!'.format(user_name)
print(greeting) 
```

在`.format()`方法中，我们将括号中的值替换为字符串中的`{}`。

## 模板

模板允许您创建可重复使用的字符串，其中特殊的占位符可以由值替换。它们比任何其他形式的格式化都要慢得多，而且与上述两种相比，几乎没有任何好处。

这里有一个例子:

```
from string import Template
user_name = input('Enter your name: ')
greeting_template = Template('Hello, $who!')
greeting = s.substitute(who=user_name)
print(greeting) 
```

## 串并置

字符串连接是 Python 中最简单也是最古老的字符串格式化形式。当你有非常简单的字符串，很少格式化时，这是很有用的。

这里有一个例子:

```
user_name = input('Enter your name: ')
greeting = 'Hello, ' + user_name + '!'
print(greeting) 
```

然而，当将不是字符串的东西连接在一起时，很容易在 Python 代码中产生意外错误，正如我们从下面的示例中看到的:

```
age = 30
greeting = 'You are ' + age + ' years old.'  # This raises an error!
print(greeting) 
```

这是对 Python 中现有的字符串格式化方式的一个快速、简短的概述。有很多不同的方法！

许多不同的教程可能会建议不同的字符串格式化方法，因为它们可能是在 Python 生命的不同时期编写的。我的建议是:如果在 Python3.6 上，坚持使用 f 字符串，如果在 Python3.5 或更早的版本上，坚持使用`.format`方法。