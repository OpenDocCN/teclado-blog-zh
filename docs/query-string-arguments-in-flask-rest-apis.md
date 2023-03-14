# 使用 Flask 在 REST APIs 中查询字符串参数

> 原文：<https://blog.teclado.com/query-string-arguments-in-flask-rest-apis/>

查询字符串参数是添加在 URL 末尾的值，遵循`name1=value1&name2=value2`的格式。您可以用“与”符号分隔多个值(如图所示)。查询字符串参数通过问号与 URL 的主要部分分开。

因此，带有查询字符串参数的完整 URL 可能如下所示:

```py
http://your-api.com/item/?tag=furniture 
```

要获得多个值，您需要包含两个或多个相同的查询字符串参数:

```py
http://your-api.com/item/?tag=furniture&tag=office 
```

在我的使用 Flask 和 Python 的 REST API 课程中，我们使用 Flask-Smorest 库构建 REST API。在这篇博文中，让我向您展示如何将查询字符串参数与 Flask-Smorest 一起使用。

如果你想看视频，请点击这里:

[https://www.youtube.com/embed/oJGRvtJbnmU](https://www.youtube.com/embed/oJGRvtJbnmU)

VIDEO

## 如何访问 Flask 端点中的查询字符串值

在 Flask 端点中访问查询字符串值相对简单:

*   从`flask`导入`request`代理
*   查询字符串参数存储在`request.args`下一个类似 dict 的值中。

这里有一个示例端点，它返回数据库中的所有商店。

```py
@app.route("/store")
def get_stores():
    return [{"name": store.name, "id": store.id} for store in StoreModel.query.all()] 
```

现在，假设您想要添加一个查询字符串参数值来按名称过滤商店。我们的 API 用户将发送给我们`name=Tec`，API 应该响应名称以`Tec`开头的商店。

为了获得查询字符串参数值，我们使用`request.args.get("name")`:

```py
# at top of file
from flask import app, request

# in endpoint
@app.route("/store")
def get_stores():
    name = request.args.get("name")
    return [{"name": store.name, "id": store.id} for store in StoreModel.query.all()] 
```

然后我们可以在调用`.all()`之前给`.query`添加一个过滤器:

```py
# at top of file
from flask import request

# in endpoint
@app.route("/store")
def get_stores():
    name = request.args.get("name")
    return [
        {"name": store.name, "id": store.id}
        for store in StoreModel.query.filter(StoreModel.name.startswith(name)).all()
    ] 
```

顺便说一下，如果您想在商店名称的任何地方搜索给定值，而不是`.startswith`，您可以使用:

```py
StoreModel.query.filter(StoreModel.name.like(f"%{name}%")).all() 
```

## 访问查询字符串参数中的值列表

首先，确保用户像这样发送查询字符串:

```py
?tags=furniture&tags=office 
```

然后在 Flask 中，像这样访问这些值:

```py
request.args.getlist("tags")  # ["furniture", "office"] 
```

如果使用`request.args.get("tags")`，那么只返回第一个标签值。

或者，您*可以*(但我不推荐)，这样做来处理逗号分隔的值:

```py
request.args.get("tags").split(",") 
```

我可以预见的问题是，如果收到的值包含逗号，您可能会遇到麻烦。对于标签*和*，这种情况可能不会发生，但对于其他值，这种情况可能会发生。您最好使用推荐的方法，即获取多个查询字符串参数并使用`.getlist()`。

## 如何访问 Flask-Smorest 资源中的查询字符串参数

这里有一个示例`Resource`类，它返回数据库中的所有商店。这类似于我们之前展示的端点。

```py
@blp.route("/store")
class StoreList(MethodView):
    @blp.response(200, StoreSchema(many=True))
    def get(self):
        return StoreModel.query.all() 
```

现在，您希望添加与前面相同的查询字符串参数，以便按名称过滤商店。你可以简单地像在 vanilla Flask 中一样，通过导入`request`，然后使用`request.args.get()`。

但是 Flask-Smorest 的主要好处之一是文档和验证，所以让我们试着做得更好。

首先，我们将为查询字符串参数定义一个模式:

```py
class StoreSearchQueryArgs(BaseSchema):
    name: str | None 
```

然后我们用一个`@blp.arguments()`装饰器来装饰`get`方法，确保通过`location="query"`，就像这样:

```py
@blp.route("/store")
class StoreList(MethodView):
    @blp.arguments(StoreSearchQueryArgs, location="query")
    @blp.response(200, StoreSchema(many=True))
    def get(self, search_values):
        return StoreModel.query.all() 
```

然后我们可以在调用`.all()`之前将过滤器添加到`.query`:

```py
@blp.route("/store")
class StoreList(MethodView):
    @blp.arguments(StoreSearchQueryArgs, location="query")
    @blp.response(200, StoreSchema(many=True))
    def get(self, search_values):
        return StoreModel.query.filter(StoreModel.name.startswith(search_values.get("name", ""))).all() 
```

对于一个完整的 Flask-Smorest 应用程序，你可以看看这个要点。

好了，这就是这篇博文的全部内容！我只是想向您展示如何在 Flask 中使用查询字符串参数，重点是使用 Flask-Smorest 的 REST APIs。

如果你想了解更多关于用 Flask 开发 REST API 的知识，可以考虑看看我们的课程:[用 Flask 和 Python 开发 REST API](https://go.tecla.do/rest-apis-sale)。这是一个完整的视频课程，涵盖了您需要的一切，从基础到使用数据库、部署、后台任务等等！