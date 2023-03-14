# 烧瓶的高级配置-JWT

> 原文：<https://blog.teclado.com/learn-python-advanced-configuration-of-flask-jwt/>

烧瓶-JWT 增加了 JWT 功能的烧瓶在一个易于使用的方式。它为您提供了许多现成的功能，但有时我们希望修改一些配置。本文档介绍了如何:

*   更改认证端点(默认为`/auth`)；
*   更改令牌过期时间(默认为`5 minutes`)；
*   更改认证密钥名称(默认为`username`)；
*   更改认证响应体(默认只包含`access_token`)；
*   更改错误处理程序。

此外，它还介绍了如何从我们的 Flask 应用程序端点检索当前登录的用户的**。**

本教程假设你已经听完了讲座，并且已经建立了 Flask-JWT！如果您还没有这样做，请查看我的课程的第 5 部分。

## 开始之前

首先，让我们看看这里已经有什么。在我们的 app.py 文件中，我们应该已经使用下面的代码设置了 JWT:

```py
from flask_jwt import JWT
from security import authenticate, identity

jwt = JWT(app, authenticate, identity)  # /auth 
```

在我们的 security.py 文件中，我们应该有这样的内容:

```py
from werkzeug.security import safe_str_cmp
from models.user import UserModel

def authenticate(username, password):
    user = UserModel.find_by_username(username)
    if user and safe_str_cmp(user.password, password):
        return user

def identity(payload):
    user_id = payload['identity']
    return UserModel.find_by_id(user_id) 
```

## 配置

### 认证 URL

例如，如果我们想要更改身份验证端点的 url，我们想要使用`/login`而不是`/auth`，我们可以这样做:

```py
app.config['JWT_AUTH_URL_RULE'] = '/login'
jwt = JWT(app, authenticate, identity) 
```

**重要的**:我们添加了第二行代码来强调，在创建`JWT`实例之前，我们必须首先更改 JWT 认证 URL**。否则，我们的配置不会生效。然而，它仅在配置 auth URL 时需要，在请求`JWT`实例后，以下配置仍然有效。**

### 令牌过期时间

```py
# config JWT to expire within half an hour
app.config['JWT_EXPIRATION_DELTA'] = timedelta(seconds=1800) 
```

### 认证密钥名称

```py
# config JWT auth key name to be 'email' instead of default 'username'
app.config['JWT_AUTH_USERNAME_KEY'] = 'email' 
```

### 认证响应处理程序

有时我们可能希望在认证响应体中包含更多信息，而不仅仅是`access_token`。例如，我们可能还想在响应正文中包含用户的 ID。在这种情况下，我们可以这样做:

```py
# customize JWT auth response, include user_id in response body
from flask import jsonify
from flask_jwt import JWT

from security import authenticate, identity as identity_function
jwt = JWT(app, authenticate, identity_function)

@jwt.auth_response_handler
def customized_response_handler(access_token, identity):
    return jsonify({
                        'access_token': access_token.decode('utf-8'),
                        'user_id': identity.id
                   }) 
```

记住，`identity`应该是由`authenticate()`函数返回的，在我们的例子中，它是一个包含字段`id`的`UserModel`对象。确保只访问您的`identity`模型中的有效字段！

此外，一般不建议在`access_token`中包含加密的信息，因为这可能会带来安全问题。

### 错误处理程序

默认情况下，当任何处理程序中出现错误时(例如，在认证、标识或创建响应期间)，Flask-JWT 会引发`JWTError`。在某些情况下，我们可能想要定制当这样的错误发生时我们的 Flask 应用程序做什么。我们可以这样做:

```py
# customize JWT auth response, include user_id in response body
from flask import jsonify
from flask_jwt import JWT

from security import authenticate, identity as identity_function
jwt = JWT(app, authenticate, identity_function)

@jwt.error_handler
def customized_error_handler(error):
    return jsonify({
                       'message': error.description,
                       'code': error.status_code
                   }), error.status_code 
```

### 其他配置

你可以在这里找到更多的配置选项:[https://pythonhosted.org/Flask-JWT/](https://pythonhosted.org/Flask-JWT/)

请参考<configuration options="">部分。</configuration>

## 更大的

### 正在从令牌中检索用户

另一个常见问题是:*我如何从访问令牌(JWT)中获取用户的身份？*因为在某些情况下，我们不仅希望保证只有我们的用户可以访问终端，而且我们可能还希望访问用户的数据。例如，如果您想将访问权限限制在某个用户组，而不是每个用户。在这种情况下，您可以这样做:

```py
from flask_jwt import jwt_required, current_identity

class User(Resource):

    @jwt_required()
    def get(self):   # view all users
        user = current_identity
        # then implement admin auth method
        ... 
```

现在这个端点受到 JWT 的保护。您可以从 JWT 使用`current_identity`访问与该端点交互的用户的身份。