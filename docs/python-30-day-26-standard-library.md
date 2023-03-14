# 第 26 天:利用标准库

> 原文：<https://blog.teclado.com/python-30-day-26-standard-library/>

欢迎来到 Python 系列 [30 天的第 26 天！今天我们将在](https://blog.teclado.com/30-days-of-python/) [Python 标准库](https://docs.python.org/3/library/)中看到更多有用的工具。我们还将看到在本系列迄今为止编写的一些代码中如何使用这些工具。

我鼓励你在读完这篇文章后更深入地探索标准库，因为它包含了大量方便的工具，为我们节省了大量的时间和精力。

## `namedtuple`功能

`namedtuple`是`collections`模块中的一个可用函数，它使我们能够用命名字段定义特殊元组。

这些特殊的元组提供了惊人的可读性改进，因为我们可以使用指定的字段名来检索值，就像我们使用字典键一样。我们还可以使用关键字参数创建这些元组的实例，这允许我们在用数据填充元组时给出数据的上下文。

为了利用这些特殊的元组，我们首先需要定义为给定的元组配置指定名称的模板，并详细说明元组的字段。这就是`namedtuple`函数的用武之地。`namedtuple`用于创建此模板。

例如，假设我们想要创建一个新的元组来存储关于一本书的信息。我们可以这样定义模板:

```py
`from collections import namedtuple

Book = namedtuple("Book", ["title", "author", "year"])` 
```

这里我们定义了一个`"Book"`元组，它有字段`"title"`、`"author"`和`"year"`。

我们已经将这个模板赋给了变量名`Book`，我们可以像这样创建这个元组的实例:

```py
`from collections import namedtuple

Book = namedtuple("Book", ["title", "author", "year"])

book = Book("The Colour of Magic", "Terry Pratchett", 1983)` 
```

正如我前面提到的，我们也可以使用关键字参数来给值提供一些上下文。

```py
`from collections import namedtuple

Book = namedtuple("Book", ["title", "author", "year"])

book = Book(title="The Colour of Magic", author="Terry Pratchett", year=1983)` 
```

从这些特殊的元组中检索值是使用点符号来完成的，但是我们也可以像往常一样通过索引来访问元素。

```py
`from collections import namedtuple

Book = namedtuple("Book", ["title", "author", "year"])

book = Book(title="The Colour of Magic", author="Terry Pratchett", year=1983)
print(f"{book.title} ({book.year}), by {book.author}")` 
```

利用`namedtuple`的一个好地方可能是在[第 14 天](/30-days-of-python/python-30-day-14-files/)处理虹膜数据的时候。

我们最终的解决方案如下所示:

```py
`with open("iris.csv", "r") as iris_file:
    iris_data = iris_file.readlines()

irises = []

for row in iris_data[1:]:
    sepal_length, sepal_width, petal_length, petal_width, species = row.strip().split(",")

    irises.append({
        "sepal_length": sepal_length,
        "sepal_width": sepal_width,
        "petal_length": petal_length,
        "petal_width": petal_width,
        "species": species
    })` 
```

这完全没问题，但是如果能针对数据变异提供一些保护就更好了，元组提供了这种保护。我们可以创建一个`Iris`元组类型来存储给定虹膜的所有数据，而不是使用字典。

```py
`from collections import namedtuple

Iris = namedtuple("Iris", ["sepal_length", "sepal_width", "petal_length", "petal_width", "species"])

with open("iris.csv", "r") as iris_file:
    iris_data = iris_file.readlines()

irises = []

for row in iris_data[1:]:
    iris = Iris(*row.strip().split(","))
    irises.append(iris)` 
```

我们用`namedtuple`定义的元组也有一个叫做`._make`的方法，它是使用`*`的替代方法。

```py
`for row in iris_data[1:]:
    iris = Iris._make(row.strip().split(","))
    irises.append(iris)` 
```

我强烈建议你查看一下[文档](https://docs.python.org/3/library/collections.html#collections.namedtuple)，以了解更多关于`namedtuple`的信息。我们应该一直使用它。

### 风格注释

虽然不是必需的，但是我强烈建议您将您的`namedtuple`模板命名为与您分配模板的变量相同的名称。这对可读性非常重要。

例如，这样做是完全合法的:

```py
`Movie = namedtuple("Film", ["title", "director", "year"])` 
```

但是您应该改为编写以下两者之一:

```py
`Film = namedtuple("Film", ["title", "director", "year"])` 
```

```py
`Movie = namedtuple("Movie", ["title", "director", "year"])` 
```

## `partial`功能

`partial`函数是一种创建函数新版本的方法，其中已经给出了一部分参数。

例如，让我们看看来自[第 13 天练习](/30-days-of-python/python-30-day-13-exercise-solutions/)的`exponentiate`函数。

```py
`def exponentiate(base, exponent):
    return base ** exponent` 
```

这基本上是内建的`pow`的重新实现，它工作得非常好。但是假设大多数时候我们想用它来对作为`base`传入的数字求平方或立方。当我们可以写类似于`square(5)`的东西时，却不得不写出`exponentiate(5, 2)`，这真的很烦人。可读性也比较差。

虽然我们可以继续编写另一个函数，如下所示:

```py
`def square(base):
    return base ** 2` 
```

如果我们处理一个更复杂的函数，这不是一个非常合理的解决方案。这将导致大量重复代码。

相反，我们可以使用`partial`来创建一个新的函数，它的`exponent`参数有一个固定值。

让我们使用`partial`从我们原来的`exponentiate`函数创建一个`square`函数和一个`cube`函数。

```py
`from functools import partial

def exponentiate(base, exponent):
    return base ** exponent

square = partial(exponentiate, exponent=2)
cube = partial(exponentiate, exponent=3)` 
```

就像这样，我们有几个新的函数可以调用:

```py
`from functools import partial

def exponentiate(base, exponent):
    return base ** exponent

square = partial(exponentiate, exponent=2)
cube = partial(exponentiate, exponent=3)

print(square(4))  # 16
print(cube(5))    # 125` 
```

你可以在这里找到更多关于`partial` [的信息。](https://docs.python.org/3/library/functools.html#functools.partial)

### 注意

有一点你必须要小心，就是参数的顺序。

如果我们想创建使用关键字参数为`base`设置固定值的函数，那会有问题，因为我们传递给新函数的第一个位置参数也会被赋值给`base`。因此，在创建我们的`partial`函数时，我们需要传入`base`的值作为位置参数。

## `defaultdict`型

模块有一些特殊类型的字典供我们使用。`defaultdict`类型是一个字典，当我们试图访问一个不存在的键时，它允许我们指定一些默认值来返回。

这非常有帮助，因为它让我们不必调用`get`，在每种情况下都指定一个默认值。这也意味着我们经常可以去掉很多逻辑来检查调用`get`的结果。

例如，假设我们有这样一个用户字典，其中每个键都是一个用户 id:

```py
`from collections import namedtuple

User = namedtuple("User", ["name", "username", "location"])

users = {
    "0001": User("Phil", "pbest", "Hungary"),
    "0002": User("Jose", "jslvtr", "Scotland"),
    "0003": User("Luka", "lukamiliv", "Serbia")
}` 
```

这是使用`namedtuple`进行更多练习的好机会，所以我把每个值都做成一个`User`元组。

现在，假设我们想使用用户的 id 从这个字典中检索用户，如果找不到用户，我们就向控制台打印一条消息。

我们最终可能会做这样的事情:

```py
`from collections import namedtuple

User = namedtuple("User", ["name", "username", "location"])

users = {
    "0001": User("Phil", "pbest", "Hungary"),
    "0002": User("Jose", "jslvtr", "Scotland"),
    "0003": User("Luka", "lukamiliv", "Serbia")
}

user_id = input("Please enter a user id: ")
user = users.get(user_id)

if user:
    print(user)
else:
    print("Could not find a user matching that user id.")` 
```

现在让我们用一个`defaultdict`重写这段代码。

为了指定在找不到键时返回的默认值，我们需要使用一个函数。在这种情况下，我将使用一个 lambda 表达式来定义这个函数，它将返回一个简单的字符串。

每当请求一个丢失的键时，`defaultdict`将调用这个函数，并且这个函数的返回值将被返回。

```py
`from collections import defaultdict, namedtuple

User = namedtuple("User", ["name", "username", "location"])

users = defaultdict(
    lambda: "Could not find a user matching that user id.",
    {
        "0001": User("Phil", "pbest", "Hungary"),
        "0002": User("Jose", "jslvtr", "Scotland"),
        "0003": User("Luka", "lukamiliv", "Serbia")
    }
)

user_id = input("Please enter a user id: ")
print(users[user_id])` 
```

文档中有很多使用`defaultdict`的创新方法的有趣例子。一种方法是作为一种计数器。

为了证明这一点，让我们假设我们正试图跟踪某种 RPG 中角色的库存。我们使用字典来存储角色的库存，关键字是物品名称，值是角色拥有多少物品的计数。

我还将创建一个函数来修改这个字典，允许用户添加新条目。

```py
`inventory = {}

def add_item(item, amount):
    if inventory.get(item):
        inventory[item] += amount
    else:
        inventory[item] = amount

add_item("bow", 1)
add_item("arrow", 20)
add_item("arrow", 20)
add_item("bracer", 2)

print(inventory)  # {'bow': 1, 'arrow': 40, 'bracer': 2}` 
```

这是可行的，但是我们可以通过使用带有`int`的`defaultdict`作为工厂函数来做得更好。

当不带任何参数调用`int`时，它返回`0`。这对我们非常有用，因为这意味着当我们这样做时:

```py
`inventory[item] += amount` 
```

就像这样:

```py
`inventory[item] = inventory[item] + amount` 
```

右边的`inventory[item]`将被替换为`0`，允许我们创建一个新的密钥，其起始值等于所添加的项目的数量。相当整洁。

```py
`from collections import defaultdict

inventory = defaultdict(int)

def add_item(item, amount):
    inventory[item] += amount

add_item("bow", 1)
add_item("arrow", 20)
add_item("arrow", 20)
add_item("bracer", 2)

print(inventory)  # defaultdict(<class 'int'>, {'bow': 1, 'arrow': 40, 'bracer': 2})` 
```

如您所见，这导致了我们代码的显著简化。

## 练习

1)使用接受标题、导演、发行年份和预算的`namedtuple`定义一个`Movie`元组。提示用户为每个字段提供信息，并创建您定义的`Movie`元组的一个实例。

2)使用`defaultdict`存储给定字符串中出现的每个字符的计数。打印本词典中最常见的字符。

3)使用`operator`模块中的`mul`函数创建一个名为`double`的`partial`，它总是提供`2`作为第一个参数。

4)使用以读取(`"r"`)模式打开文件的`partial`创建一个`read`函数。

你可以在这里找到我们的练习[的答案。](/30-days-of-python/python-30-day-26-exercise-solutions)