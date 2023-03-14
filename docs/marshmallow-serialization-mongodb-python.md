# 用 MongoDB 和 Python 实现棉花糖序列化

> 原文：<https://blog.teclado.com/marshmallow-serialization-mongodb-python/>

在编程中，序列化是将对象转换为可以存储(例如文件或数据库)或传输(例如通过互联网)的新格式的过程。因此，反序列化是将某种格式的东西转换成对象的过程。序列化通常被称为“编组”，反序列化通常被称为“解组”。

这就是“棉花糖”这个名字的由来。

Marshmallow 是为简化序列化和反序列化过程而开发的 Python 库。它可以将我们的 Python 对象转换成本地 Python 数据类型，比如字典或字符串，反之亦然。

## 用棉花糖和 Python 序列化

首先，我们必须安装棉花糖:

```
pip install marshmallow 
```

确保安装棉花糖 3，因为这是新版本。我们在这篇博文中写的代码将用于棉花糖 3。

一旦安装完毕，您就可以继续创建一个`Schema`。

一个`Schema`定义告诉 marshmallow 在序列化或反序列化时它将处理哪些单独的数据。再重复一遍:

*   Marshmallow 会将我们的对象序列化为包含这些独立数据的原生数据类型。例如，它可以把一个物体变成一本字典。
*   Marshmallow 会将包含这些单独数据片段的原生数据类型反序列化到我们的对象中。

假设我们有这个类:

```
class Store:
    def __init__(self, name: str, location: str):
        self.name = name
        self.location = location 
```

这是一个非常简单的类，它的构造函数有两个参数:两个字符串代表商店的名称和位置。

作为人类，我们可以很容易地识别出下面的字典可以代表该类的一个实例:

```
{
	"name": "Walmart",
	"location": "Venice, CA"
} 
```

但是 Python 不知道如何把类似`Store`对象的东西变成字典。

这就是棉花糖的用武之地，但是我们必须告诉它**需要使用什么属性**来构造字典:`name`和`location`。

我们通过创建一个`Schema`来做到这一点:

```
from marshmallow import Schema, fields

class StoreSchema(Schema):
	name = fields.Str()
	location = fields.Str() 
```

这里我们创建了`StoreSchema`类，它继承了 marshmallow 的`Schema`类。它包含两个类属性，`name`和`location`。名字*很重要*！价值观也很重要:`fields.Str()`。

当我们使用 marshmallow 从一个 Python 对象创建一个字典时，结果将是一个包含两个键的字典:`name`和`location`。这些值将是字符串。

现在，要将对象转换成字典，我们需要做三件事:

1.  导入我们的`Store`和`StoreSchema`类。
2.  创建一个用于实际执行序列化的`StoreSchema`对象。
3.  用`.dump()`通过`StoreSchema`对象“转储”对象。这给了我们一本字典。

```
from store import Store
from schema import StoreSchema

walmart = Store("Walmart", "Venice, CA")
store_schema = StoreSchema()

print(store_schema.dump(walmart))
# {'name': 'Walmart', 'location': 'Venice, CA'} 
```

此时一个典型的问题是:“为什么我们需要创建`StoreSchema`对象？”这是因为我们可以在那时传递一些配置选项来稍微修改或限制模式的功能。

## 用棉花糖和 Python 反序列化

在反序列化之前，marshmallow 可以验证要反序列化的数据。

我们可以添加验证规则，这样，如果数据与这些规则不一致，就会出现错误。

目前，我们仅有的验证规则是字段`name`和`location`必须是字符串，所以让我们仔细检查一下:

1.  导入我们的`StoreSchema`类。
2.  获取我们的商店数据(可能在一个文件中，由我们的用户给出，或者在这种情况下只是硬编码)。
3.  创建我们的`StoreSchema`对象。
4.  使用`.load()`通过模式传递数据进行验证。

```
from schema import StoreSchema

store_data = {"name": "Walmart", "location": "Venice, CA"}
store_schema = StoreSchema()

print(store_schema.load(store_data))
# {'name': 'Walmart', 'location': 'Venice, CA'} 
```

这里没有问题，因为字段确实是字符串！

如果我们尝试这样做，我们会得到一个错误:

