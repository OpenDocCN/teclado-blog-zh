# 使用 Python 为 Flask 或 FastAPI 创建 jwt

> 原文：<https://blog.teclado.com/jwts-python-fastapi-flask/>

在我们开始之前，让我们先谈谈什么是 JWT。JWT 是一个编码字符串，可以被解码以揭示其 JSON 内容。例如，这里有一个 JWT:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ5b3VyLXVzZXJuYW1lIiwiZXhwIjoxNjY2NzIwNDkwfQ.K62wERMzH28I5g8IygVB0y-JuYdA0-7wuZkll2AyCNs 
```

如果我们使用诸如 [jwt.io](https://jwt.io) 之类的工具对此进行解码，我们可以看到它的内容:

![](img/3c593093bad3c91eb62c734ebe4de439.png)

在这里你可以看到算法类型是`HS256`，令牌类型是`JWT`,`sub`是`your-username`，到期时间是`1666720490`(或者像我写的，大约 29 分钟后)。

jwt 用于身份验证。您的服务器可以使用给定的用户 ID 生成一个 JWT，并将其提供给您的客户端(例如浏览器)。然后，对于每个请求，客户端都向您的服务器发送 JWT，这意味着客户端代表其用户 ID 是 JWT 的`sub`的用户发出请求。

![](img/d536bcaa94699f7586e5e96aa26a816a.png)

但是如果你能解码 jwt，那不是让它们变得不安全了吗？

虽然用户可以通过解码看到 JWT 的内容，但他们不能更改 JWT(这将允许他们冒充其他用户)。用户能够看到他们自己的用户 id 通常不是问题。但是，不要在 JWT 中存储任何不想让用户看到的数据，这一点很重要。

## 使用 python-jose 库创建 JWT

让我们用 Python 和`python-jose`库创建一个 JWT:

```
pip install python-jose 
```

我将从定义三个变量开始:

```
SECRET_KEY = "9b73f2a1bdd7ae163444473d29a6885ffa22ab26117068f72a5a56a74d12d1fc"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30 
```

您可以用几种不同的方式生成自己的密钥。在 MacOS 和 Linux 中，运行终端命令`openssl rand -hex 32`。在 Windows 中，您可以使用 Python 解释器运行这个命令:`import secrets; str(secrets.SystemRandom().getrandbits(128))`。

现在你已经有了这三个，生成一个 JWT 是超级容易的！

```
from jose import jwt

def create_access_token(id_: str):
    expire = datetime.datetime.utcnow() + datetime.timedelta(
        minutes=ACCESS_TOKEN_EXPIRE_MINUTES
    )
    jwt_data = {"sub": id_, "exp": expire}
    encoded_jwt = jwt.encode(jwt_data, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt 
```

这是一个函数，给定一个字符串，它将创建一个 JWT，并将该字符串放入 JWT 的`sub`键中。在我的代码中，我倾向于将用户 id 放在那里。

## 创建一个 JWT 与烧瓶和烧瓶-JWT-扩展

如果你正在使用 Flask，有一个非常好的扩展叫做 Flask-JWT 扩展，你可以用它来代替手工创建 JWT。

这个库已经提供了一个你可以使用的函数`create_access_token`。它还支持令牌刷新和黑名单。

首先，您应该安装扩展:

```
pip install flask-jwt-extended 
```

然后，在创建 Flask 应用程序时初始化它:

```
from flask import Flask
from flask_jwt_extended import JWTManager

app = Flask(__name__)
app.config["JWT_SECRET_KEY"] = "9b73f2a1bdd7ae163444473d29a6885ffa22ab26117068f72a5a56a74d12d1fc"
jwt = JWTManager(app) 
```

然后，当您为 Flask 应用程序实现登录端点时，您可以简单地调用`create_access_token`，就像这样:

```
from flask_jwt_extended import create_access_token
# other imports
...

# Endpoint defined using Flask-Smorest
@blp.route("/login")
class UserLogin(MethodView):
    @blp.arguments(UserSchema)
    def post(self, user_data):
        user = UserModel.query.filter(
            UserModel.username == user_data["username"]
        ).first()

        if user and pbkdf2_sha256.verify(user_data["password"], user.password):
            access_token = create_access_token(identity=user.id)
            return {"access_token": access_token}, 200

        abort(401, message="Invalid credentials.") 
```

如果你想用 Flask 开发 REST API，我们有一个完整的课程，使用 Flask-Smorest、Flask-JWT 扩展、Flask-SQLAlchemy 和一些其他扩展来教你如何构建生产就绪的 REST API。你可以[在这里](https://go.tecla.do/rest-apis-sale)获得课程。

感谢阅读！如果您有任何问题或意见，请使用下面的评论部分！