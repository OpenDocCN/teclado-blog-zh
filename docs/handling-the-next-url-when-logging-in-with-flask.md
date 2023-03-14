# 使用 Flask 登录时处理下一个 URL

> 原文：<https://blog.teclado.com/handling-the-next-url-when-logging-in-with-flask/>

当一个注销的用户想要访问一个受保护的页面时，我们通常会将他们重定向到应用程序中的登录页面。然而，在登录后，许多应用程序会将用户发送到他们的“个人资料”或“仪表板”页面，而不是他们最初想去的地方。

这可能非常令人沮丧，所以让我们学习如何在使用我们的页面时为用户提供更好的体验。

## 先决条件

对于这篇博文，您需要一个具有登录功能的 Flask 应用程序，但是它还不能正确处理这个流。

此外，你应该阅读我们之前的系列博客文章“通过要求登录保护 Flask 应用中的端点”。在那里，我们介绍了如何编写装饰器，我们将在这篇文章中修改它。

如果你手边没有样品瓶应用程序，我为你编写了一个。你可以在这里访问它[。为了测试这个流，启动应用程序并:](https://github.com/tecladocode/simple-flask-template-app/tree/feature/login_decorator)

1.  访问`/profile`端点。此端点已被配置为需要登录，并将您重定向到登录页面。
2.  使用测试用户详细信息`jose`和`1234`登录。
3.  注意，现在你在`/`结束，而不是在`/profile`结束。这就是我们在这篇博文中要解决的问题。

## 告诉登录端点接下来将用户发送到哪里

为了让我们的应用程序能够将用户重定向到正确的地方，`/login`端点必须知道那个地方是什么。因此，当用户登录时，我们需要能够告诉它他们想要在哪里结束。

类似地，为了让`/login`页面知道用户想要到达哪里， *it* 必须被告知最终目的地是哪里。

因此，数据流将是这样的:

1.  用户想要访问`/profile`。这是受保护的，所以我们将用户重定向到`/login`页面以及最终目的地。
2.  用户登录，因此他们提交登录表单，在提交中我们包括最终目的地。
3.  我们在 Python 代码中处理登录，不是将用户重定向到默认目的地，而是将他们重定向到他们想要的目的地。

让我们从做第一步开始:告诉`/login`页面最终目的地是哪里。为此，我们将修改`login_required`装饰器。

此刻，装饰者看起来像这样:

```
def login_required(func):
    @functools.wraps(func)
    def secure_function(*args, **kwargs):
        if "email" not in session:
            return redirect(url_for("login"))
        return func(*args, **kwargs)

    return secure_function 
```

但是我们想添加一段数据发送到`/login`页面:

```
def login_required(func):
    @functools.wraps(func)
    def secure_function(*args, **kwargs):
        if "email" not in session:
            return redirect(url_for("login", next=request.url))
        return func(*args, **kwargs)

    return secure_function 
```

这意味着当我们将用户重定向到登录页面时，我们会将他们发送到`/login?next=/profile`，而不是发送到`/login`。因为我们将这些数据包含在 URL 中，这意味着页面本身可以在呈现时访问这些信息。

## 当用户登录时传递该信息

但是当用户提交表单时，我们的 Python 代码将不再能够访问`?next=/profile`。我们必须将该信息传递到表单中。

唯一的方法是在表单中包含一个字段，将最终目的地作为文本。这有点粗糙，但是我们可以通过在表单中添加一个隐藏字段来做到这一点，而不会干扰用户流。

让我们修改我们的登录表单，并将该字段包含在其中:

```
<input
    type="hidden"
    name="next"
    value="{{ request.args.get('next', '') }}"
/> 
```

它从查询字符串参数中读取`next`参数，并将其值包含在一个隐藏字段中。当我们提交表单时，我们将能够在 Python 代码中访问这个字段，并且能够使用它的值来重定向用户。

注意该字段不需要标签，因为它是隐藏的，所以用户在页面上看不到它。

## 使用隐藏字段重定向用户

最后，当我们收到隐藏字段值时，我们必须使用它将用户重定向到他们最终的目的地。

请记住，如果用户没有尝试访问某个特定的站点，这个字段可能为空，所以我们只希望在这个字段有值的情况下将用户重定向到这个位置——否则我们仍然会重定向到默认的目的地(目前是`/`)。

目前，我们的登录处理程序代码如下所示:

```
@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")

        if username in users and users[username][1] == password:
            session["username"] = username
            return redirect(url_for("profile"))
    return render_template("login.html") 
```

我们将把它改成这样:

```
@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")
        next_url = request.form.get("next")

        if username in users and users[username][1] == password:
            session["username"] = username
            if next_url:
                return redirect(next_url)
            return redirect(url_for("profile"))
    return render_template("login.html") 
```

这样做的目的是读取隐藏字段的内容，并使用它来重定向用户。

注意，我们不需要用`next_url`调用`url_for`，因为它已经是我们应用程序中的一个 URL，而不是一个端点处理程序。

## 概述

概括地说，为了将用户重定向到他们的预期目的地，而不是默认的“登录”页面，我们必须:

1.  将最终目的地传递给登录页面。
2.  从那里，将最终目的地传递给登录处理程序。
3.  使用它来重定向用户，而不是将他们发送到默认的目标页面。

尽管如此，这似乎是显而易见的——但要记住每个端点都是完全独立的，每当我们想要进行这种类型的通信时，我们都需要通过用户的浏览器传递数据，这一点总是很棘手。

如果你想在这篇博文之后查看最终的完整代码，请使用[这个链接](https://github.com/tecladocode/simple-flask-template-app/tree/feature/login_next)！

如果你想阅读以前关于这个主题的帖子，这些帖子涉及 Flask web 应用程序开发的更多基础元素，请在这里阅读:

## 包扎

感谢您的阅读！我希望你觉得这篇文章有趣并且有用。

如果你想了解更多关于使用 Flask 进行 web 开发的知识，请查看我们的[完整 Python Web 课程](https://www.udemy.com/the-complete-python-web-course-learn-by-building-8-apps/?couponCode=BLOGGER)。如果您是 Python 的新手，并且想要更完整地理解基础知识(包括装饰器！)，查看我们的[完整 Python 课程](https://udemy.com/the-complete-python-course/?couponCode=BLOGGER)。

您可能也想注册下面的邮件列表，因为我们每个月都会向我们的订户发送博客文章更新和折扣代码！

感谢阅读，我们下次再见！