```
store_data = {"name": 5, "location": "Venice, CA"}
print(store_schema.load(store_data)) 
```

您应该会得到这样一个错误:

```
Traceback (most recent call last):
File "main.py", line 11, in <module>
    print(store_schema.load(store_data))
  File "/home/runner/.local/share/virtualenvs/python3/lib/python3.8/site-packages/marshmallow/schema.py", line 722, in load
    return self._do_load(
  File "/home/runner/.local/share/virtualenvs/python3/lib/python3.8/site-packages/marshmallow/schema.py", line 904, in _do_load
    raise exc
marshmallow.exceptions.ValidationError: {'name': ['Not a valid string.']} 
```

很美，不是吗！不是有效字符串就是它最后说的，准确！

如果我们想将验证过的字典转换成一个`Store`对象，我们可以这样做，将字典的每个键作为命名参数传递给`Store`构造函数:

```
from store import Store
from schema import StoreSchema

store_data = {"name": "Walmart", "location": "Venice, CA"}
store_schema = StoreSchema()

store = Store(**store_schema.load(store_data))
print(stores)
# {'name': 'Walmart', 'location': 'Venice, CA'} 
```

默认情况下，当我们反序列化时，marshmallow 只执行验证。它没有为我们创造一个对象。

但是现在我们已经解决了验证问题，让我们稍微修改一下我们的模式，以便它在完成验证时创建一个`Store`对象。我会告诉你你*如何能*做到这一点，但通常我*不会*这样做:

```
from marshmallow import Schema, fields, post_load
from store import Store

class StoreSchema(Schema):
	name = fields.Str()
	location = fields.Str()

	@post_load
	def make_store(self, data, **kwargs):
        return Store(**data) 
```

我们现在已经添加了`@post_load`修饰方法。这在默认加载操作结束后(即验证后)运行。

`make_store`方法接收`data`:棉花糖处理过的整个有效字典。它还有一些其他可能用到的关键字参数，您可以在官方文档中找到更多相关信息。

现在我们已经得到了这个模式，它在加载后将不再给我们有效的字典。它将验证并立即执行`make_store`，这为我们提供了`Store`对象:

```
from schema import StoreSchema

store_data = {"name": "Walmart", "location": "Venice, CA"}
store_schema = StoreSchema()

print(store_schema.load(store_data))
# <store.Store instance at 0x7ff2a18c> 
```

我们马上会看到，让我们的模式为我们创建对象是一件好事，但有时也会有一点限制！我通常会避免让我们的模式返回对象，而是在使用模式的代码中这样做。

## 如何将数据存储到 MongoDB 数据库中

如果你以前从未使用过 MongoDB，这篇文章将不会是一个完整的 MongoDB 初学者指南！如果你是 MongoDB 的新手，你可以查看官方介绍。

MongoDB 是一个非关系数据库，我们在其中存储 JSON 字符串。这些 JSON 字符串是可搜索的，但是 MongoDB 没有表定义的概念，所以 JSON 字符串不必在一个集合中具有相同的结构。

在 MongoDB 中，表被称为“集合”,因为当每一行都可以有不同的列时,“列”的概念并不真正适用。

在 Python 中开始与 MongoDB 交互的最简单方法是安装`pymongo`库:

```
pip install pymongo 
```

然后，我们可以创建一个`database.py`文件来处理与 MongoDB 的交互。该课程包括:

*   一个处理创建 MongoDB 连接的方法。
*   一个将`data`参数保存到 MongoDB 中的`stores`集合的`save_to_db()`方法。
*   一个使用`query`参数在`stores`集合中查找所有匹配元素的`load_from_db()`方法。

请注意，这绝不是与 MongoDB 交互的最佳方式(尤其是在较大的应用程序中)，但是这篇博文的目的是向您介绍棉花糖序列化和反序列化——而不是 MongoDB 最佳实践！

下面是样本`database.py file:`

```
import pymongo

class Database:
	@classmethod
	def initialize(cls):
		client = pymongo.MongoClient("mongodb://localhost:27017/test_db")
		cls.database = client.get_default_database()

	@classmethod
	def save_to_db(cls, data):
		cls.database.stores.insert_one(data)

	@classmethod
	def load_from_db(cls, query):
		return cls.database.stores.find(query) 
```

