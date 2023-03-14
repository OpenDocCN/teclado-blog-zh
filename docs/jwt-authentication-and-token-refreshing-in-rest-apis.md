# 使用 Flask-JWT 扩展的令牌认证和刷新

> 原文：<https://blog.teclado.com/jwt-authentication-and-token-refreshing-in-rest-apis/>

在[之前的博文](https://blog.teclado.com/learn-python-advanced-configuration-of-flask-jwt/)中，我们谈到了 Flask 扩展`Flask-JWT`，它允许我们在 Flask 应用中创建 jwt(JSON Web 令牌)。弗拉斯克-JWT 是方便的，并提供了一个最小的功能集，我们将需要基于令牌的认证。然而，随着我们的应用程序变得越来越复杂，我们可能会发现它有点受限。

请继续阅读，寻找更强大的替代方案！

## 介绍

在这篇文章中，我们介绍了一个新的烧瓶扩展:`Flask-JWT-Extended`。它具有一组更高级的功能，使我们能够设计更实用的身份认证工作流。

`Flask-JWT-Extended`相比`Flask-JWT`有很多优势。例如，它支持令牌刷新，这可能会产生更加实用和用户友好的身份验证工作流。它还有一个更加活跃的社区来维护和升级项目，因此它更有可能引入新的特性并保持稳定。在本帖中，我们将重点关注认证工作流。

## 推荐的身份验证工作流程

### 基于令牌的认证

非常像在`Flask-JWT`中，我们可以使用`Flask-JWT-Extended`执行基于令牌的认证。用户通过身份验证，他们的信息被加密并作为访问令牌返回(JWT)。

每当用户想告诉我们他们是谁时，他们就发送访问令牌和他们的请求。访问令牌让我们对用户的身份有了一定程度的信任，并提高了安全性，因为所有用户的信息都是加密的，不太可能被泄露。

### 令牌刷新

出于安全目的，每个访问令牌必须有一个到期时间。通常设置为 5 到 15 分钟，之后用户必须重新认证。

在`Flask-JWT`中，重新认证将要求用户再次输入用户名和密码。这对用户来说是非常乏味的！

**令牌刷新**正是为了这个目的。第一次通过凭证进行身份验证时，我们不仅返回包含用户帐户信息的访问令牌，还返回仅用于刷新访问令牌的刷新令牌。

当访问令牌过期时，我们提供刷新令牌，`Flask-JWT-Extended`验证它并返回新的有效访问令牌。这样，用户可以继续使用该访问令牌来访问受保护的服务。

每当原始访问令牌过期时，都会重复这个过程...那么这是否意味着用户再也不用输入他们的凭证了呢？

### 象征性新鲜度

根据上面的描述，有人可能会问，使用令牌刷新工作流和拥有永不过期的访问令牌之间有什么区别？

确实，如果我们简单地允许令牌刷新而没有进一步的限制，这并没有什么不同。但是，有一个解决方案可以使我们的身份验证工作流更加健壮。

这里有一个常见的用例:一旦我们登录，我们通常可以继续使用应用程序，而无需输入凭据。然而，如果我们试图执行一个“关键”操作——比如删除一些数据或者完成一次银行交易——我们经常会被要求提供凭证(或者至少是密码)。

这是怎么回事？

进入，**令牌新鲜度**。根据经验，通过凭证获取的任何访问令牌被标记为`fresh`，而通过刷新机制获取的访问令牌被标记为`non-fresh`。

回到我们之前的身份验证工作流，用户第一次使用其凭证登录时，他将获得一个`fresh`访问令牌和一个刷新令牌。如果他尝试重新登录并发现他的当前访问令牌已经过期，他使用他的刷新令牌来获得新的访问令牌。这个新的访问令牌应该是`non-fresh`，因为它不是通过凭证获得的。

当用户出示`non-fresh`访问令牌时，我们仍然*对用户有一些*信任。但是，当他试图执行一个关键的操作时，比如一个事务，应用程序不会接受这个访问令牌。相反，他需要再次输入他的凭证，并获得一个新的`fresh`访问令牌来继续关键操作。

让我们来看看一些代码。

## 烧瓶-JWT-延伸行动

### 证明

下面使用来自`Flask-RESTful`的基于类的视图显示了用户登录端点的代码片段。如果你学过我们的 REST API 课程，这看起来会很熟悉。

```py
from models.user import UserModel
from flask_restful import Resource, reqparse
from flask_jwt_extended import (
    create_access_token,
    create_refresh_token
)
from werkzeug.security import safe_str_cmp

class UserLogin(Resource):
    # defining the request parser and expected arguments in the request
    parser = reqparse.RequestParser()
    parser.add_argument('username',
                        type=str,
                        required=True,
                        help="This field cannot be blank."
                        )
    parser.add_argument('password',
                        type=str,
                        required=True,
                        help="This field cannot be blank."
                        )

    def post(self):
        data = self.parser.parse_args()
        # read from database to find the user and then check the password
        user = UserModel.find_by_username(data['username'])

        if user and safe_str_cmp(user.password, data['password']):
            # when authenticated, return a fresh access token and a refresh token
            access_token = create_access_token(identity=user.id, fresh=True)
            refresh_token = create_refresh_token(user.id)
            return {
                'access_token': access_token,
                'refresh_token': refresh_token
            }, 200

        return {"message": "Invalid Credentials!"}, 401 
```

### 令牌刷新

以下代码片段显示了令牌刷新端点的工作方式。

```py
class TokenRefresh(Resource):
    @jwt_refresh_token_required
    def post(self):
        # retrive the user's identity from the refresh token using a Flask-JWT-Extended built-in method
        current_user = get_jwt_identity()
        # return a non-fresh token for the user
        new_token = create_access_token(identity=current_user, fresh=False)
        return {'access_token': new_token}, 200 
```

用`@jwt_refresh_token_required`修饰的端点要求在请求中包含一个`Authorization: Bearer {refresh_token}`头。

### 使用令牌新鲜度定义不同的保护级别

然后，我们可以保护我们的端点，并定义不同的保护级别，如下所示:

```py
# An endpoint that requires a valid access token (non-expired, either fresh or non-fresh)
@jwt_required
def get(self):
    pass

# An endpoint that requires a valid fresh access token (non-expired and fresh only)
@fresh_jwt_required
def post(self):
    pass 
```

## 承认

是一个开源的 python 项目，由一个活跃而令人敬畏的社区维护。你可以参考原始文档以及他们的 GitHub repo 上的源代码:【https://github.com/vimalloc/flask-jwt-extended。

想了解更多关于开发带有身份验证、数据存储、关系数据库和部署的 REST APIs 吗？查看我们的[旗舰课程](https://www.udemy.com/rest-api-flask-and-python/?couponCode=BLOGGER)！