# 用 Flask 进行密码加密和认证

> 原文：<https://blog.teclado.com/learn-python-password-encryption-with-flask/>

## 为什么要加密？

当你开始制作软件时，不担心许多事情是正常的:性能、安全性，甚至是你制作的产品。然而，如果你有一个人们开始使用的产品，你有责任让他们的数据安全。

加密用于更改数据，因此除非读者拥有特权信息，否则无法读取数据。它主要与密码一起使用，这样如果您的应用程序被黑客入侵，他们就无法访问您的用户提供给您的密码。

它还可以与其他 PII(个人身份信息)一起使用，如护照号码、信用卡详细信息等。

## 什么是加密？

经常相互混淆的四个词是*加密*、*编码*、*散列*和*混淆*。虽然相似，但它们的目的不同，行为也不同。

当我们加密某个东西时，我们可以在以后解密它。为此，我们使用了一个**键**，这是另一个应该安全保存的数据。

当我们对某个东西进行编码时，我们可以通过使用与进行编码的算法相同或相似的算法来轻松解码。编码用于将数据从一种格式转换为另一种格式(例如，将文本编码为 ASCII 值)。

当我们散列某样东西时，我们不能“解开”它。您可以对一些数据进行哈希运算，得到一长串字符。如果您更改数据并再次对其进行哈希处理，字符串也会发生变化。因此，它可以用来很容易地比较数据，只需比较哈希值。

当我们混淆某事时，意思是让它变得难以理解。例如，一些混淆技术可能是缩小或改变源代码，使它看起来做一些不同于它实际做的事情；因此更难理解。混淆是有限制的，因为内容必须仍然是可读的，无论是谁想要使用它，例如，混淆的数据必须仍然有效，混淆的计算机代码必须仍然可以由机器运行。

## 使用 Werkzeug 散列

如果您有一个 Flask 和 Python 应用程序，并且您想快速开始散列 PII(这样您以后就不能取消散列)，您可以通过使用附带一组加密函数的 Flask 依赖项来这样做:`werkzeug`。

> 想用 Python 加密，但不用 Flask？查看我们的另一篇博文。

```
from werkzeug.security import generate_password_hash, check_password_hash

...

@app.route('/register', methods=['POST'])
def register_user():
    username = request.form['username']
    password = request.form['password']

    try:
        User(username, generate_password_hash(password)).save_to_db()
    except:
        return jsonify({'error': 'An error occurred saving the user to the database'}), 500

    return jsonify({'message': 'User registered successfully'}), 201 
```

这段代码将从请求的表单数据中获取`username`和`password`，并创建一个新的`User`对象(可能在别处定义了)，然后将其保存到数据库中。

代码假设`User`对象在其`__init__`方法中接受用户名和密码，并且它有一个将自身保存到数据库的方法。

如需更完整的示例，请查看此链接:[https://gist . github . com/jslvtr/139 cf 76 db 7132 b 53 F2 b 20 C5 b 6 a9 fa 7 ad](https://gist.github.com/jslvtr/139cf76db7132b53f2b20c5b6a9fa7ad)

* * *

附言:如果你有兴趣学习更多关于 Flask 和 Python 的知识，请注册我们下面的邮件列表！