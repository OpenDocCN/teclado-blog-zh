# 使用 Python 与 REST APIs 交互

> 原文：<https://blog.teclado.com/learn-python-interacting-with-rest-apis-with-python/>

互联网上的 REST APIs 使用 HTTP 进行通信。这意味着无论何时我们想要向 REST API 发送一些数据或者从它那里接收一些数据，我们都必须使用 HTTP。

在 HTTP 中，客户端通过发送**请求**与服务器通信；服务器用**响应**进行响应。当我们谈论与 REST API 交互时，我们就是客户端。服务器就是我们与之对话的 API。

例如，作为一个客户端，你可以与 Twitter API 或者脸书 API 进行通信，等等。

> 如果你正在考虑创建自己的 API，以便客户可以与你交流(例如，你自己的移动应用)，你可能会对我的[综合在线课程](https://www.udemy.com/rest-api-flask-and-python/?couponCode=TECLADO_BLOG)感兴趣。

## 向 API 发送请求

假设我们有想要与之通信的示例 API，`http://api.example.com/`。我们可以通过发送一个 HTTP 请求与它进行交互。

> 当向类似于`http://api.example.com/`的 URL 发出请求时，TLD ( `.com`)之后的所有内容都被称为端点。在这种情况下，端点是`/`。

API 文档将为您提供关于向哪个端点请求特定数据的信息。

### 用 Python 发出 GET 请求

您必须安装流行的`requests`库，以便用 Python 轻松地发出 HTTP 请求:

```
pip install requests 
```

然后，您将能够执行如下所示的文件来发出 GET 请求:

```
import requests

resp = requests.get('http://api.example.com/')
print(resp.json()) 
```

### 用 Python 发出 POST 请求

POST 请求通常用于向服务器发送数据。例如，您可以发送一些 JSON 数据:

```
import requests

payload = {'name': 'Example'}

resp = requests.post('http://api.example.com/', json=payload)
print(resp.json()) 
```

如果需要，您还可以包括自定义标题:

```
import requests

payload = {'name': 'Example'}
headers = {'Content-Type': 'application/json'}

resp = requests.post('http://api.example.com/', json=payload, headers=headers)
print(resp.json()) 
```

您可以发送表单数据而不是 JSON，只需改为执行`requests.post('http://api.example.com/', data={'field': 'value'}`即可。

大多数 REST APIs 将要求您使用 API 密钥进行身份验证。这通常包含在 URL、请求正文或标题中。以下是一些关于如何包含 API 授权的示例:

在 URL 中:

```
import requests

API_KEY = '123abc'

resp = requests.post('http://api.example.com/?key=' + API_KEY)
print(resp.json()) 
```

在有效载荷中:

```
import requests

payload = {'key': '123abc'}

resp = requests.post('http://api.example.com/', json=payload)
print(resp.json()) 
```

在标题中:

```
import requests

headers = {'Authorization': 'Bearer 123abc'}  # Having 'Bearer' is common. Check your API documentation to see if it is required!

resp = requests.post('http://api.example.com/', headers=headers)
print(resp.json()) 
```