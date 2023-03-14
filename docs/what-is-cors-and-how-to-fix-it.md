# 什么是 CORS，如何“修复”它

> 原文：<https://blog.teclado.com/what-is-cors-and-how-to-fix-it/>

# **简介**

> 对'[https://www.yourapi.com](https://www.yourapi.com/)'处提取的访问已被 CORS 策略阻止:对预检请求的响应未通过访问控制检查:请求的资源上不存在“Access-Control-Allow-Origin”标头。如果不透明响应满足您的需要，请将请求的模式设置为“no-cors ”,以便在禁用 cors 的情况下获取资源。

如果您曾经构建过想要与 REST API 交互的 web 应用程序，您可能对这个错误很熟悉。如果您也构建了 API，您可能会想:为什么在测试 API 时它能与 Postman 一起工作，而在浏览器中却不能？我的网络应用肯定有问题。然而，事实很可能并非如此。

这篇文章将会给你一个什么是 CORS 的总体概念，以及如何解决这个问题。

# **CORS**

## **什么是 CORS**

**CORS** 代表跨产地资源共享。这是一种限制来自不同来源(域)的请求的机制。来自不同来源的请求被称为跨来源请求。当您的站点需要从其他服务加载数据时，跨源请求至关重要。

CORS 允许服务器指定谁可以访问他们的资源以及如何访问。浏览器遵循服务器的策略，向服务器发送一个*测试*请求(预检)并检查它是否被允许。我将在接下来的几节中进行更详细的介绍。

## **为什么是 CORS**

其背后的基本原理是允许一个源上的服务器(API)限制其他源的行为。例如，允许一个源读取和写入数据，但其他源只能读取数据。因此，默认情况下，像 **GET** 这样的请求通常是允许的。但是，**放**，**删**，有时**贴**会受到限制。

## **CORS 是如何工作的**

当浏览器将要发送一个将触发 CORS 到不同来源的请求时，例如一个 **PUT** 请求，它不会立即发送实际的请求。取而代之的是，它将发送一个所谓的*飞行前*请求，作为关于服务器是否允许通信的*测试*。如果服务器以成功状态响应，浏览器将发送实际的请求。否则，它将拒绝请求，返回 405 状态代码和本文开头显示的错误消息。

## **更多关于 CORS 的信息**

到目前为止，我已经非常简要地介绍了 CORS。对于那些想要深入挖掘的人，你可以在 MDN 上找到对 CORS 更彻底的解释:[https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。

另一个好的读物是什么是 CORS？

# **如何让它工作**

CORS 问题可能非常烦人，尤其是对学习者而言。本文将总结一些解决方案，可以节省您在 StackOverflow 上搜索的时间。解决方案取决于您的特定场景:更具体地说，取决于您对服务器的控制程度。我们将从*最好的情况*场景到*最坏的情况*。

## **场景 1:完全控制服务器**

如果您是将 API 部署到您自己的服务器上的人，那么您处于最好的情况。假设您使用的是 **Nginx** ，那么您需要做的就是配置您的服务器。如果你不是，你仍然可以看看下面的场景，这些场景也可以解决这个问题。

这里有一段来自[https://enable-cors.org/server_nginx.html](https://enable-cors.org/server_nginx.html)的代码片段，展示了如何配置一个完全开放的 CORS 策略。

```py
if ($request_method = 'OPTIONS') {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';

  # Custom headers and headers various browsers *should* be OK with but aren't
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';

  # Tell client that this pre-flight info is valid for 20 days
  add_header 'Access-Control-Max-Age' 1728000;
  add_header 'Content-Type' 'text/plain; charset=utf-8';
  add_header 'Content-Length' 0;
  return 204;
}
if ($request_method = 'POST') {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
  add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
} 
```

你可以根据自己的需要定制。例如，您可以只允许您的 web 应用程序域，而不是对所有来源使用通配符。您也可以选择只允许特定的方法和头通过。

这个解决方案允许您在不篡改 API 代码的情况下配置 CORS，这在大多数情况下是理想的。如果这不是您想要的，或者您无法控制服务器，您可以在下一个场景中找到解决方案。例如，如果你通过 **Heroku** 部署你的 API。

# **场景 2:我的 API 而不是我的服务器**

在这个场景中，您完全部署了您的 API，但是您可能使用了类似于 **Heroku** 的托管服务，因此您无法控制服务器配置。本节将指导您在后端代码中添加必要的头部。

没有通用的服务器端语言或框架，但我们至少可以展示一个使用非常流行的选项的解决方案。

这里我要用一个 **Flask** app 进行演示，这是一个非常流行的 **Python** 框架(抱歉 **Django** 爱好者！).

```py
app = Flask(__name__)

@app.after_request
def after_request(response):
    response.headers.add('Access-Control-Allow-Origin', '*')
    response.headers.add('Access-Control-Allow-Headers', 'Content-Type')
    return response 
```

上面的代码块显示了如何在处理请求之后、发送响应之前添加额外的头。这与 Nginx 的情况相同:您可以将解决方案定制得不那么开放，只允许某些来源、方法或头文件。

# **场景 3:不是我的 API，也不是我的服务器**

前端开发人员可能处于“最坏”的情况。然而，这不是你的错，这是很常见的情况。你可能只是想与第三方 API 交互，但它一直抱怨 CORS。对此我们能做些什么？

理想的解决方案是通知 API 所有者这个问题，因为从技术上来说这是他们的错。但是，您可能不会立即得到回复。因为他们发现使用它没有问题，这是你自己的错误配置。如果这是你的情况，请参考这篇文章。但是，与此同时，您可以尝试以下解决方案。

您可以构建一个代理 API，作为通向第三方 API 的桥梁。这听起来有点奇怪，但如果你希望你的网络应用程序正常工作，这可能是唯一可行的解决方案。代理 API 将从您的 web 应用程序接收请求，并将它们发送给第三方 API。当第三方 API 响应时，它会将响应转发到您的 web 应用程序。由于对第三方 API 的请求不通过浏览器，它绕过了 CORS 保护。然后，您可以像前面两个场景一样配置自己的代理 API。

最后一种解决方案是使用浏览器插件将所需的头注入到响应中。不建议这样做，因为这只是“愚弄你自己”。浏览器插件不能在其他计算机上工作(比如你的用户)，除非他们也有插件。但是，如果您正在本地进行测试，并且希望尽快看到一些结果，它仍然是有用的。通常每个浏览器都有很多这样的插件。其中之一就是 Chrome 的[Allow-Control-Allow-Origin](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi)扩展。

也有关于如何在浏览器上关闭 CORS 检查的教程，但我们不会在这里覆盖它，因为它也不可伸缩。

# **结论**

这篇文章提供了一个非常高层次的关于 CORS 政策的观点。还有很多要学的，可以在顶部的[关于 CORS 的更多信息](#More-about-CORS)部分找到。我为不同的场景量身定制了解决方案，以帮助您找到适合您需求的解决方案。我希望这篇文章对您有所帮助，因为我自己也遇到过这些情况，并且希望有一篇像这样的文章。干杯！