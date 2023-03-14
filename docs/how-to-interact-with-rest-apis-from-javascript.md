# 如何从 JavaScript 与 REST APIs 交互

> 原文：<https://blog.teclado.com/how-to-interact-with-rest-apis-from-javascript/>

这篇文章是对我们 REST API 开发课程的学生的一个常见问题的回应。

在那里，我们教如何使用 Python 创建 REST APIs，但不教如何使用它们。通常你会创建一个 REST API 来标准化和共享对资源的访问，比如数据库。例如，您可能希望允许移动应用程序和 web 应用程序通过 REST API 访问您的数据库。

当然，为了做到这一点，您需要知道如何在您的移动应用程序或 web 应用程序中使用 REST API！

在这篇文章中，我们将看看如何使用 JavaScript 与 REST API 交互，以便您的 web 应用程序使用它。

如果你想继续学习，[这里有一个 GitHub 库](https://github.com/tecladocode/interacting-with-apis-javascript),里面有一个 Flask 示例应用程序和这篇博文中提到的所有 JavaScript 示例。

## “互动”是什么意思

在 REST APIs 的上下文中，“交互”意味着向 API 发出请求并获得响应。

那么，我们如何使用 JavaScript 向 REST API 发出请求呢？

有几种方法可以做到。

### 用普通的 JavaScript 发出 HTTP 请求

用 JavaScript *发出 HTTP 请求而不使用*任何库的方法需要大量的输入，所以通常你不会使用这种方法。不过，反正知道了还是好的！

下面是如何使用普通 JavaScript 执行 GET 请求:

```
const request = new XMLHttpRequest();
const url = 'http://localhost:5000/movies';
request.open("GET", url);
request.send();

request.onload = (e) => {
    alert(request.response);
} 
```

这里我们首先创建一个新的`XMLHttpRequest`。这是保存我们正在进行的 HTTP 请求的数据，然后将它发送给 API(我在本地运行，因此 url 在`localhost`中)。

然后我们打开请求，传递方法和 URL，这里的方法是`"GET"`，从 API 中检索数据时经常用到。

`XMLHttpRequest`是异步的，这意味着在收到响应之前，运行`.send()`不会阻塞应用程序。相反，接收和处理响应可以通过一个事件处理程序来完成。

这就是`.onload`的作用。将一个函数定义为该属性的值，它将在响应到达时运行。

### 用普通 JavaScript 发送表单数据

发送表单数据有两种主要方式:使用 HTML 和使用 JavaScript。

如果可以，用 HTML 发送表单数据将会简单得多。为此，您需要一个包含表单和提交按钮的 HTML 页面。然后，当您按下 submit 按钮时，HTML 会为您构建并发送请求。

大概是这样的:

```
<!DOCTYPE html>
<html>
    <head>

    </head>
    <body>
        <form action="http://localhost:5000" method="POST">
            <label>
                Title: <input type="text" name="title" placeholder="Movie name" />
            </label>
            <label>
                Year: <input type="text" name="year" placeholder="1994" />
            </label>
            <input type="submit" value="Submit" />
        </form>
    </body>
</html> 
```

注意，这根本没有使用 JavaScript。但是，如果您希望发送代码在您的 JavaScript 中，而不是使用 HTML，您可以这样做。

您可以这样做，使用`XMLHttpRequest`:

```
const formData = new FormData();

formData.append("title", "The Matrix");
formData.append("year", "1999");

const response = new XMLHttpRequest();
response.open("POST", "http://localhost:5000/");
response.send(formData);

response.onload = (e) => {
    alert(response.response);
} 
```

这看起来与我们之前的相似，但是现在我们首先创建一个`FormData`对象，并向它追加我们想要的字段。

那么当我们`.open()`的时候，我们说方法是`"POST"`。这就是我们的示例 API 在接收数据时所期望的，但是在您的 API 中可能会有所不同。

最后，当执行`.send()`时，确保包含表单数据，以便它被发送到 API。

### 用普通 JavaScript 发送 JSON 数据

发送 JSON 数据仅仅意味着发送一个字符串，并告诉 API 这个字符串代表什么。

因此，在这个例子中，我们将发送包含 JSON 对象的字符串，以及一个*头*，告诉 API 我们的数据是 JSON 数据。

```
const response = new XMLHttpRequest();

const json = JSON.stringify({
    title: "The Matrix",
    year: "1999"
});

response.open("POST", 'http://localhost:5000/')
response.setRequestHeader('Content-Type', 'application/json');

response.send(json);

response.onload = (e) => {
    alert(response.response);
} 
```

我们需要设置的头是`Content-Type`，值是`application/json`。这使得我们的 API 知道它正在接收 JSON 数据。

然而，我们在`.send()`中使用的数据不能是 JavaScript 对象。它必须是一个字符串——你可以通过做`JSON.stringify()`把 JavaScript 对象变成字符串。

### 用 JavaScript 的 fetch 获取数据

较新版本的 JavaScript(称为 ES6)带有一个名为`fetch()`的函数，它极大地简化并改进了发送和接收请求和响应的传统方式。

要使用`fetch()`而不是`XMLHttpRequest`来检索数据，您必须这样做。一定要确保运行这段代码的浏览器支持 ES6。Chrome 是个不错的选择。

```
const url = 'http://localhost:5000/movies';
fetch(url)
.then(data => data.json())
.then((json) => {
    alert(JSON.stringify(json));
}) 
```

如果你不确定你的用户的浏览器是否支持`fetch()`，通常我们所做的是通过使用一个叫做 transpiler 的工具把现代的 JavaScript 代码转换成旧的代码。

在这段代码中，我们使用 ES6 中承诺的`.then()`语法来告诉 JavaScript 在响应到达时做什么，而不是使用事件处理程序。

这更干净，并且将与响应处理相关的代码与发出请求的代码放在同一个地方。

### 用 JavaScript 的 fetch 发送 JSON

用`fetch()`发送数据可能看起来相当冗长——就像用`XMLHttpRequest`一样，但是我认为当你知道它的意思时，它更容易阅读！

看看这个:

```
const url = 'http://localhost:5000/';
const data = { title: "The Matrix", year: "1994" };

fetch(
    url,
    {
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
        method: "POST"
    }
)
.then(data => data.json())
.then((json) => {
    alert(JSON.stringify(json));
}); 
```

这段代码的主要部分是我们用两个参数调用`fetch()`:`url`和`options`。

`options`包括`headers`、`body`和`method`。这包含了我们的请求成功发送所需的所有内容。

因为所有的选项都在一个地方，所以我认为它比`XMLHttpRequest`更具可读性。

## 结论

还有更多的库可以用来用 JavaScript 发送 HTTP 请求，比如 jQuery、axios、Angular 的 HttpClient 等等。然而，只要有可能，我会坚持使用`fetch()`，因为这是新的、现代的、推荐的方法。

感谢您阅读今天的博文，我希望它对您的 REST API 开发有用！

如果你想学习如何创建 REST API，请查看我们的[REST API 与 Flask 和 Python 课程](https://go.tecla.do/rest-apis-sale)。此外，注册我们的邮件列表，因为我们每个月都与我们的订户共享折扣代码！

下次见！