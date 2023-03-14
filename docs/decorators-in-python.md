# 如何用 Python 写 decorators

> 原文：<https://blog.teclado.com/decorators-in-python/>

Python 中的 Decorators 可能是一个令人困惑的话题，尤其是对于新生来说。这就是为什么我们制作了一个由四部分组成的视频课程供您浏览，链接如下。

我还整理了一些重要概念的书面解释，所以如果你是一个读者，或者如果你只是想要更多的例子，你会在下面找到你需要的所有信息。

## 高阶函数

在学习 decorators 之前，你必须对高阶函数有所了解。毕竟所有的 decorator 都是高阶函数(但不是所有的高阶函数都是 decorator！).

那么什么是高阶函数呢？

它们只是将其他函数作为参数的函数。高阶函数通常调用传递给它们的函数，但它们通常也涉及额外的操作。

例如，假设您有一个名为`say_hello`的函数:

```py
def say_hello():
    user_name = input("Enter your name: ")
    print(f"Hello, {user_name}!") 
```

假设您想要创建另一个函数来运行这个函数，但是在运行之前还会打印一条欢迎消息。

您可以这样做:

```py
def welcome_user():
    print("Welcome to my application!")
    say_hello() 
```

这不是高阶函数。它只是一个函数！

如果您希望能够在任何任意函数之前打印欢迎消息，那么您应该创建一个接受另一个函数作为参数的高阶函数。像这样:

```py
def welcome_user(func):
    print("Welcome to my application!")
    func() 
```

这样做的好处是，现在任何函数都可以传递给`welcome_user`，所以非常具有重用性。这个特殊的例子可能看起来不太有用，但是我们会在这篇文章的后面看到一个更有趣的例子！

你应该这样使用它:

```py
def welcome_user(func):
    print("Welcome to my application!")
    func()

def say_hello():
    user_name = input("Enter your name: ")
    print(f"Hello, {user_name}!")

welcome_user(say_hello)  # Calls both functions and prints both things. 
```

## 什么是装修工？

装饰器是不直接调用参数的高阶函数；相反，它会创建另一个函数，并返回该函数。

一句话就这么多“功能”，我们来分解一下吧！

这是一个室内设计师可能的样子:

```py
def decorator():
    def say_goodbye():
        print("Goodbye!")

    return say_goodbye 
```

`decorator`函数定义一个内部函数`say_goodbye`，然后返回它。内部函数在被调用之前不会运行。你可以这样使用它:

```py
my_variable = decorator()
my_variable()  # Goodbye! 
```

虽然看起来`my_variable`不是函数(那里没有`def`关键字！)，`my_variable`很大程度上是一种功能。

请记住，在赋值操作发生之前，赋值的右边是先计算的。这样，`decorator`、`say_goodbye`函数的返回值被分配给`my_variable`。

那么，`decorator`是装修工吗？

还没有。

为了成为装饰器，它需要将一个函数作为参数:

```py
def decorator(func):
    def say_goodbye():
        func()
        print("Goodbye!")

    return say_goodbye 
```

然后你可以像这样使用这个装饰器:

```py
def say_hello():
    print("Hello!")

greet = decorator(say_hello)
greet()  # Hello! and Goodbye! 
```

现在`decorator`名副其实了。它将一个函数作为参数，并返回一个本质上“扩展”它的新函数。新的`greet`函数不再仅仅调用原来的函数，而是做了更多的*。*

*通常，您会使用 decorators 来替换现有的函数，同时扩展它们:*

```py
*`def say_hello():
    print("Hello!")

say_hello = decorator(say_hello)
say_hello()  # Hello! and Goodbye!`* 
```

 *在最后一个例子中，`decorator`函数首先运行，因为它在赋值的右边。最初的`say_hello`函数将只在装饰器内部定义的`say_goodbye`函数中被调用。

在第 4 行之后，全局范围内的`say_hello`变量不再引用原始函数。相反，它引用了`say_goodbye`函数。

您可以通过打印出`__name__`来检查这一点:

```py
def say_hello():
    print("Hello!")