让我们将一个字典保存到数据库中进行测试:

```
from database import Database

Database.initialize()
Database.save_to_db({"name": "Walmart", "location": "Venice, CA"})

loaded_objects = Database.load_from_db({"name": "Walmart"})
print(loaded_objects)
# [{'_id': ObjectId('5e7cea2c0d86c32f5a934f92'), name': 'Walmart', 'location': 'Venice, CA'}] 
```

请注意，当查询数据库时，MongoDB 将如何在`stores`集合中找到具有`Walmart`的`name`的所有元素，并返回它们。目前我们只有一个，但是如果您多次运行该文件，您会看到我们多次将同一个存储插入 MongoDB。因此，返回的列表每次都会增加。

注意，MongoDB 正在生成`_id`字段，这是一个`ObjectId`。建议生成您自己的 id，而不是使用 MongoDB 的默认值。为此，我们将使用`uuid`。

这是一个非常的 MongoDB 快速入门。现在让我们看看如何在这个应用程序中使用棉花糖。首先，我们将更改模式，使其具有一个`_id`字段:

```
from marshmallow import Schema, fields, post_load
from store import Store

class StoreSchema(Schema):
    _id = fields.Str()
	name = fields.Str()
	location = fields.Str()

	@post_load
	def make_store(self, data, **kwargs):
        return Store(**data) 
```

然后我们将改变模型，在`__init__`方法中接受它。我们给它一个默认值，这样如果没有传入，我们将生成一个 UUID:

```
import uuid

class Store:
    def __init__(self, name: str, location: str, _id: str = None):
        self.name = name
        self.location = location
        self._id = _id or uuid.uuid4().hex 
```

最后，我们可以将两者结合使用:

```
from database import Database
from schema import StoreSchema

store_schema = StoreSchema()

Database.initialize()
Database.save_to_db({"name": "Walmart", "location": "Venice, CA"})

loaded_objects = Database.load_from_db({"name": "Walmart"})

for loaded_store in loaded_objects:
	store = store_schema.load(loaded_store)
    print(store.name) 
```

我们在这里做的是获取 MongoDB 返回的字典列表，并通过我们的`StoreSchema`对象的`.load()`方法传递每个存储。然后，我们可以访问那个`Store`对象的属性(或者方法，如果有的话！).

## 在这个过程中如何处理用户数据

您可以使用 marshmallow 来处理用户数据，而不是使用 marshmallow 来处理 MongoDB 的序列化和反序列化。

假设用户给你一个字典作为数据，让你保存到 MongoDB:

1.  首先，我们将获得用户输入，通常是一个字符串。
2.  然后我们把它转换成字典。
3.  然后，我们将它传递给我们的模式进行验证。这给了我们一个对象。
4.  然后，我们使用`.dump`取回经过验证的字典，并将其保存到 MongoDB。

这是一个完美的例子，如果我们的`StoreSchema` **没有**给我们`Store`对象，我们可以节省一些工作。

```
import json
from database import Database
from schema import StoreSchema

store_schema = StoreSchema()

Database.initialize()

user_input = input("Enter a store dictionary: ")
user_dict = json.loads(user_input)
user_object = store_schema.load(user_dict)

Database.save_to_db(store_schema.dump(user_object))

loaded_objects = Database.load_from_db({"name": "Walmart"})

for loaded_store in loaded_objects:
	store = store_schema.load(loaded_store)
    print(store.name) 
```

下面是模式没有给我们`Store`对象时的代码:

```
import json
from database import Database
from schema import StoreSchema

store_schema = StoreSchema()

Database.initialize()

user_input = input("Enter a store dictionary: ")
user_dict = json.loads(user_input)
validated_dict = store_schema.load(user_dict)

Database.save_to_db(validated_dict)

loaded_objects = Database.load_from_db({"name": "Walmart"})

for loaded_store in loaded_objects:
    store = Store(**store_schema.load(loaded_store))
    print(store.name) 
```

这样做的好处是，我们现在可以使用该模式来加载用户数据和数据库数据，以防后来被应用程序的另一部分更改。

## 包扎

在这篇博文中，我们学习了如何使用 Marshmallow 来序列化和反序列化数据，以及如何在 MongoDB 中使用它。

希望这篇帖子有用，感谢阅读！