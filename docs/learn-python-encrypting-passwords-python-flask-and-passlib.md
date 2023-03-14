# 用 passlib 在 Python 中加密密码

> 原文：<https://blog.teclado.com/learn-python-encrypting-passwords-python-flask-and-passlib/>

处理敏感数据或密码时，加密是必须的。在[之前的一篇博客文章](https://blog.teclado.com/learn-python-password-encryption-with-flask/)中，我们已经研究了使用`werkzeug`加密，这是 Flask 附带的。在本帖中，我们将更进一步，使用一个流行的加密库`passlib`。

不依赖 werkzeug 意味着你可以把这篇博文中的任何东西应用到任何 Python 应用程序中——而不仅仅是 Flask 应用程序。

我建议阅读我以前关于加密的博客文章，了解什么是加密以及为什么它很重要。这个也是另一个很好的关于密码学的快速指南。

## 哈希算法

有许多不同的哈希算法，我们正在不断地寻找和提出新的更好的算法。过去使用的一些现在被认为是不安全的，最现代的通常被认为是无法破解的。

> 哈希与加密。一个简单的解释就是，如果你加密了一个东西，你就可以解密它。如果你散列了某样东西，你就不能“取消散列”。对于密码，我们对它们进行哈希处理，因为我们不希望任何人能够将它们还原成可读的文本。继续阅读，看看如何检查散列密码是正确的！

目前出现的一些散列算法有:

*   氩气 2
*   PBKDF2
*   Bcrypt / Scrypt

它们在硬件要求、速度和实现上是不同的。然而，它们都极难破解。

我不建议学习哈希算法如何工作的所有知识，因为这是一个非常复杂的领域(充满了数学！).Argon2 是一个非常现代、经过严格审查、看起来很全面的算法。但是，在使用它时，您可能会发现一些操作系统中的一些问题。

由于加密和哈希的最大问题是软件开发人员没有充分使用它，因此任何选择都比没有选择好。我会推荐使用 PBKDF2 或 Argon2，因为它们都比 Bcrypt“好”,并且不像 Scrypt 那样消耗硬件。

为了简单起见，我们将使用 pbk df 2——因为它适用于所有操作系统和主机提供商。

我们将使用 passlib 作为我们的 Python 库来实现散列。他们有一份关于如何选择散列算法的优秀指南。

## 使用密码库

要安装 passlib，我们需要做的就是:

```
pip install passlib 
```

您可能会收到一些关于缺少库的警告。这些将用于其他加密和哈希算法。如果您想使用 PBKDF2 以外的东西，请查看 Passlib 文档中的[可选库部分](http://passlib.readthedocs.io/en/stable/install.html#optional-libraries),了解您需要哪一个(哪些)库。

### `CryptContext`

Passlib 通过定义上下文来工作。在其中，我们指定我们将使用什么算法进行散列，以及任何配置参数。

我们可以跳过使用`CryptContext`，而是直接与哈希算法交互。Passlib 提供了实现这一点的方法。然而，使用`CryptContext`允许我们更明确地定义配置参数，并使事情更具可读性。此外，如果我们的系统需要，它允许我们同时支持多种算法。

使用 PBKDF2 创建上下文如下所示。将以下代码放入一个新文件(例如`security.py`):

```
from passlib.context import CryptContext

pwd_context = CryptContext(
        schemes=["pbkdf2_sha256"],
        default="pbkdf2_sha256",
        pbkdf2_sha256__default_rounds=30000
) 
```

### 什么是回合？

一个[回合](https://crypto.stackexchange.com/a/6291)是算法的一部分，为了减少“可破解性”而运行多次。不同的算法推荐使用不同的回合数。

这个是一个来自 passlib 开发者的精彩的 StackOverflow 回答，它深入讨论了关于轮次的更多细节，以及如何计算选择多少轮次。

根据您选择的轮数，算法将需要更长的加密时间。因此，您的用户将不得不等待更长时间来恢复您的应用程序。选择一个太大的数字，你的用户会感到沮丧。选择一个太低的数字，你的用户数据将不安全。

一个经验法则是让用户等待最多 350 毫秒——这样你就可以微调回合数，这样你的算法就不会花费超过这个时间。

[这里有一个解释](https://security.stackexchange.com/a/16357)为什么等待 350 毫秒是好的。

### 加密和验证密码

现在我们有了我们的`CryptContext`，我们可以用它来加密和验证密码。让我们在创建上下文的同一个文件中添加几个函数:

```
def encrypt_password(password):
    return pwd_context.encrypt(password)

def check_encrypted_password(password, hashed):
    return pwd_context.verify(password, hashed) 
```

就是这样！每当我们想在数据库中存储一个用户的密码时，我们应该首先使用我们的`encrypt_password()`函数对它进行加密。每当我们想让一个用户登录时，我们会使用`check_encrypted_password()`函数将他们在登录表单中提供的密码与存储在数据库中的密码进行比较。

请注意，在任何时候我们都不会在数据库中“取消”密码。我们只需对想要检查的密码使用算法，然后将它变成一个新的散列。如果数据库中的密码和我们创建的新密码相同，我们就知道密码是正确的！

最后更新于 2019 年 5 月。本帖最初发表于 2017 年 12 月。