# 使用 Flask-JWT 的简单 JWT 认证

> 原文：<https://blog.teclado.com/simple-jwt-authentication-with-flask-jwt/>

通常在 Flask 应用程序中，我们希望添加登录/注销功能。根据您创建的应用程序的类型，您可以使用会话或令牌。

*   会话最适合使用 Flask 提供网页服务的应用程序——即大量使用`render_template`。
*   令牌最适合 API，其中 Flask 应用程序接受数据并将其返回给另一个应用程序(如移动应用程序或 web 应用程序)。

在这篇文章中，我们将学习如何为你的 Flask 应用添加基于令牌的认证。但是首先...

## 什么是 JWT？

JWT 代表 JSON Web Token，它是一段编码了一些信息的文本。

在 Flask 应用中进行身份验证时存储的信息通常是我们可以用来识别为其生成 JWT 的用户的信息。

流程是这样的:

1.  用户提供他们的用户名和密码
2.  我们在 Flask 应用程序中验证它们是正确的
3.  我们生成一个包含用户 ID 的 JWT。
4.  我们把它发送给用户。
5.  每当用户向我们的应用程序发出请求时，他们必须将我们之前生成的 JWT 发送给我们。通过这样做，我们可以验证 JWT 是有效的——然后我们将知道向我们发送 JWT 的用户是我们为其生成的用户。

最后一点很重要。当我们收到已知有效的 JWT 时，我们知道我们是为特定用户生成的。我们可以使用存储在 JWT 中的信息来检查这一点。

因为我们知道用户向我们发送了他们登录时我们生成的 JWT，所以我们可以将它视为“登录用户”。

任何没有向我们发送有效 JWT 的用户，我们将被视为“注销”用户。

## 使用 Flask-JWT 进行认证

用 Flask 认证有两个主要的库:Flask-JWT 和 Flask-JWT-扩展。

弗拉斯克-JWT 稍微简单一点，而弗拉斯克-JWT-扩展的稍微强大一点。学习一个将使学习另一个变得非常简单。

在这篇文章中，我们将使用弗拉斯克-JWT。

### 安装并链接我们的应用程序

要安装 Flask-JWT，请激活您的虚拟环境，然后执行以下操作:

```py
pip install flask-jwt 
```

然后，在定义你的应用程序的文件中，你需要导入弗拉斯克-JWT 并创建`JWT`对象。您还需要将`app.secret_key`定义为用于签署 JWT，这样您就知道是您的应用程序创建了它，而不是其他任何人:

```py
from flask import Flask
from flask_jwt import JWT
from security import authenticate, identity

app = Flask(__name__)
app.secret_key = "jose"  # Make this long, random, and secret in a real app!
jwt = JWT(app, authenticate, identity)

if __name__ == "__main__":
	app.run() 
```

我们还有一个针对`authenticate`和`identity`的导入。Flask-JWT 需要这两个函数来知道如何处理传入的 JWT，以及我们希望在传出的 JWT 中存储什么数据。

一旦我们创建了`JWT`对象，Flask-JWT 就向我们的应用程序注册了一个端点`/auth`。

这意味着该代码中的简单应用程序已经有了一个用户可以访问的端点。默认情况下，用户应该能够向`/auth`端点发送带有一些 JSON 有效负载的 POST 请求:

```py
{
  "username": "their_username",
  "password": "their_plaintext_password"
} 
```

## 什么是`authenticate`？

`authenticate`功能用于认证用户。这意味着，当用户向我们提供他们的用户名和密码时，我们希望将哪些数据放入 JWT。请记住，我们放入 JWT 的数据将在用户每次发送请求时返回给我们。

流程是这样的:

1.  用户使用他们的用户名和密码作为 JSON 有效负载向新的`/auth`端点发出 POST 请求。
2.  使用该用户名和密码调用`authenticate`函数。弗拉斯克-JWT 在我们创建`JWT`对象时设置了这个。

通常在`authenticate`函数中，我检查用户的用户名和密码的有效性，然后告诉弗拉斯克-JWT 将用户的`id`存储在 JWT 中。

大概是这样的:

```py
from werkzeug.security import safe_str_cmp
from models.user import UserModel

def authenticate(username, password):
    user = UserModel.find_by_username(username)
    if user and safe_str_cmp(user.password, password):
        return user 
```

我的`authenticate`函数接受一个`username`和`password`。然后，它进入数据库，找到与该用户名匹配的用户，并检查密码是否正确。

如果是，则返回用户。

这是否意味着用户存储在 JWT？

第二个 Flask-JWT 将获取`user`对象的`id`属性，并将那个对象的*存储在 JWT 中。*

如果您的`user`对象没有`id`属性，您将得到一个错误。

您可以通过设置应用程序配置属性来更改存储在 JWT 中的属性。在我们的 [Flask-JWT 配置博客文章](https://blog.teclado.com/learn-python-advanced-configuration-of-flask-jwt/)中了解更多信息。

### 什么是`identity`？

当我们接收到一个 JWT 时，就会用到`identity`函数。

在我们的任何端点(除了`/auth`端点)，用户可以向我们发送一个 JWT 以及他们的数据。为此，他们会在请求中添加一个标题:

```py
Authorization: JWT <JWT_VALUE_HERE> 
```

当这种情况发生时，弗拉斯克-JWT 公司将接管 JWT 并从中获取数据。存储在 JWT 中的数据被称为“有效载荷”，因此我们的`identity`函数接受该有效载荷作为参数:

```py
def identity(payload):
    user_id = payload['identity']
    return UserModel.find_by_id(user_id) 
```

`payload['identity']`包含用户的`id`属性，这是我们在创建它时保存到 JWT 中的。`payload`还包含其他内容，比如令牌何时创建、何时到期等等。更多信息，请阅读本文的[“有效载荷”部分。](https://scotch.io/tutorials/the-anatomy-of-a-json-web-token)

因为`payload['identity']`是用户的`id`——我们用它在数据库中找到用户并返回它。

**重要的**:除非我们用`@jwt_required()`装饰器装饰端点，否则不会调用`identity`函数，如下所示:

```py
from flask_jwt import jwt_required, current_identity

@app.route('/protected')
@jwt_required()
def protected():
    return '%s' % current_identity 
```

在任何用`@jwt_required()`修饰的端点内，我们可以访问`current_identity`代理——它将为我们提供`identity`函数在这个特定请求中收到的 JWT 返回的任何内容。

### 测试和错误消息

这里有一个简单的应用程序，取自官方文档，你可以用它来测试你的 Flask-JWT 请求。

我建议用弗拉斯克-JWT 测试不同的场景，看看它能给你带来什么。例如，如果发生以下情况会怎么样:

*   您发送了无效的用户名或密码；
*   您发送了无效或不完整的 JWT；
*   在负载中 id 为的数据库中找不到您的用户；

## 烧瓶-JWT-扩展

烧瓶-JWT-扩展非常类似于烧瓶-JWT，但有更多的配置选项和一些更多的功能。例如，它允许令牌刷新。

在你熟悉了 Flask-JWT 之后——如果你需要那些高级功能——阅读我们在 Flask-JWT-Extended 上的博客文章,了解更多！

* * *

我希望这篇文章对你有用，你也学到了一些东西！

如果你想要一套更好、更容易理解的视频教程来指导你创建 Flask 应用程序和 REST API，请查看我们的[REST API with Flask and Python](https://go.tecla.do/rest-apis-sale)课程。它包含了您轻松开发简单、专业的 REST APIs 所需的一切。

如果您注册了我们下面的邮件列表，这是获得折扣代码的最佳方式——我们每个月都会与我们的订户分享它们！