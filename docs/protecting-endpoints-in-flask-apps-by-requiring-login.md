# 通过要求登录来保护 Flask 应用中的端点

> 原文：<https://blog.teclado.com/protecting-endpoints-in-flask-apps-by-requiring-login/>

在我们上一篇关于 Flask 的博文中，我们讨论了如何在 Flask 网站上添加登录和注册。我们通过添加一个表单，从用户那里接收用户名和密码，然后确保它们存在于我们的数据库中。

当用户成功登录时，我们将他们的电子邮件保存到一个会话中。当我们从用户那里收到一个合适的 cookie 时，我们就能够检索他们的会话。如果那里有一封电子邮件，我们知道用户以前登录过——因为这是电子邮件可能存在的唯一方式。

如果你没有看过[之前的帖子](https://blog.teclado.com/how-to-add-user-logins-to-your-flask-website/)，我绝对推荐！

## 保护端点

“保护终端”意味着确保终端只能被登录用户访问。为此，在端点中执行任何代码之前，必须检查用户是否已登录。

例如，让我们以这个端点为例:

```py
@app.route("/profile")
def profile():
    return render_template("profile.html", name=session["username"]) 
```

这里我们的代码正在执行`session["username"]`，所以如果用户名不在会话中，这段代码将会失败。用户必须登录才能执行这个端点，但目前我们不检查。

让我们添加一个检查并保护这个端点:

```py
@app.route("/profile")
def profile():
    if "email" not in session:
        return redirect(url_for("login"))
    return render_template("profile.html", name=session["username"]) 
```

我们添加了两行代码，如果会话中没有电子邮件，它们会将用户发送到登录页面。这是一个很容易做到的检查，但是如果您有许多端点需要保护，那么您最终会在许多端点中拥有相同的代码。

随着代码的增长，编写装饰器会有所帮助。

## 用装饰器保护端点

装饰器是扩展另一个函数的函数。它允许我们在运行一个函数之前或之后运行一些代码，在我们的例子中，我们将在运行实际的端点函数之前使用一个装饰器来运行电子邮件检查。

装饰器将像这样使用:

```py
@app.route("/profile")
@login_required
def profile():
    return render_template("profile.html", name=session["username"]) 
```

那个`@login_required`是我们应用到`profile`端点的装饰器。让我们来学习如何编写这个装饰器。

为了定义装饰器，我们编写一个函数，它将另一个函数作为参数。这里有一个装饰器，它根本不修改函数。

```py
def login_required(func):
    return func 
```

当我们键入`@login_required`时，Python 会立即将装饰器下面的函数传递给`login_required`函数。在我们的代码中，`profile`函数将被传递给装饰器。装饰器然后做的是用装饰器的返回值替换`profile`函数。

所以我们希望我们的装饰器返回一个函数，它将:

1.  检查包含电子邮件的会话；
2.  如果有，调用原始函数；
3.  如果没有，将用户重定向到登录页面。

大概是这样的:

```py
def login_required(func):
    def secure_function():
        if "email" not in session:
            return redirect(url_for("login"))
        return func()

    return secure_function 
```

这里我们的装饰器将一个函数作为参数。然后，它定义了另一个名为`secure_function`的函数，并在`secure_function`内部执行电子邮件检查。如果在`session`中没有找到电子邮件，它会将用户重定向到`login`端点。否则，它调用原始函数(也就是`profile`函数！).

最后它返回`secure_function`，这就是修饰后的函数。

同样，请记住此处应用了装饰器:

```py
@app.route("/profile")
@login_required
def profile():
    return render_template("profile.html", name=session["username"]) 
```

因此，最终结果是，只有当电子邮件在会话中时，才会调用我们的`profile`函数。

### 保留函数名

Flask 在某些事情上使用函数名，比如在使用`url_for`时，所以当我们在不止一个端点中使用这个装饰器时，我们会发现问题——因为所有被装饰的端点都将被`secure_function`函数替换。

我们希望确保当我们用这个新函数替换一个函数时，我们保留了原始函数的名称。

这是必需的，因为即使不使用`url_for`，Flask 也要求端点中使用的函数名都是唯一的。

我们将使用一个名为`functools`的内置模块来保留原来的函数名称，具体来说就是函数`wraps`:

```py
import functools

def login_required(func):
    @functools.wraps(func)
    def secure_function():
        if "email" not in session:
            return redirect(url_for("login"))
        return func()

    return secure_function 
```

`wraps`函数修改了`secure_function`，并对其进行了重新命名，将其命名为`func`。

### 允许参数

既然我们保留了原来的函数名，我们也应该保留原来的函数参数。

我们的一些端点可能有参数(目前没有，但将来可能会有)。因此，在修饰函数中考虑参数是一个很好的做法。

为了让我们的`secure_function`有任意数量的参数，我们可以使用`*args`和`**kwargs`。如果你不熟悉这些，[这里有一个很好的解释](https://www.digitalocean.com/community/tutorials/how-to-use-args-and-kwargs-in-python-3)它们的意思！

```py
import functools

def login_required(func):
    @functools.wraps(func)
    def secure_function(*args, **kwargs):
        if "email" not in session:
            return redirect(url_for("login"))
        return func(*args, **kwargs)

    return secure_function 
```

这里我们允许任意数量的位置参数(`*args`)和任意数量的关键字参数(`**kwargs`)。这意味着当我们的端点函数被`secure_function`替换时，它仍然可以像往常一样接受参数。

### 跟踪所请求的端点

当我们用这个装饰器装饰一个函数时，我们可能最终会把用户发送到登录页面，而不是他们请求的端点。

对于我们的应用程序来说，记住用户想去的地方是一个很好的实践，这样当他们登录时，我们可以继续将他们发送到那里，而不是默认情况下`login`端点将他们发送到哪里。

例如，如果用户请求他们的“仪表板”,我们应该在他们登录后将他们发送到那里——而不是他们的个人资料！

我们可以做的是，当我们将用户发送到`login`端点时，也在 URL 中包含他们想要访问应用程序的哪一部分:

```py
def login_required(func):
    @functools.wraps(func)
    def secure_function(*args, **kwargs):
        if "email" not in session:
            return redirect(url_for("login", next=request.url))
        return func(*args, **kwargs)

    return secure_function 
```

在这里，`next=request.url`将一个查询字符串参数添加到我们将用户发送到的 URL 中——因此，我们不会将它们发送到像`http://127.0.0.1:5000/login`这样的地方，而是将它们发送到`http://127.0.0.1:5000/login?next=/dashboard`。有了这些信息，我们就可以在他们成功登录后对我们的应用程序进行一些修改。

然而，我们不会在这篇博文中学习如何做到这一点——这篇博文太长了！密切关注未来的博客帖子，了解我们如何处理那里的`next`参数，并将用户发送到他们最初的预期目的地！或者，注册我们的电子邮件列表(页面底部有一个表格)或在 Twitter 上关注我们[，我们会在有新博客发布时通知您。](https://twitter.com/TecladoCode)

## 使用装饰器

既然我们已经定义了我们的装饰器，使用它就相当简单了！

在您想要保护的任何端点上(即，确保用户在访问它之前已经登录)，只需将装饰器放在端点定义之上。

只要确保装修工在`@app.route...`线下就行了！

```py
@app.route("/profile")
@login_required
def profile():
    return render_template("profile.html", name=session["username"]) 
```

## 包扎

我希望你喜欢这篇博文。我们现在正在做一些非常高级的 Python，这些东西会很快变得令人困惑！如果你以前没有使用过装饰者，一定要花时间去理解他们是如何工作的。它们在很多情况下都会派上用场！

如果你想了解更多关于使用 Flask 进行 web 开发的知识，请查看我们的[完整 Python Web 课程](https://www.udemy.com/the-complete-python-web-course-learn-by-building-8-apps/)。如果您是 Python 的新手，并且想要更完整地理解基础知识(包括装饰器！)，查看我们的[完整 Python 课程](https://udemy.com/the-complete-python-course/)。此外，请注册下面的邮件列表，因为我们每个月都会与我们的订户共享折扣代码！

感谢阅读，我们下次再见！