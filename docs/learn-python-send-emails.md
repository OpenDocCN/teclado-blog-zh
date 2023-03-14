# 用 Python 发送电子邮件

> 原文：<https://blog.teclado.com/learn-python-send-emails/>

Python 附带了几个库，允许我们创建电子邮件并发送它们。这些是`email`和`smtp`库。

## 创建电子邮件

有了`email`库，使用 Python 创建电子邮件变得很容易。我们将只定义内容及其元数据(From、To、Subject)。

```py
from email.message import EmailMessage

email = EmailMessage()

email['Subject'] = 'Test email'
email['From'] = '[[email protected]](/cdn-cgi/l/email-protection)'
email['To'] = '[[email protected]](/cdn-cgi/l/email-protection)'

email.set_content('Hello, John') 
```

## 发送电子邮件

发送电子邮件可能有点复杂。电子邮件是通过 SMTP 发送的，所以我们需要一个 SMTP 服务器来处理邮件的发送。

如果您在公司环境中工作，您可能会访问 SMTP 服务器。如果你没有，你可以随时使用 Gmail 或 Hotmail 发送电子邮件。**警告**:不要使用个人账户发送大量电子邮件。如果许多人将你的邮件标记为垃圾邮件，你可能会看到你的个人邮件的投递率下降。

我们将使用`smtp`库发送电子邮件。

### 登录到 Gmail SMTP 服务器

在这个例子中，我们将使用一个假冒的 Gmail 帐户发送邮件。如果你愿意，你可以使用你自己的帐户！

```py
import smtplib

s = smtplib.SMTP(host='smtp.gmail.com', port=587)
s.starttls()
s.login('[[email protected]](/cdn-cgi/l/email-protection)', 'password') 
```

如果您在密码正确的情况下得到一个`SMTPAuthenticationError`，那么您可能启用了双因素身份验证。你需要使用一个[应用密码](https://support.google.com/accounts/answer/185833)来代替你的普通密码登录。

如果你没有启用 2-FA，你必须在你的 Gmail 安全首选项中允许不太安全的应用程序访问——尽管记住在你学习完使用 Python 发送电子邮件后要禁用它！

### 发送电子邮件

一旦我们收到`MIMEMultipart`消息并登录，我们就准备发送它。

```py
s.send_message(email)
s.quit() 
```

## 完全码

下面是您可以用 Python 发送电子邮件的完整代码。试着把它的一部分或全部放入函数中，这样重用它就变得非常简单了！

```py
import smtplib

from email.message import EmailMessage

email = EmailMessage()

email['Subject'] = 'Test email'
email['From'] = '[[email protected]](/cdn-cgi/l/email-protection)'
email['To'] = '[[email protected]](/cdn-cgi/l/email-protection)'

email.set_content('Hello, John')

s = smtplib.SMTP(host='smtp.gmail.com', port=587)
s.starttls()
s.login('[[email protected]](/cdn-cgi/l/email-protection)', 'password')

s.send_message(email)
s.quit() 
```

使用 Python 发送电子邮件非常简单！我们要做的就是:

1.  使用`EmailMessage`对象创建电子邮件；
2.  设置我们的 SMTP 服务器连接；
3.  发送消息！

享受从 Python 应用程序发送电子邮件的乐趣！如果你正在开发一个可以发送更多电子邮件的应用程序，那么研究一下像 [Mailgun](https://www.mailgun.com) 这样的电子邮件服务是值得的。