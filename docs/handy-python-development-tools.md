# 便捷的 Python 开发工具

> 原文：<https://blog.teclado.com/handy-python-development-tools/>

linter 用来捕捉与我们使用语言相关的问题。例如，如果我们忘记添加冒号(`:`)，或者如果我们忘记导入我们需要的东西。一个短绒会让我们知道这些事情。

使用 Python3 安装和使用 linter 非常简单，只需:

```py
pip install pylint 
```

当您这样做时，您可以运行它以确保您的代码符合默认样式。

要运行它，只需使用终端导航到包含您的代码的文件夹，然后运行`pylint`:

```py
pylint myfile.py 
```

然后，Pylint 会告诉您它在您的文件中发现的任何问题。

例如，对于如下所示的文件:

```py
 Mailgun.send(['[[email protected]](/cdn-cgi/l/email-protection)'], 'Test e-mail', 'This is a test e-mail') 
```

皮林特会说:

```py
No config file found, using default configuration
************* Module using_mailgun_lib
C:  1, 0: Missing module docstring (missing-docstring)
E:  2, 0: Undefined variable 'Mailgun' (undefined-variable)

-----------------------------------------------------------------------
Your code has been rated at -50.00/10 (previous run: -40.00/10, -10.00) 
```

这是一些相当糟糕的代码！它的得分是-50，因为它甚至不会运行——因为我们有一个未定义的变量。

如果你加上它说缺少的东西:

```py
"""
Sample code on how to use our new Mailgun library to
reduce code duplication.

This library sends an e-mail to '[[email protected]](/cdn-cgi/l/email-protection)', with
subject 'Test e-mail' and content 'This is a test e-mail'.
"""
from libs.mailgun import Mailgun

Mailgun.send(['[[email protected]](/cdn-cgi/l/email-protection)'], 'Test e-mail', 'This is a test e-mail') 
```

然后它高兴了，说:

```py
No config file found, using default configuration

--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00) 
```

现在，你可能不总是想包含模块文档字符串(我知道我不想！)，所以很容易禁用该检查:

```py
pylint --disable=missing-docstring using_mailgun_lib.py 
```

这将禁用“checker”`missing-docstring`，它是检查模块和函数中的文档字符串的工具。

您可以对一个文件夹中的所有文件运行 pylint，只需给出该文件夹的路径，例如:

```py
pylint my_module 
```

## 格式化我们的代码

通常情况下，我们在编码时会尽量遵循特定的风格。风格不一定会给我们的代码带来问题——这正是 Pylint 所发现的——但是在整个项目中坚持一致的风格仍然是一件好事。

例如，Python 社区的大部分人更喜欢这段代码:

```py
x = {'a': 37, 'b': 42, 'c': 927}

y = 'hello ' 'world'
z = 'hello ' + 'world'
a = 'hello {}'.format('world')

class foo(object):
    def f(self):
        return 37 * -+2

    def g(self, x, y=42):
        return y

def f(a):
    return 37 + -+a[42 - x:y**3] 
```

关于这段代码:

```py
x = {  'a':37,'b':42,

'c':927}

y = 'hello ''world'
z = 'hello '+'world'
a = 'hello {}'.format('world')
class foo  (     object  ):
  def f    (self   ):
    return       37*-+2
  def g(self, x,y=42):
      return y
def f  (   a ) :
  return      37+-+a[42-x :  y**3] 
```

当然，这有点夸张——我愿意认为你们中没有人编写过这样的代码——但尽管如此，不一致的间距、换行符、引号等等可以很容易地通过使用格式化程序来保持一致。

Python 有几个格式化程序，但我想到了其中两个:

`yapf`是一个由来已久的 Python 格式化程序，由来已久；而 Black 是一种新的 Python 格式化程序，正在受到越来越多的关注。您可以随意使用这两种方法，但是在本指南中，我们将坚持使用长期使用的方法:`yapf`。

要安装`yapf`，只需通过`pip`就像我们为`pylint`做的那样:

```py
pip install yapf 
```

然后，您可以对文件运行格式化程序，方法是使用终端导航到该文件包含的文件夹，并执行以下操作:

```py
yapf myfile.py 
```

这会告诉你`yapf`想做什么——但不会改变你的文件并重新格式化。如果你想改变你的文件，你可以这样做:

```py
yapf -i myfile.py 
```

更多信息，请查看[官方自述](https://github.com/google/yapf)。

* * *

我希望这篇关于 Python 开发工具的短文有所帮助！我总是建议运行一个棉绒。运行格式化程序也是一个好主意，这样项目中的每个人都将遵循相同的风格。然而，格式化程序也会让你变得有点懒惰，不在乎代码的风格。

我总是试图自己做好每一件事，然后运行格式化程序，这样整个项目都是一致的。