say_hello = decorator(say_hello)
print(say_hello.__name__)  # say_goodbye 
```

这是这些不完整装饰器的一个缺点，因为旧函数的`__name__`和`__doc__`属性被新函数的属性所取代。在一天结束时，我们用一个新值完全替换了变量，所以这是有意义的。

然而，当使用 decorators 时，我们常常希望扩展`say_hello`函数，但让它保留自己的名字和文档(如果有的话)。这就是内置模块`functools`的用武之地。

如果这样做，该函数将保留其原始名称和文档:

```py
import functools

def decorator(func):
    @functools.wraps(func)
    def say_goodbye():
        func()
        print("Goodbye!")

    return say_goodbye 
```

这意味着该名称将被保留:

```py
def say_hello():
    print("Hello!")

say_hello = decorator(say_hello)
print(say_hello.__name__)  # say_hello 
```

但是，等等！那个`@`语法是什么？

好吧，那就来了解一下吧！

## 装饰者的`@`语法

`@`语法用于应用装饰器。当我们之前编写`@functools.wraps(func)`时，我们将`wraps`装饰器应用到了我们的`say_goodbye`函数中。

我们也可以在代码中使用这种语法。目前，我们的代码如下所示:

```py
import functools

def decorator(func):
    @functools.wraps(func)
    def say_goodbye():
        func()
        print("Goodbye!")

    return say_goodbye

def say_hello():
    print("Hello!")

say_hello = decorator(say_hello)
say_hello() 
```

但是使用`@`语法，可以简化成这样:

```py
import functools

def decorator(func):
    @functools.wraps(func)
    def say_goodbye():
        func()
        print("Goodbye!")

    return say_goodbye

@decorator
def say_hello():
    print("Hello!")

say_hello() 
```

注意，我们已经去掉了重新赋值`say_hello`的那一行，现在我们把`@decorator`放在了函数定义的上面。

这完全一样:它创建函数，然后将它传递给装饰器，并重新分配它。

如果您运行代码，您会看到调用`say_hello()`仍然输出“Hello！”还有“再见！”。

这样做的一个主要好处是函数会立即被修饰，所以在它被修饰之前不会被意外调用。

## 用参数修饰函数

此时，我们的`say_hello`函数被替换为`say_goodbye`。这意味着，如果`say_hello`有任何参数，当我们替换函数时，这些参数会丢失。请看这个例子:

```py
import functools

def decorator(func):
    @functools.wraps(func)
    def say_goodbye():
        func()
        print("Goodbye!")
    return say_goodbye

@decorator
def say_hello(name):
    print(f"Hello, {name}!")

say_hello("Bob")  # TypeError, say_goodbye() takes 0 positional arguments but 1 was given 
```

事实上，如果我们要这样做，我们还需要将参数添加到`say_goodbye`:

```py
def decorator(func):
    @functools.wraps(func)
    def say_goodbye(name):
        func(name)
        print("Goodbye!")
    return say_goodbye 
```

但是这是一个坏主意，因为它将我们的装饰器耦合到了我们的`say_hello`函数，这意味着我们不能将该装饰器与具有不同数量参数的其他函数一起使用。

相反，推荐的方法是通过让我们的装饰器接受任意数量的参数，并将参数(如果有的话)传递给被装饰的函数，从而对我们的装饰器进行一般化。

为了做到这一点，我们将使用标准语法`*args, **kwargs`。如果你以前没有遇到过他们，[这里有一个快速指南](https://www.digitalocean.com/community/tutorials/how-to-use-args-and-kwargs-in-python-3):

```py
def decorator(func):
    @functools.wraps(func)
    def say_goodbye(*args, **kwargs):
        func(*args, **kwargs)
        print("Goodbye!")
    return say_goodbye 
