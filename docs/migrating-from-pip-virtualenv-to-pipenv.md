# 从 pip + virtualenv 迁移到 Pipenv

> 原文：<https://blog.teclado.com/migrating-from-pip-virtualenv-to-pipenv/>

我们最近的课程《Flask 和 Python 的高级 REST APIs》使用了 Pipenv。

然而,“初学者”课程——使用 Flask 和 Python 的 REST APIs 使用了 virtualenv。

这篇博客旨在帮助第一门课的学生迁移到 Pipenv，因为第二门课会用到它。

## 为什么是 Pipenv？

对于本课程和小型应用程序开发来说，迁移到 Pipenv 有两个主要原因。在开发更大的应用程序时，有更多的理由进行迁移。

学习本课程的两个主要原因是:

*   使用 Pipenv 比使用 virtualenv 和 pip 更容易，因为它为您创建虚拟环境并管理它们；
*   使用 Pipenv 比 virtualenv 更安全，因为安装的每个依赖项都有一个 hash，保存在一个名为`Pipfile.lock`的文件中。下载依赖项时，如果它与该版本的哈希不匹配，它将无法工作。通过这种方式，您可以知道依赖项是否以某种方式被修改了，但是版本号没有改变。

### 更安全？

例如，如果有人在您不知道的情况下侵入依赖项的存储位置并修改了依赖项，那么依赖项的版本号可能不会改变。

恶意代码可能会在无人知晓的情况下被添加，这是非常危险的。

使用`Pipfile.lock`，每个依赖项都有一个从包内容生成的散列——所以如果包内容改变，散列也会改变。Pipenv 在安装时检查这一点，以确保您安装的是您认为的依赖项。

## 迁移到 Pipenv

迁移到 Pipenv 实际上非常容易。你要做的就是:

*   安装 Pipenv(`pip install pipenv`)；
*   在保存`requirements.txt`文件的同一个文件夹中运行`pipenv install`；
*   删除你的`requirements.txt`文件，因为现在你已经有了`Pipfile`和`Pipfile.lock`文件。

## 使用 Pipenv 运行您的应用

当使用 pip 和 virtualenv 时，我们通常会首先激活 virtualenv，然后运行我们的 Python 应用程序。

有了 Pipenv，可以做`pipenv run python app.py`来一气呵成。

或者，您可以通过运行`pipenv shell`来激活 virtualenv。然后以与使用 virtualenv 时相同的方式运行应用程序。

## 生成 requirements.txt 文件

一些软件服务可能需要提供`requirements.txt`文件(例如 ReadTheDocs、Heroku...).用 Pipenv 生成一个`requirements.txt`文件非常简单:

```
pipenv lock --requirements > requirements.txt 
```

## 其他阅读材料