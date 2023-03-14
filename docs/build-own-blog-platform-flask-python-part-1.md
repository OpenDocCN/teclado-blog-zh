# 用 Flask 构建博客平台:写文章和展示文章

> 原文：<https://blog.teclado.com/build-own-blog-platform-flask-python-part-1/>

在这个系列中，我们将使用 Flask 和 PostgreSQL 来构建一个博客平台。我们将添加用户和身份验证、评论、标记，并使用 AWS 处理我们的应用程序的部署。

我正在为我的学生写这一系列的博客文章，他们正在学习 Flask 和 web 开发，并且想继续一个更全面的项目。

如果您对 Flask、Python 或 web 开发完全陌生，这可能不适合您！

在本帖中，我们将从以下内容开始:

*   设置 Flask 应用程序
*   设置数据库
*   创建帖子
*   显示帖子

为此，我们将使用:

*   计算机编程语言
*   一种数据库系统
*   HTML(还没有 CSS！)
*   和一些 Python 库:
    *   烧瓶(和金贾)
    *   SQLModel

## 安装此帖子所需的依赖项

在整个应用程序中，我们将使用几个不同的库，但在本文中，我们只需要安装两个:

```py
pip install flask
pip install sqlmodel 
```

## 使用工厂模式设置烧瓶应用程序

Flask 中的工厂模式允许我们推迟创建`Flask`对象，直到我们调用创建它的函数。

这很有帮助，因为我们可以很容易地自己调用函数来创建`Flask`对象，并且我们可以在这样做的时候传递配置参数。当我们开始测试应用程序时，这将派上用场，因为在我们的测试中，我们可能希望将不同的配置传递给应用程序，而不是在生产中。

这是我们创建 Flask 应用程序的方式:

```py
from flask import Flask

def create_app():
    app = Flask(__name__)

    return app 
```

我们只需在终端输入`flask run`就可以运行这个应用程序。Flask 命令行界面将看到一个名为`create_app`的函数存在，将运行它来获取我们的 Flask 应用程序。它只适用于这个函数名，因为这是 Flask 应用程序的约定。

## 为我们的邮政路线创建一个烧瓶蓝图

随着应用程序开始增长，就像我们的应用程序一样，将路线定义分成多个文件是有意义的。我们经常使用蓝图来帮助解决这个问题。

让我们创建一个`routes/post.py`文件。在里面，我们将定义我们的蓝图:

```py
from flask import Blueprint, request

post_pages = Blueprint("posts", __name__)

@post_pages.get("/post/<string:title>")
def display_post(title: str):
    return "Display post page."

@post_pages.route("/post/", methods=["GET", "POST"])
def create_post():
    if request.method == "POST":
        pass
    return "Create post page." 
```

为了定义我们的蓝图，我们传递了两个参数:

*   `name`，它由`url_for`函数使用，因此我们可以获得端点。稍后将详细介绍。
*   `import_name`，通常是`__name__`，用来告诉 Flask 蓝图相对于应用程序根路径的位置。

我们可以传递许多其他参数，但是其他参数的默认值是我们想要的。

我们定义了两条路由或端点:

*   `/post/`，我们将使用它来:
    *   显示用户可以提交以创建帖子的表单
    *   当用户提交表单时，接受表单数据，并在我们的数据库中创建帖子
*   `/post/<string:title>`，接收帖子标题并显示帖子页面

目前，两个端点都做得很少:只是显示一个字符串，告诉我们访问了什么路由。

请注意，`/post/`路由中有一些逻辑，如果`request.method == "POST"`发生错误，它会做一些事情。我们将使用请求方法来决定要做什么:如果是 POST，我们将获取表单数据，如果是 get，我们将显示表单页面。

## 用我们的 Flask 应用程序注册蓝图

如果我们现在运行 Flask 应用程序，它仍然不能做任何事情。那是因为蓝图和 app 没有链接。

我们需要注册蓝图，以便 Flask 应用程序可以访问它的路线，并且我们可以从我们的浏览器向它们发出请求。

为此，我们只需在应用程序中导入蓝图，并注册它:

```py
from flask import Flask
+from routes.post import post_pages

def create_app():
    app = Flask(__name__)
+   app.register_blueprint(post_pages)

    return app 
```

现在，如果我们用`flask run`运行应用程序，端点将会工作！

## 创建表单模板以添加新帖子

