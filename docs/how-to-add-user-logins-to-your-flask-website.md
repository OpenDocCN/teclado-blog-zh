# 如何添加用户登录到您的 Flask 网站

> 原文：<https://blog.teclado.com/how-to-add-user-logins-to-your-flask-website/>

在本文中，我们将在 Flask 应用程序中添加基于会话的身份验证。基于会话的认证非常简单和灵活，非常适合使用`render_template`的 Flask 应用程序。这些应用程序最适合我们将在本帖中添加的用户登录类型。

## 先决条件

如果你手头没有 Flask 应用程序，这里有一些你可以使用的启动代码。它是不完整的，因为我们将在本帖中完成它！

链接:[https://github.com/tecladocode/simple-flask-template-app](https://github.com/tecladocode/simple-flask-template-app)

提供的 Flask 应用程序有三个端点，`home`、`login`和`register`。`login`和`register`端点包含表单。我们将使用这些表单从用户那里接收数据。

还定义了一个`app.secret_key`，用于对 cookies 进行加密签名(马上了解更多！).

## 身份验证的工作原理

### 第一个请求

Flask 在每次请求时都会向用户发送一个 cookie。这是自动发生的。用户的浏览器存储 Flask 发送的 cookie 以及发送它的网站的域。然后，每次用户访问该域时，浏览器都会发回 cookie。

例如，假设用户首先通过访问域[http://127 . 0 . 0 . 1:5000/hello](http://127.0.0.1:5000/hello)来访问我们的网站。由于这是用户第一次访问我们的网站，他们的浏览器中没有与我们的网站相关的 cookie，所以他们无法向我们发送 cookie。

Flask 生成一个新的浏览器，并在其中存储这个新浏览器的唯一标识字符串。请记住，一个浏览器可能是一个用户，但也可能是多人在使用同一个浏览器。这个以后会有关系。

当我们的 Flask 应用程序响应时，它将自动附加这个新的 cookie。然后用户的浏览器存储它(只要用户启用了 cookies！).在将来的请求中，用户的浏览器将包含这个 cookie 和其他请求数据。

现在让我们看看任何后续请求会发生什么。

### 后续请求

假设现在用户向[http://127 . 0 . 0 . 1:5000/另一个](http://127.0.0.1:5000/another)发出请求。这一次，用户的浏览器有一个与我们的域相关的 cookie，所以默认情况下它会将它包含在请求中。

当 Flask 收到请求中的 cookie 时，它提取唯一标识发出请求的浏览器的信息。现在 Flask 知道了发出请求的浏览器的一些信息。但是它知道什么呢？嗯，那要看我们想存什么了！

cookies 的这种机制允许 Flask 识别浏览器，对于每个浏览器，它可以存储任意信息。例如，如果用户向我们发送他们的名字，我们可以根据他们的浏览器存储他们的名字，这样如果他们向我们发送 cookie，我们就可以在后续的请求中记住他们的名字。

我们用`secret_key`给饼干签名。这就是我们的 Flask 应用程序可以告诉*它*创建了 cookie，而不是一个不同的应用程序。`secret_key`应该是长的、安全的、秘密的。否则，很容易有人创建一个假的 cookie，并将其输入我们的应用程序。他们可以欺骗我们，让我们以为他们是以另一个用户的身份登录的，然后窃取他们的数据！

针对浏览器存储数据称为“创建会话”。因此，cookie 是浏览器保存的数据，会话是服务器保存的数据。他们是有联系的，但也是分离的。

## 用户认证

通过这种机制，我们将实现用户登录。因为我们可以为每个浏览器存储一些信息，所以我们可以使用这些信息来存储浏览器是否“登录”。

流程是这样的:

1.  用户向我们发送有效的用户名和密码。
2.  我们验证它们是有效的。
3.  如果是，我们将用户的用户名存储在会话中的浏览器中。
4.  当这个浏览器向我们发送请求时，我们可以将其视为“已登录”的浏览器。我们这样做是因为它们包含了适当的 cookie，当我们得到相关的会话时，我们可以看到其中有一个用户名。请记住，只有在他们之前成功登录的情况下，才会有用户名。
5.  如果用户想要注销，我们只需从他们的会话中删除数据。在以后的请求中，我们不会在那里看到用户名，所以我们将浏览器视为“注销”。

概括一下:如果会话包含用户名，我们将该请求视为登录请求。如果没有，我们会将其视为注销请求。

### 实现登录

让我们看看如何实现 Flask 端点来验证(登录)用户。我们必须通过模板中的表单从用户那里接收数据。这将通过一个`POST`请求来完成，因为这是提供的示例代码中表单的方法。

```
@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")

        if username in users and users[username][1] == password:
            session["username"] = username
            return redirect(url_for("home"))
    return render_template("login.jinja2") 
```

我们在这里所做的首先是，允许这个端点接收`GET`和`POST`请求。

然后，如果我们正在处理一个`POST`请求，我们知道我们正在处理表单数据。我们将尝试从表单数据中获取用户名和密码。

如果用户名是我们的用户之一，并且他们的密码是正确的，那么我们将把他们的用户名存储在会话中的`session["username"]`下。

然后，我们将它们重定向到主页，如果用户名在会话中，我们将显示用户名(如果不在会话中，则显示`"Unknown"`):

```
@app.route("/")
def home():
    return render_template("home.html", name=session.get("username", "Unknown")) 
```

在任何其他端点中，您可以检查用户是否登录，有可能将他们重定向到他们可以登录的另一个端点。

正常的流程是检查`session`是否包含用户名。如果是，那么用户被登录，您可以允许您的端点做它的事情。如果没有，那么您可以将它们重定向到登录端点(以与上面相同的方式)。

### 实现寄存器

注册用户非常简单。根据他们给我们的用户名和密码数据，我们只需检查用户名是否已经存在。然后我们将它添加到字典中:

```
@app.route("/register", methods=["GET", "POST"])
def register():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")

        if username not in users:
            users[username] = (username, password)
    return render_template("register.html") 
```

注意，`login`和`register`端点都缺少一些关键的错误处理。当用户的用户名或密码不正确时，以及当他们试图注册一个已经存在的用户名时，告诉他们总是好的。目前我们的代码两者都没有。

### 实施注销

要注销用户，我们只需清除会话—从会话中删除用户名意味着后续请求将被视为“注销”。请注意，浏览器仍然有 cookie，Flask 仍然有会话。它只会是空的。

```
@app.route("/logout")
def logout():
    session.clear()
    return redirect(url_for("home")) 
```

### 最终代码

请看一下我们带有用户认证的简单应用程序的最终完整代码。

我们已经实现了登录、注销和用户注册。

链接:[https://github . com/tecladocode/simple-flask-template-app/tree/feature/log in](https://github.com/tecladocode/simple-flask-template-app/tree/feature/login)

## 在不渲染模板的 Flask 应用程序上

这种身份验证方法只适用于既提供内容又处理用户数据的应用程序。它不能在用 JavaScript 开发的网络应用上运行。这是因为这种基于会话的身份验证要求 Python 代码可以在每次请求或页面更改时读取针对用户存储的数据。

对于使用 JavaScript 的 web 应用程序，您可能希望使用基于令牌的身份验证。我们在[这篇博文](https://blog.teclado.com/simple-jwt-authentication-with-flask-jwt/)中谈到了这一点。

想了解关于基于会话的身份验证的更多信息，包括如何拥有不同类型的用户和访问级别？查看[这篇博文](https://blog.teclado.com/learn-python-defining-user-access-roles-in-flask/)！

## Flask 的验证扩展

Flask 有一些扩展可以帮你做很多这方面的工作。然而，在使用它们之前，理解这一切在幕后是如何工作的是很重要的。现在您已经知道了它是如何工作的，您可以考虑使用一个扩展。

[Flask-Login](https://flask-login.readthedocs.io/en/latest/) 就是这样一个扩展，它可以为你做以下事情:

*   将活动用户的 ID 存储在会话中，让您可以轻松地登录和注销他们。
*   允许您将视图限制为登录(或注销)的用户。
*   处理通常棘手的“记住我”功能。
*   帮助保护您的用户会话不被 cookie 窃贼窃取。

如果您想了解更多信息，我建议您阅读官方文档。

## 包扎

如果你正在学习 Python，并且对这类内容感兴趣，一定要在 [Twitter](https://twitter.com/TecladoCode) 上关注我们，或者注册我们的邮件列表，以获得所有最新内容。如果你感兴趣，在这一页的底部有一个表格。

我们还有一个[完整的 Python Web 课程](https://www.udemy.com/the-complete-python-web-course-learn-by-building-8-apps?couponCode=BLOGGER)用于更深入的培训和教程，以及多个项目的演练。我们有一个 30 天的退款保证，所以你真的没有什么损失，尝试一下。我们很希望你能来！

另外，注册我们下面的邮件列表。我们将每月向我们的订户发送折扣代码！