```

现在这个装饰器可以用于任何函数，不管它们有多少参数！

## 用参数编写装饰器

为了了解带参数的 decorators，我们来看另一个例子。这个例子也包含在这篇文章顶部的视频中，所以如果你还没有看的话，一定要看看！

假设您有一个函数`get_admin_password`，它将密码返回给管理面板，还有另一个函数`get_user_password`，它将密码返回给用户仪表板。它们看起来是这样的:

```py
def get_admin_password():
    return "1234"

def get_user_password():
    return "secure_password" 
```

您可以像这样调用这些函数之一:

```py
print(get_admin_password())  # 1234 
```

但是自然地，您希望只有管理员能够获得管理员密码，用户能够获得用户面板密码。

这是您的用户的样子:

```py
user = {"name": "Bob Smith", "access_level": "admin"} 
```

因此，让我们编写一个装饰器来保护`get_password`函数，这样它就不会暴露给任何用户了！

我们将从只允许管理员运行该功能开始:

```py
def make_secure(func):
    def secure_func(*args, **kwargs):
        if user["access_level"] == "admin":
            return func(*args, **kwargs)

    return secure_func 
```

一个简单的装饰器，但是使用了我们到目前为止讨论过的所有东西！

它定义了一个内部函数，只有当用户的访问级别是“admin”时，这个函数才会运行传递的函数`func`。

下面是完整的代码:

```py
import functools

user = {"name": "Bob Smith", "access_level": "admin"}

def make_secure(func):
    @functools.wraps(func)
    def secure_func(*args, **kwargs):
        if user["access_level"] == "admin":
            return func(*args, **kwargs)

    return secure_func

@make_secure
def get_admin_password():
    return "1234"

@make_secure
def get_user_password():
    return "secure_password"

print(get_password("admin"))  # 1234
print(get_password("user"))  # None 
```

为什么当我们试图获取用户面板密码时，它会打印出`None`？因为装饰器从不运行`return func(*args, **kwargs)`，所以默认情况下它只返回`None`——就像所有 Python 函数一样。

目前，`make_secure`装饰器只允许用户在访问级别为`"admin"`时运行该函数。它不允许用户运行任何功能。

事实上，我们需要两个略有不同的装饰器，一个检查`"admin"`级别的权限，一个检查`"user"`级别的权限。

但是如果我们只定义两个装饰器，会有很多重复，所以我们要做的是通过添加一个参数使它们更动态:

```py
@make_secure("admin")
def get_admin_password():
    return "1234"

@make_secure("user")
def get_user_password():
    return "secure_password" 
```

如果我们要这样做，我们需要对装饰器本身做一些改变！

关于这个新语法，*非常*重要的一点是，`make_secure("admin")`将首先运行，然后它将作为装饰器与`@`语法一起应用。

所以我们的装饰者必须变成这样:

```py
def make_secure(access_level):
    def decorator(func):
        @functools.wraps(func)
        def secure_func(*args, **kwargs):
            if user["access_level"] == "admin":
                return func(*args, **kwargs)

        return secure_func
    return decorator 
```

我们增加了一个级别，因为`make_secure`不再是装饰者*本身*。现在是一个函数创建一个装饰器，然后*将*应用于带有‘at’语法的函数。

所以当你运行`@make_secure("admin")`时，首先会执行`make_secure`函数并创建`decorator`函数。

然后应用,`@decorator`,但是它已经知道您提供的访问级别。

这非常令人困惑，是 Python 中最复杂的代码之一。我们很少在函数之间有三层深度嵌套！

我建议查看视频播放列表以获得另一种解释，示例略有不同。这种结构越看越有道理，越容易理解！

## 包扎

如果你有兴趣提高你的 Python 技能，并学习更多的 Python 主题，我们很乐意邀请你参加我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)。这是一门全面的课程，超过 30 个小时，涵盖了所有主要的 Python 主题！我们认为这是一个很棒的课程，但如果它不适合你，我们也有 30 天的退款保证。

您也可以注册下面的邮件列表，因为我们每个月都会向我们的订户发送折扣代码！

希望在那里见到你！*