用 Flask 和 Jinja 创建表单有很多方法，从自己用普通的 HTML 编码，到使用 Jinja 宏，甚至像 Flask-WTF 这样的库。

当您的应用程序变得有点大或者您需要表单的安全性时，我推荐使用 Flask-WTF。因为这个应用程序中提交的表单不是很重要，所以我们可以不要它。

我将只使用 HTML 对表单进行编码。

我已经创建了一个`templates`文件夹，在里面我将放置`new_post.html`:

```py
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>New Post</title>
</head>
<body>
  <form method="POST">
    <label for="title">Title:</label>
    <input type="text" name="title" id="title" />

    <label for="content">Content: </label>
    <textarea name="content" id="content"></textarea>

    <input type="submit" value="Create post" />
  </form>
</body>
</html> 
```

我暂时保持简单。稍后当我们想给表单添加样式时，我们需要做一些改变。

这里的很多 HTML 都是样板文件。在`body`中，我们有一个`form`元素，它使用一个`POST`请求将数据发送到当前页面。

在表单中，我们有两个字段:`title`文本和`content`文本区域。

所有这些意味着当我们使用浏览器访问`/post/`时，我们将显示这个表单。当我们提交表单时，这两个字段的内容将作为一个`POST`请求提交给`/post/`端点。

在提交表单数据时会用到每个字段的`name`属性，所以提交结果会是这样的:`title=TITLE_TEXT&content=CONTENT_TEXT`。

让我们转到 Flask 端点，处理呈现模板和获取表单数据:

```py
 from flask import Blueprint, render_template, redirect, url_for, request # New imports added

...

@post_pages.route("/post/", methods=["GET", "POST"])
def create_post():
    if request.method == "POST":
        title = request.form.get("title")
        content = request.form.get("content")
        # TODO: We can create the post in our database here
        return redirect(url_for(".display_post", title=title))
    return render_template("new_post.html") 
```

我留下了一个“TODO”注释，因为我们仍然需要处理那里的数据库交互。现在，我们获取表单内容并重定向到另一个端点，将接收到的标题作为参数传递。

作为对`url_for`的一个参数，我们传递了`".display_post"`，这将计算当前蓝图中`display_post`函数*的 URL(这就是`.`的意思)。*

## 使用 Jinja 模板显示帖子

虽然我们在`new_post.html`文件中使用了一个“模板”,但我们实际上并没有在那里编写任何 Jinja 代码。从技术上讲，它并不是一个真正的模板。这只是我们发送给客户的一个 HTML 文件。

让我们使用 Jinja 来显示帖子。我将创建一个`templates/post.html`文件:

```py
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>{{ title }}</title>
</head>
<body>
  <article>
    <h1>{{ title }}</h1>
    <p>{{ content }}</p>
  </article>
</body>
</html> 
```

再说一次，就目前而言，这很简单。只是一个带有标题(`h1`)和内容(`p`)的`article`元素。稍后，当我们使用 markdown 时，我们将从 Flask 获取 HTML 格式的文章内容，并将它直接插入到模板中。

在我们的 Flask 应用程序中，我们所要做的就是获取文章标题和内容，并将其传递给我们的`render_template`调用:

```py
@post_pages.get("/post/<string:title>")
def display_post(title):
    content = "..." # How do we get the content?
    return render_template("post.html", title=title, content=content) 
```

在这个端点中，我们有文章标题，所以我们可以将它传递给模板。不过，我们还无法访问帖子内容。

在我们能够访问帖子内容(以及关于帖子的任何其他信息，如发布日期)之前，我们需要一个数据库。

