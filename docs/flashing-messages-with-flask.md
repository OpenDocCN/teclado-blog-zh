# 用烧瓶闪烁信息

> 原文：<https://blog.teclado.com/flashing-messages-with-flask/>

通常在网站和网络应用程序中，我们想在发生什么事情时临时向用户显示一条消息。以下是一些信息示例:

*   尝试登录时，他们的用户名或密码不正确。
*   他们寻找着什么，但没有找到结果。
*   他们试图购买，但由于某种原因失败了。

你能明白我的意思。在许多情况下，我们的应用程序中会发生这样或那样的事情，我们必须通知用户。这些通常被称为警报、错误或消息。

因为它非常常见，所以 Flask 附带了简化显示和隐藏这些消息的过程的功能。这叫“闪”。

我已经编写了一个小 Flask 应用程序，向您展示我们将在这篇博文中学到的东西。如果你想用它来编码，或者只是检查最终的代码，请访问这个 GitHub 库。

## 如何闪现消息

为了刷新消息，您必须有一个 Flask 端点，并且`flash`导入:

```
from flask import flash, render_template

...

@app.route("/")
def home():
    flash("This is a flashed message.")
    return render_template("home.html") 
```

令许多人惊讶的是，调用`flash()`并不会让你的消息显示在你的模板上。它只是将消息添加到**消息存储库**。

一旦消息被闪现，它就被添加到存储中并保存在那里，直到被消费。

在您的模板中，您必须决定是否使用消息库。

但是请记住，当您这样做时，您将消耗整个消息库。这意味着，如果你多次调用`flash()`，所有这些消息都会在模板中显示给你。

让我们来看看可以使用消息存储库的模板:

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    {% for message in get_flashed_messages() %}
        <p>{{ message }}</p>
    {% endfor %}
  </body>
</html> 
```

现在你的网站只会显示文本`"This is a flashed message."`。

### 闪烁多条信息

如果您决定不止一次打电话给`flash()`:

```
@app.route("/")
def home():
    flash("This is a flashed message.")
    flash("Another message.")
    return render_template("home.html") 
```

那么你的模板会将这两条消息显示为单独的段落。同样，当您在模板中调用`get_flashed_messages()`时，将检索存储中的所有消息。然后，它们会从消息存储中删除。

## 示例场景:登录错误

假设我们有下面的代码，它允许用户登录:

```
@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form.get("username")
        password = request.form.get("password")
        user = find_user(username)

        if user and password == user.password:
            return redirect("profile")
        else:
            flash("incorrect username or password.")
    return render_template("login.html") 
```

让你想象填充`find_user`函数，想象它从数据库中检索用户数据。如果你有兴趣了解如何在你的 Flask 应用中添加登录和注册功能，我们有另一篇博文会涉及到这个问题。

您可以在代码中看到，如果用户不存在，或者密码不正确，我们将显示一条消息。之后，我们渲染`login.html`模板。

这意味着如果我们想要显示消息，我们的`login.html`模板应该能够处理闪烁的消息。

```
<!DOCTYPE html>
<html>
  <head>
    <style>
      .alert-error {
        padding: 8px;
        background-color: red;
        color: white;
      }
    </style>
  </head>
  <body>
    <h1>Flashing messages</h1>
    {% for message in get_flashed_messages() %}
      <div class="alert-error">Error: {{ message }}</div>
    {% endfor %}
  </body>
</html> 
```

记住，如果`login.html`模板没有使用消息存储库，但是向其中添加了消息，那么这些消息将被保存在存储库中，直到另一个模板使用它。

## 带类别的闪烁邮件

有时，我们可能希望闪现不同类型的信息。例如，显示“未找到结果”的消息可能被归类为信息性消息，而显示“用户名或密码不正确”的消息可能被称为错误。

你可能也想在你的网站上设计不同的风格，所以区分它们是很重要的。

这就是为什么`flash()`有一个可选的第二个参数:`category`。

可以这样做:`flash("No results found", "info")`。

然后，在您的模板中，您可以通过使用`with_categories=True`参数来访问类别。注意，类别是返回的元组的第一个元素，后面是消息:

```
{% for category, message in get_flashed_messages(with_categories=True) %}
  <div class="alert-{{category}}">Error: {{ message }}</div>
{% endfor %} 
```

但是我们也做了一些偷偷摸摸的事情，我们将类别传递给了 HTML 类:`alert-{{category}}`而不是我们之前的类别`alert-error`。

这意味着在我们的 CSS 中，我们现在可以这样:

```
.alert-error {
  background-color: red;
  color: white;
}

.alert-info {
  background-color: yellow;
  color: black;
} 
```

这大大简化了为不同类别的消息显示不同类型的警报。

## 样式代码改进

因为这是一篇简短的博文，所以我也给你一个提示，让你在处理警报时更好地构建你的样式代码。

如果你想给你的提醒添加更多的样式，你可以这样做:

```
.alert-error {
  padding: 12px;
  border-radius: 3px;
  font-size: 1.2rem;
  margin-bottom: 16px;
  border: 2px solid darkred;
  background-color: red;
  color: white;
}

.alert-info {
  padding: 12px;
  border-radius: 3px;
  font-size: 1.2rem;
  margin-bottom: 16px;
  border: 2px solid orange;
  background-color: yellow;
  color: black;
} 
```

但是请注意，虽然警告有不同的颜色，但是它们也有许多重复的样式。

在 CSS 中，您通常可以将它提取到另一个类中，并在 HTML 中应用两个类，而不是一个。这样做会更好:

```
.alert {
  padding: 12px;
  border-radius: 3px;
  font-size: 1.2rem;
  margin-bottom: 16px;
  border-width: 2px;
  border-style: solid;
}

.alert-error {
  border-color: darkred;
  background-color: red;
  color: white;
}

.alert-info {
  border-color: orange;
  background-color: yellow;
  color: black;
} 
```

然后，在 HTML 中，您将对元素应用`alert`和适当的 alert 类类型:

```
{% for category, message in get_flashed_messages(with_categories=True) %}
  <div class="alert alert-{{category}}">Error: {{ message }}</div>
{% endfor %} 
```

这减少了 CSS 代码中的重复，也使这个类的用途更加清晰。`alert`类定义了所有警报的属性，`alert-*`类定义了特定类型警报的属性。

一个额外的好处是，现在如果你想添加另一个样式的所有警报，你只需要在一个地方这样做。

你可以在 CSS 中的更多地方应用它，而不仅仅是这里，所以如果你的 CSS 代码中有重复，看看你是否能把重复提取到一个共享类中！

## 包扎

这就是 Flask 中闪烁消息的全部内容！这很简单，但是让我们的生活很容易。

如果我们没有消息闪烁，通常我们需要向模板传递参数来告诉它们是否应该显示消息。那会更混乱和复杂，所以 Flask 有这个功能是很好的！

如果你热衷于学习更多关于 Flask 和 web 开发的知识，请查看我们的 [Web 开发者训练营 Flask 和 Python](https://go.tecla.do/web-dev-course-sale) 。在其中，我们使用 Python、Flask、MongoDB 和 HTML/CSS 开发了几个网站！

感谢阅读，我们下次再见！