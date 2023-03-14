# 第 28 天项目:网络搜集

> 原文：<https://blog.teclado.com/python-30-day-28-project-web-scraping/>

欢迎来到 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[第 28](/30-days-of-python/python-30-day-28-type-hinting) 天项目！今天，我们将继续探索第三方模块，编写一个程序来自动为我们从网页中抓取内容！

为了做到这一点，我们将使用一个名为`BeautifulSoup`的库，用于解析和搜索 HTML 文档。HTML 是一种标记语言，用于为网页内容提供结构和意义。

在我们更详细地查看`BeautifulSoup`之前，我们需要了解一下网页是如何构造的，以及我们如何通过编程来导航它们。

记住，如果你喜欢这种媒体，我们已经为这篇博文准备了一个[视频演示](https://youtu.be/lZwMZOv-7KA)！

## HTML 快速入门

正如我在介绍中提到的，HTML 是一种标记语言，用于为网页内容提供结构和意义。

标记语言是一种以计算机可以理解的方式注释文本的语言。标记语言的另一个例子是 [Markdown](https://www.markdownguide.org/basic-syntax/) ，它被用在各种论坛、博客和聊天应用程序上，以指示文本应该如何被格式化。

下面是一些描述一个简单网页的 HTML。

```py
`<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="description" content="A simple page that greets the world with enthusiasm.">
    <title>Hello World!</title>
</head>

<body>
    <h1>Hello World!</h1>
</body>

</html>` 
```

第一行包含`<!DOCTYPE html>`，它实际上只是一个标签，让计算机知道接下来的内容应该是什么格式。这对浏览器来说很重要，因为虽然 HTML5 现在是标准，但一些网站使用其他更老的格式和不同的语法。

一个 HTML 文档的主要内容是由标签组成的，所以让我们来谈谈这些标签是如何工作的。

### HTML 标签

标签由几个组件组成，HTML 中有许多不同类型的元素，我们可以为它们创建标签，每种元素都有自己的名称和特殊含义。我们没有时间在这里讨论所有的元素，我们也不需要这样做，因为当*构建*网站时，知道使用哪个元素确实是一个需要考虑的问题。

当我们想要创建一个 HTML 元素时，这个元素的名字被放在尖括号中，就像这样:`<html>`。这种结构被称为标签。

通常标签有一个伴随的结束标签，在元素名称前有一个`/`。我们可以在上面的文档中看到一个关于`html`、`head`、`body`、`title`和`h1`元素的例子。

当一个 HTML 元素有一个开始和结束标记时，放置在这些标记之间的任何内容都被认为是在该元素内部。通过嵌套元素，我们可以给出一个页面结构。

例如，让我们看看下面这段 HTML 代码:

```py
`<div>
    <p>
        Spicy jalapeno bacon ipsum dolor amet id tongue pork belly andouille.
    </p>
</div>` 
```

这里我们有一些[辣培根 ipsum](https://baconipsum.com/) 文本位于一组`<p>`标签中，这些标签用于表示一段文本。这些`<p>`标签是一组`<div>`标签中的*，用于定义一些通用的内容组。*

理解 HTML 的层次本质非常重要，因为它将允许我们稍后轻松地遍历文档。

### 注意

并非所有的 HTML 元素都有开始和结束标记。如果我们查看 Hello，World 页面的`head`标记中的元素，我们可以看到有几个`meta`元素，每个元素只由一个标记组成。

这些元素不允许有内容，HTML 中有很多这样的元素。

### HTML 属性

除了名称，我们还可以为给定的 HTML 元素定义一组属性。对于这个项目，我们需要知道几个非常常见的属性，叫做`class`和`id`。

下面是一个设置了`class`和`id`属性的 HTML 元素的例子。

```py
`<body>
    <h1 class="header" id="pageTitle">Hello World!</h1>
</body>` 
```

这种方法的语法非常简单。我们只有属性的名称，后跟一个符号和一个描述与该属性相关的值的字符串。

`class`和`id`属性为我们提供了引用 HTML 文档不同部分的方法，它们通常用于应用样式，或者用 JavaScript 与页面上的元素进行交互。

在我们的例子中，它们对于获取页面上的元素非常有用。

## CSS 选择器快速入门

CSS 是另一种语言(简称为**C**as cading**S**tyle**S**sheets ),用于定义 HTML 文档的元素实际应该是什么样子。

在这个项目中，我们不打算做任何样式设计，但是我们确实需要了解一点选择器语法，这是我们如何描述一个样式适用于哪些元素的。这与我们相关的原因是因为`BeautifulSoup`允许我们使用这些选择器搜索 HTML 文档，它们非常强大。

### 基本选择器

首先，让我们讨论如何通过元素名、类和 id 进行选择。

选择页面上元素的所有实例非常容易，因为选择器就是元素名称。例如，如果我们想要选择所有的`h1`标签，我们可以只写`h1`。

如果我们想通过一个类进行选择，我们必须写下这个类的名字，以一个句点(`.`)为前缀。

回到这个例子，

```py
`<body>
    <h1 class="header" id="pageTitle">Hello World!</h1>
</body>` 
```

我们可以使用选择器`.header`获取这个`h1`标签，因为`header`类被列在这个特定的`h1`标签的类中。

为了通过`id`进行搜索，我们需要使用`#`后跟`id`的名字。对于上面的例子，选择器应该是这样的:`#pageTitle`。

理论上，id 应该是唯一的，所以通过`id`选择应该提供单个元素，但是 HTML 没有强制这样做，所以我们不能指望它。

### 组合 CSS 选择器

有时我们不能用一个术语准确地说明我们的意思，所以我们可以将选择器组合成更具体的选择器。

例如，如果我们想确保只选择带有`header`类的`h1`元素，而不是带有`header`类的`h2`元素，我们可以这样写:

通过在选择器之间放置一个空格，我们可以暗示一个层次结构。下面的意思是"`div`内带有 header 类的`h1`元素":

我们可以比这复杂得多，但是这些相对简单的选择器足以让我们开始这个项目。

你可以在这里找到更多关于不同 CSS 选择器的信息。

## 开始使用`BeautifulSoup`

至此，您已经知道了安装第三方模块的步骤，所以没有必要在此重复。如果你卡住了，你可以在[第 27 天](/30-days-of-python/python-30-day-27-local-development/)的帖子中找到信息，或者你可以遵循`BeautifulSoup` [安装指南](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#installing-beautiful-soup)。

### 解析 HTML

让我们试着从`BeautifulSoup`文档中解析一些样本 HTML，看起来像这样:

```py
`html_doc = """
<html>

<head>
 <title>The Dormouse's story</title>
</head>

<body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">
 Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.
 </p>

</body>

</html>
"""` 
```

这里我们正在处理一个多行字符串，它包含一些 HTML 格式的文本。稍后，我们将直接从网站获取 HTML。过程是一样的。

`BeautifulSoup`的导入有点奇怪。

```py
`from bs4 import BeautifulSoup` 
```

注意不要试图导入`from BeautifulSoup`。那不行。

导入`BeautifulSoup`后，我们需要做的就是解析文件:

```py
`from bs4 import BeautifulSoup

html_doc = """
<html>

<head>
 <title>The Dormouse's story</title>
</head>

<body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">
 Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.
 </p>

</body>

</html>
"""

soup = BeautifulSoup(html_doc, "html.parser")` 
```

现在我们可以使用`soup`轻松地在源文档中找到我们想要的东西。

## 从混乱中获取数据

汤里面的物品是`Tag`物体。这是由 BeautifulSoup 作者创建的一种特殊类型，用于存储 HTML 文档中给定元素的信息。当我们在`soup`中搜索以获得页面上的特定元素时，我们将得到一个`Tag`或一列`Tag`对象。

`Tag`对象有很多有用的方法和属性，比如`content`，它让我们看到给定元素内部的内容。如果标签只包含文本内容，我们可以使用`string`属性来获取内容。我们一会儿会看到一个例子。

如果我们想获得一个属性的值，我们可以把标签当作一个字典，标签名就是键。因此，如果我们想要给定标签的`class`属性的内容，我们可以编写`tag["class"]`。

我们可以使用`select`和`select_one`方法搜索`soup`，这两个方法接受 CSS 选择器作为字符串。

例如，假设我们想从上面的 HTML 文档中获取姐妹的名字。我们可以这样做:

```py
`from bs4 import BeautifulSoup

html_doc = """
<html>

<head>
 <title>The Dormouse's story</title>
</head>

<body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">
 Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.
 </p>

</body>

</html>
"""

soup = BeautifulSoup(html_doc, "html.parser")
sisters = soup.select(".sister")

for sister in sisters:
    print(sister.string)` 
```

这里`soup.select(".sister")`给了我们一个作为`Tag`对象的`<a>`元素列表。然后我们遍历列表，这意味着对于任何给定的迭代，`sister`包含一个`Tag`。

因为`<a>`元素只包含文本，所以我们可以通过访问标签的`string`属性来提取文本内容。

希望这些都有意义。如果你想更深入地研究`BeautifulSoup`，你可以在这里找到文档[。](https://www.crummy.com/software/BeautifulSoup/bs4/doc/#making-the-soup)

## 案情摘要

唷！这需要涵盖很多内容，但是我们现在已经准备好处理实际的项目了。

对于这个项目，我们将刮一个名为[http://books.toscrape.com/](http://books.toscrape.com/)的网站。这是一个专门用来学习如何抓取真实网站的网站。

你需要做的是刮这个网站的首页，并抓取该页面上每本书的一些数据。

对于每本书，我要你抓住标题，星级和价格。您应该将所有这些信息以 CSV 格式写入一个新文件。

在做这些之前，您需要获得实际的 HTML 文档。为此，请在文件中添加以下几行:

```py
`import requests

data = requests.get("http://books.toscrape.com/").content` 
```

注意，您必须安装`requests`库，因为它不是标准库的一部分。

实际查看 HTML 时，您有几个选项。通过在您的`soup`上使用`prettify`方法，您可以以一种很好的格式获得数据，您可以将数据写入文件或打印到控制台。你也可以直接进入网站，在页面上点击右键。您应该看到一个查看页面源代码的选项。如果你看不到这个，试着按下`ctrl` + `u`。

祝项目好运！

## 我们的解决方案

首先，让我们把所有的进口和初始设置有序。

```py
`import requests
from bs4 import BeautifulSoup

data = requests.get("http://books.toscrape.com/").content

soup = BeautifulSoup(data, "html.parser")` 
```

在我们继续之前，我们真的需要看一下 HTML 代码，看看如何最好地获取我们正在寻找的数据。

每本书都作为有序列表(`<ol>`标签)的一部分存储为列表项(`<li>`标签)，如下所示:

```py
`<li class="col-xs-6 col-sm-4 col-md-3 col-lg-3">
    <article class="product_pod">
        <div class="image_container">
            <a href="catalogue/a-light-in-the-attic_1000/index.html">
                <img
                    src="media/cache/2c/da/2cdad67c44b002e7ead0cc35693c0e8b.jpg"
                    alt="A Light in the Attic"
                    class="thumbnail"
                >
            </a>
        </div>

        <p class="star-rating Three">
            <i class="icon-star"></i>
            <i class="icon-star"></i>
            <i class="icon-star"></i>
            <i class="icon-star"></i>
            <i class="icon-star"></i>
        </p>

        <h3>
            <a href="catalogue/a-light-in-the-attic_1000/index.html" title="A Light in the Attic">
                A Light in the ...
            </a>
        </h3>

        <div class="product_price">
            <p class="price_color">£51.77</p>
            <p class="instock availability">
                <i class="icon-ok"></i>
                In stock
            </p>

            <form>
                <button type="submit" class="btn btn-primary btn-block" data-loading-text="Adding...">
                    Add to basket
                </button>
            </form>
        </div>
    </article>
</li>` 
```

我们不需要担心这些信息的大部分。例如，每个`<li>`的第一部分专用于图像。底部还有一个小的`form`元素，用于向购物篮中添加一本书。

让我们把它分成几个部分，这样我们可以更好地推理到底发生了什么。

对于价格，我们关心的是这一部分:

```py
`<div class="product_price">
    <p class="price_color">£51.77</p>
    <p class="instock availability">
        <i class="icon-ok"></i>
        In stock
     </p>

     <form>
        <button type="submit" class="btn btn-primary btn-block" data-loading-text="Adding...">
            Add to basket
        </button>
     </form>
</div>` 
```

更方便的是，价格在它自己的`<p>`标签中，并且有一个与之相关联的类。如果我们扫描文档，我们还会发现它是一个唯一的类，仅用于价格。

因此，使用一个非常简单的选择器:`".price_color"`就可以获得包含价格的`p`标签。

标题有点复杂。它位于这里:

```py
`<h3>
    <a href="catalogue/a-light-in-the-attic_1000/index.html" title="A Light in the Attic">
        A Light in the ...
    </a>
</h3>` 
```

这里我们没有任何类或 id 可以使用，而且书名实际上也不是`<a>`标签中的字符串内容。如果我们查看，较长的标题会被截断，所以我们需要查看`title`属性中的值。

在这种情况下，我认为我们应该使用这样的选择器:

这是在寻找位于`h3`元素内部的`a`元素，也就是位于具有类`.product_pod`的元素内部的`a`元素。`product_pod`从何而来？它是添加到包含我们的`h3`标签的`article`元素中的一个类。

这个选择器足够具体，我们不太可能意外地选择了我们不想要的东西。否则，我们将不得不使用一个更加特殊的选择器。

最后，让我们看看评级的 HTML。

```py
`<p class="star-rating Three">
    <i class="icon-star"></i>
    <i class="icon-star"></i>
    <i class="icon-star"></i>
    <i class="icon-star"></i>
    <i class="icon-star"></i>
</p>` 
```

这里我们有另一个简单的选择器选项，我们可以只寻找`star-rating`类。不过，获取我们需要的信息会有点困难，因为一本书的星级数是由另一个类表示的。因此，我们必须创建一些方法来检索第二个类，并将其转换成我们在程序中更容易使用的东西。

现在我们已经分析了 HTML，让我们实现代码来获取我们想要的标签。

作为一个小提示，我将把我们的选择器存储在变量中，这样它们更容易重用。

```py
`import requests
from bs4 import BeautifulSoup

price_selector = ".price_color"
title_selector = ".product_pod h3 a"
rating_selector = ".star-rating"

data = requests.get("http://books.toscrape.com/").content

soup = BeautifulSoup(data, "html.parser")

prices = soup.select(price_selector)
titles = soup.select(title_selector)
ratings = soup.select(rating_selector)` 
```

这里的一个好处是我们的标签有一致的顺序，因为我们处理的是列表。因此，我们可以使用`zip`将不同的值组合到原始书籍中。

然后我们可以使用一个`for`循环来迭代`zip`对象，这样我们就可以处理给定书籍的所有数据。现在我只关注打印值，但是这个`for`循环最终会转移到上下文管理器中。

```py
`import requests
from bs4 import BeautifulSoup

price_selector = ".price_color"
title_selector = ".product_pod h3 a"
rating_selector = ".star-rating"

data = requests.get("http://books.toscrape.com/").content

soup = BeautifulSoup(data, "html.parser")

prices = soup.select(price_selector)
titles = soup.select(title_selector)
ratings = soup.select(rating_selector)

for price, title, rating in zip(prices, titles, ratings):
    pass` 
```

我们现在可以处理每个标签来获得我们想要的值。对于`price`，这很容易:我们只需要找到标签的`string`属性。

Title 稍微难一点，但是仍然可以管理，因为我们有一个包含完整标题的属性。正如我们在`BeautifulSoup`的简短介绍中所讨论的，我们可以像字典一样对待标签，像键一样访问属性值。

```py
`import requests
from bs4 import BeautifulSoup

price_selector = ".price_color"
title_selector = ".product_pod h3 a"
rating_selector = ".star-rating"

data = requests.get("http://books.toscrape.com/").content

soup = BeautifulSoup(data, "html.parser")

prices = soup.select(price_selector)
titles = soup.select(title_selector)
ratings = soup.select(rating_selector)

for price, title, rating in zip(prices, titles, ratings):
    print(f"{title['title']} costs {price.string}")` 
```

谜题的最后一块是收视率。为了将类名转换成实际的星级，我将使用一个函数。

我还将定义一个字典，以便我们可以将给定的类名映射到更有用的东西。在这种情况下，我将把类名映射到由`"★"`字符组成的字符串。

```py
`rating_mappings = {
    "One":   "★",
    "Two":   "★ ★",
    "Three": "★ ★ ★",
    "Four":  "★ ★ ★ ★",
    "Five":  "★ ★ ★ ★ ★"
}

def get_rating(tag):
    for term, rating in rating_mappings.items():
        if term in tag["class"]:
            return rating` 
```

我上面定义的函数接收一个标签，然后使用我们的`rating_mappings`字典来确定哪个术语是这个`Tag`中使用的类名。

我们通过检查我们的`rating_mappings`键是否是与这个`Tag`的`"class"`键相关联的列表的成员来做到这一点。

如果找到匹配，我们返回与我们的`rating_mappings`字典中的关键字相关联的星级字符串。

```py
`import requests
from bs4 import BeautifulSoup

def get_rating(tag):
    for term, rating in rating_mappings.items():
        if term in tag["class"]:
            return rating

rating_mappings = {
    "One":   "★",
    "Two":   "★ ★",
    "Three": "★ ★ ★",
    "Four":  "★ ★ ★ ★",
    "Five":  "★ ★ ★ ★ ★"
}

price_selector = ".price_color"
title_selector = ".product_pod h3 a"
rating_selector = ".star-rating"

data = requests.get("http://books.toscrape.com/").content

soup = BeautifulSoup(data, "html.parser")

prices = soup.select(price_selector)
titles = soup.select(title_selector)
ratings = soup.select(rating_selector)

for price, title, rating in zip(prices, titles, ratings):
    print(f"{title['title']} costs {price.string} - {get_rating(rating)}")` 
```

一旦我们确认一切正常，我们就可以修改我们的`for`循环，这样我们就可以写入一个文件。在这种情况下，因为我使用了一个特殊的`★`字符，我们需要显式地为我们的文件指定一个编码。

```py
`import requests
from bs4 import BeautifulSoup

def get_rating(tag):
    for term, rating in rating_mappings.items():
        if term in tag["class"]:
            return rating

rating_mappings = {
    "One":   "★",
    "Two":   "★ ★",
    "Three": "★ ★ ★",
    "Four":  "★ ★ ★ ★",
    "Five":  "★ ★ ★ ★ ★"
}

price_selector = ".price_color"
title_selector = ".product_pod h3 a"
rating_selector = ".star-rating"

data = requests.get("http://books.toscrape.com/").content

soup = BeautifulSoup(data, "html.parser")

prices = soup.select(price_selector)
titles = soup.select(title_selector)
ratings = soup.select(rating_selector)

with open("books.csv", "w", encoding="utf-8") as book_file:
    for price, title, rating in zip(prices, titles, ratings):
        book_file.write(f"{title['title']},{price.string},{get_rating(rating)}\n")` 
```

就这样，我们结束了！

如果我们看一下我们的`books.csv`文件，我们现在有这样的内容:

```py
`A Light in the Attic,£51.77,★ ★ ★
Tipping the Velvet,£53.74,★
Soumission,£50.10,★
Sharp Objects,£47.82,★ ★ ★ ★
Sapiens: A Brief History of Humankind,£54.23,★ ★ ★ ★ ★
The Requiem Red,£22.65,★
The Dirty Little Secrets of Getting Your Dream Job,£33.34,★ ★ ★ ★
The Coming Woman: A Novel Based on the Life of the Infamous Feminist, Victoria Woodhull,£17.93,★ ★ ★
The Boys in the Boat: Nine Americans and Their Epic Quest for Gold at the 1936 Berlin Olympics,£22.60,★ ★ ★ ★
The Black Maria,£52.15,★
Starving Hearts (Triangular Trade Trilogy, #1),£13.99,★ ★
Shakespeare's Sonnets,£20.66,★ ★ ★ ★
Set Me Free,£17.46,★ ★ ★ ★ ★
Scott Pilgrim's Precious Little Life (Scott Pilgrim #1),£52.29,★ ★ ★ ★ ★
Rip it Up and Start Again,£35.02,★ ★ ★ ★ ★
Our Band Could Be Your Life: Scenes from the American Indie Underground, 1981-1991,£57.25,★ ★ ★
Olio,£23.88,★
Mesaerion: The Best Science Fiction Stories 1800-1849,£37.59,★
Libertarianism for Beginners,£51.33,★ ★
It's Only the Himalayas,£45.17,★ ★` 
```

如果你想扩展这个项目一点点，尝试刮多页。网站上有 50 页的书，分页有一致的 URL 格式，所以如果你想的话，你可以一次抓取一页。

您可能还想尝试抓取一些书籍的详细页面，以获取特定书籍的更多信息。