顺便提一下，语法`@post_pages.get(...)`是 Flask 2.0 中的新内容。如果你以前没看过，看看[这个链接](https://github.com/pallets/flask/pull/3907)！

## 创建一个 SQLModel 模型来定义数据库表

我们将使用 SQLModel 库，它本身使用 SQLAlchemy 和 Pydantic。有了这个库，我们可以定义一个类作为数据库表定义。

我将制作`models/post.py`，并编写以下代码:

```py
from typing import Optional
from datetime import datetime
from sqlmodel import Field, SQLModel

class Post(SQLModel, table=True):
    id: Optional[int] = Field(default=None, primary_key=True)
    title: str
    content: str
    publish_date: datetime = Field(default_factory=datetime.today) 
```

这告诉 SQLAlchemy 创建一个有 4 列的`post`表:

*   `id`，将是主键，自动生成。
*   `title`，一个非空的，长度不限的`VARCHAR`。
*   `content`，一个非空的，长度不限的`VARCHAR`。
*   `publish_date`，一个`DATETIME`，当我们在 Python 中创建一个`Post`对象时，它将获得`datetime.today()`的值。

这样我们就定义了我们的表，我们将得到一个为我们填充的发布日期。不错！

让我们在蓝图中使用这个模型。非常简单！

### 使用 SQLModel 连接到数据库

让我们从 SQLModel 运行`create_engine`来实际连接到数据库，并给我们一些我们可以用来与之交互的东西。

我们来看一下`app.py`:

```py
+from sqlmodel import SQLModel, create_engine
+from models.post import Post

def create_app():
    app = Flask(__name__)
+   app.engine = create_engine("sqlite:///database.db") 
```

现在我们已经得到了，我们必须使用`app.engine`来创建我们的表。我发现的最好方法是在处理任何请求之前确保该表存在。在注册蓝图之前，我将添加以下内容:

```py
@app.before_first_request
def create_db():
    SQLModel.metadata.create_all(app.engine) 
```

重要的是，在之前*，我们已经导入了`Post`模型。否则 SQLModel 不知道要创建什么表。*

### 使用 SQLModel 向数据库添加帖子

现在我们已经得到了这些，我们可以转到我们的蓝图，利用那里的`engine`变量，以及 SQLModel 的`Session`，与数据库进行交互。

我们需要一些新的进口商品:

```py
from flask import current_app  # Add this to the existing flask imports
from sqlmodel import Session, select
from models.post import Post 
```

让我们从添加帖子开始:

```py
@post_pages.route("/post/", methods=["GET", "POST"])
def create_post():
    if request.method == "POST":
        title = request.form.get("title")
        content = request.form.get("content")
        with Session(current_app.engine) as session:
            session.add(Post(title=title, content=content))
            session.commit()
        return redirect(url_for("display_post", title=title))
    return render_template("new_post.html") 
```

这里的新代码是:

```py
with Session(current_app.engine) as session:
    session.add(Post(title=title, content=content))
    session.commit() 
```

我们首先创建一个会话，给它我们想要使用的引擎。

然后，数据库会话允许我们调用多个数据库交互。这里我们只做一个:`session.add()`。我们可以做得更多，当我们做`session.commit()`时，他们都会跑。

这意味着，如果我们想一次添加大量数据，速度会更快，因为我们不必每次都保存到数据库磁盘。它只在提交时发生。

注意，我们添加到`session`中的不是一些 SQL 代码，而是一个`Post`模型对象。SQLModel 会负责将其转换成适当的 SQL 查询！

### 使用 SQLModel 从数据库中检索帖子

我们可以在我们的`display_post`端点中做类似的事情，使用 SQLModel 的`select`函数从数据库中获取数据:

```py
@post_pages.get("/post/<string:title>")
def display_post(title):
    with Session(current_app.engine) as session:
        statement = select(Post).where(Post.title == title)
        post = session.exec(statement).first()
        return render_template("post.html", title=title, content=post.content) 
```

这里，我们再次创建一个会话，然后用它来获取与所提供的标题相匹配的文章。

`statement`保存 SQL 查询，但是直到我们用`session.exec()`执行它，它才真正运行。

在结果集中运行`.first()`,只返回第一行。在这里，我们期望只有一个职位匹配提供的标题。

我将`render_template`调用放在上下文管理器中，因为如果任何事情失败，我们将没有任何数据可以显示。如果数据库连接出现任何问题，我们可以将它封装在一些错误处理中并显示一个不同的模板。

## 后续步骤

我们在这个帖子里做了很多！我们有:

*   设置我们的应用程序。
*   使用 SQLModel 设置我们的 SQLite 数据库。
*   创建了我们的第一个非风格模板。
*   增加了创建和显示文章的蓝图。

在本系列的下一篇文章中，我们将处理用户认证，这样我们就可以注册用户，因为我们需要这个系统的*作者*和*评论*部分。

如果你喜欢这篇文章，并且想学习更多关于使用 Python 进行 web 开发的知识，考虑加入我们的 [Web 开发者训练营，使用 Flask 和 Python](https://go.tecla.do/web-dev-course-sale) 。这是一个完整的视频课程，指导您构建多个项目！