# 如何连接到 Linux 服务器

> 原文：<https://blog.teclado.com/connecting-to-linux-servers/>

我之前提到过，如果您将应用程序部署到服务器上，它很可能是 Linux。因此，能够连接到您的 Linux 服务器并理解该连接是如何工作的非常重要。

这篇文章是[“理解控制台”](https://blog.teclado.com/tag/understanding-the-console/)系列的一部分，旨在帮助理解 Linux 和文本控制台。

> 请记住，当您租用服务器时，您租用的是一台真正的计算机。当您连接到它时，您就连接到了一台计算机，因此它的行为就像您的台式机一样。

如果你目前没有服务器，但你想要一个，你可以遵循[这个指南](https://medium.com/school-of-code/deployment-of-a-flask-application-to-digitalocean-dfecc04a723b#.gs58nhhu0)。

连接到服务器通常有两种方式:

*   用户名和密码；或者
*   SSH 键。

在所有情况下，您都可以从计算机上使用以下命令登录:

```
ssh <user>@<ip> 
```

如果您尝试登录的用户没有有效的 SSH 密钥，系统会提示您输入密码。如果您尝试登录的用户拥有有效的 SSH 密钥，那么登录就可以了，不需要密码。

## 用户名和密码

这是两种方法中最不安全的一种，因为它不需要别人知道你的用户名和密码。但是，它不需要太多的设置，只要您知道用户名和密码，就可以从任何地方访问服务器。

## SSH 密钥

这更安全，但它依赖于没有人偷你的电脑或你的私人钥匙。

SSH 密钥以每次登录为基础工作。假设您在服务器上创建了一个名为`jose`的帐户。然后，您可以将一个 SSH 密钥放入该帐户，这样您就可以从您的计算机直接登录到该帐户，而无需使用密码。SSH 密钥不适用于其他帐户。

在你的服务器中，SSH 密钥在`~/.ssh/`文件夹中，正如你[已经知道的](https://blog.teclado.com/basic-linux-console-commands/)，`~`是用户的主文件夹。

### 生成 SSH 密钥

从您的计算机上，您可以使用下面的命令生成 SSH 密钥(如果您使用的是 Mac 或 Linux):

```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/yourname/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
(long number here)
The key's randomart image is:
(random art here) 
```

我建议只接受默认文件，然后输入一个您会记住的密码。这是一种保护您的 SSH 密钥的方法，这样即使有人获得了您电脑的访问权限，他们也无法使用 SSH 密钥。

如果你使用的是 Windows，安装[https://Git-for-Windows . github . io](https://git-for-windows.github.io)，然后你就可以从一个名为“Git Shell”的程序运行上面的命令了。

### 向服务器添加 SSH 密钥

使用用户名和密码登录到服务器。然后，复制 SSH 密钥文件的内容(在上例中为`/Users/yourname/.ssh/id_rsa`)，然后:

```
vi ~/.ssh/authorized_keys 
```

然后，按下`i`并粘贴 SSH 密钥的内容。

现在，您的 SSH 密钥将被该服务器中的用户接受。下次连接到服务器时，不会要求您输入密码。