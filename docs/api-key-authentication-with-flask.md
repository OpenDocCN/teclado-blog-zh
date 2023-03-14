# 如何向 Flask 应用程序添加 API 密钥认证

> 原文：<https://blog.teclado.com/api-key-authentication-with-flask/>

API 密钥类似于密码，通常提供给 API 的非人类用户。每当他们向您的 API 发出请求时，他们都会发送 API 密钥，并对他们进行身份验证和识别。

在这篇文章中，让我向你展示如何添加 API 密钥认证到你的 Flask 应用程序中！我们将使用与使用 Flask 和 Python 课程的 REST APIs 相同的库:

*   瓶
*   sqllcemy(SQL 语法)
*   长颈瓶休息
*   弗拉斯克-JWT

如果你想用 Flask-RESTX 和 Flask-JWT-Extended 来代替，所需要的改变是最小的！

## 如何在数据库中生成和存储 API 密钥

### 创建`DeviceModel`

因为我们使用 SQLAlchemy，所以第一步应该是决定我们希望如何存储关于我们的“非人类用户”的数据以及我们给他们的 API 键。

让我们首先创建一个模型，我称之为`DeviceModel`，来存储上述数据。在应用程序中，我将“非人类用户”称为“设备”，这样更简单。

该型号将具有:

*   `id`，唯一自动递增标识符，内部使用
*   `device_name`，每个设备的描述字符串。
*   `device_key`，每个设备将被赋予的 API 密钥。他们可以用这个向我们的一些 API 端点发出请求。
*   `user_id`，与用户一对多的关系，所以我们知道哪些设备归哪些用户所有。

我还将向模型中添加一些辅助方法，以便稍后从我们的视图中更容易地进行交互:

```
from db import db
import uuid

class DeviceModel(db.Model):
    __tablename__ = 'devices'

    id = db.Column(db.Integer, primary_key=True)
    device_name = db.Column(db.String(80))
    device_key = db.Column(db.String(80))
    user_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    user = db.relationship('UserModel', back_populates="devices")

    def __init__(self, device_name, user_id, device_key=None):
        self.device_name = device_name
        self.user_id = user_id
        self.device_key = device_key or uuid.uuid4().hex

    def json(self):
        return {
            'device_name': self.device_name, 
            'device_key': self.device_key, 
            'user_id': self.user_id
        }

    @classmethod
    def find_by_name(cls, device_name):
        return cls.query.filter_by(device_name=device_name).first()

    @classmethod
    def find_by_device_key(cls, device_key):
        return cls.query.filter_by(device_key=device_key).first()

    def save_to_db(self):
        db.session.add(self)
        db.session.commit()

    def delete_from_db(self):
        db.session.delete(self)
        db.session.commit() 
```

### 将设备链接到用户

我们在我们的`DeviceModel`中添加了一个`relationship`，所以现在是时候在关系的另一端做同样的事情了:

```
 class UserModel(db.Model):
     __tablename__ = 'users'

     id = db.Column(db.Integer, primary_key=True)
     username = db.Column(db.String(80))
     password = db.Column(db.String(80))
+    devices = db.relationship('DeviceModel', back_populates="user") 
```

### 注册新设备和 API 密钥的 API 端点

拼图的最后一块是允许用户创建新设备，每个设备都有一个 API 密匙。

为此，我们将添加一个带有`post()`方法的 Flask-RESTful `Resource`，用户可以用设备名调用该方法。它还要求用户使用 JWT 进行身份验证:

```
from flask_restful import Resource, reqparse
from flask_jwt import jwt_required, current_identity
from models.device import DeviceModel

class AddDevice(Resource):
    parser = reqparse.RequestParser()
    parser.add_argument(
        'device_name',
        type=str,
        required=True
        )

    @jwt_required()
    def post(self):
        data = AddDevice.parser.parse_args()
        name = data["device_name"]

        if DeviceModel.find_by_name(name):
            return {'message': f"A device with name '{name}' already exists."}, 400

        new_device = DeviceModel(
            device_name=name,
            user_id=current_identity.id
        )
        new_device.save_to_db()

        return  {"api_key": new_device.device_key}, 201 
```

我们还需要将这个`Resource`注册到我们的 Flask 应用程序，以便生成端点并可以访问。在`app.py`中:

```
+from resources.device import AddDevice

...
+api.add_resource(AddDevice, '/user/add-device') 
```

要添加一个新设备，人类用户必须使用如下所示的 JSON 主体和有效的 JWT 授权头向`/user/add-device`发出请求:

```
{
    "device_name": "New Device Example"
} 
```

他们会得到这样的回应:

```
{
    "api_key": "ef229daa-d058-4dd4-9c93-24761842aec5"
} 
```

## 如何在某些 Flask 端点中要求 API 密钥

既然经过身份验证的用户可以创建一个新设备并获得一个 API 密钥，我们就可以创建只允许使用 API 密钥进行身份验证的 Flask 端点，而不是 JWT(为人类用户保留)。

您可以从添加一个类似于`security.py`中的装饰器开始:

```
from models.device import DeviceModel
import functools
from hmac import compare_digest
from flask import request

def is_valid(api_key):
    device = DeviceModel.find_by_device_key(api_key)
    if device and compare_digest(device.device_key, api_key):
        return True

def api_required(func):
    @functools.wraps(func)
    def decorator(*args, **kwargs):
        if request.json:
            api_key = request.json.get("api_key")
        else:
            return {"message": "Please provide an API key"}, 400
        # Check if API key is correct and valid
        if request.method == "POST" and is_valid(api_key):
            return func(*args, **kwargs)
        else:
            return {"message": "The provided API key is not valid"}, 403
    return decorator 
```

它检查 API 键是否存在，以及请求方法是否为“POST”。我们假设 API 键只能用于`POST`请求，但是如果这不是您想要在应用程序中设置的一个限制，您可以随意取消这个检查。

当一个端点需要一个 API 键时，只需用`@api_required`修饰器来修饰它，就像我们在一些端点中使用`@jwt_required()`一样。然后，用户必须在他们的请求中包含一个 JSON 主体，如下所示:

```
{
    "api_key": "ef229daa-d058-4dd4-9c93-24761842aec5"
} 
```

## 结论和附加内容

本帖到此为止！我希望你喜欢它，并且它能帮助你休息！如果你想学习更多关于用 Flask 和 Python 开发 REST API 的知识，请查看我们的完整课程。

感谢 [Leo Spairani](https://github.com/LUS24) 编写了这篇文章中的大部分代码！

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/server?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)T5 拍摄