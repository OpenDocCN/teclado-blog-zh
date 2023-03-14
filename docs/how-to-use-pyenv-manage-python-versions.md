# 如何使用 pyenv 管理 Python 版本

> 原文：<https://blog.teclado.com/how-to-use-pyenv-manage-python-versions/>

作为一名 Python 开发人员，当新的 Python 版本出来时，我会安装它们。我们获得了新的功能和性能改进，但我们并不总是希望对所有东西都使用最新版本。一些项目使用较旧的 Python 版本，而其他项目必须使用特定的 Python 版本。

如果您需要安装多个 Python 版本，走 ol' installer 路线不是最好的主意。最终在您的`PATH`中会有多个 Python 可执行文件，比如`python`、`python2`、`python3`、`python3.7`等等。区分次要版本也变得非常棘手，比如`python3.10.3`和`python3.10.4`。

于是， [`pyenv`](https://github.com/pyenv/pyenv) 来救援了！这个命令行工具将处理特定 Python 版本的安装和运行！

他们的自述文件实际上解释了[如何工作](https://github.com/pyenv/pyenv#how-it-works)和[如何很好地安装](https://github.com/pyenv/pyenv#installation)。读一下吧！

值得注意的是，对于 Windows，我经常使用 [`pyenv-win`项目](https://github.com/pyenv-win/pyenv-win)，这对我很有效。

为了在 MacOS 上安装`pyenv`,我选择了自制软件:

```
brew update
brew install pyenv 
```

安装后，你必须添加一些东西到你的外壳，以配置它。详细说明在[自述文件](https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv)中。

## 我实际上如何使用`pyenv`

对我来说,`pyenv`的伟大之处在于它很好用，而且我只在一个项目的最开始使用它来创建一个虚拟环境。

一旦我使用一个特定的 Python 版本(从`pyenv`获得)创建了一个虚拟环境，从那时起我只需激活虚拟环境来获得那个版本，我就不必再用`pyenv`瞎折腾了。

那么，你是怎么做到的呢？

首先，查看您想要的 Python 版本是否可用(您可能需要更新`pyenv`才能看到最近的版本，我使用`brew update && brew upgrade pyenv`):

```
pyenv install --list 
```

这将向您显示一个很长的输出，其中可能包含如下内容:

```
...
3.10.1
3.10.2
3.10.3
3.10.4
3.10.5
3.10.6
3.10.7
3.11.0rc2
3.11-dev
3.12-dev
... 
```

现在你知道了，你可以从这些版本安装(再次提醒你更新以查看最近添加的版本)。

要安装 Python 版本:

```
pyenv install 3.10.7 
```

## 选择 Python 版本

您可以使用以下命令查看当前选择的 Python 版本:

```
pyenv version 
```

您可以看到所有版本(包括当前选定的版本),包括:

```
pyenv versions 
```

你可以有一个**全球选择的** Python 版本，然后**本地选择的** Python 版本。如果没有选择本地版本，则使用全局版本。

您可以使用以下选项选择本地版本:

```
pyenv local 3.10.7 
```

这将在当前文件夹中创建一个`.python-version`文件，告诉`pyenv`使用那个 Python 版本。

如果要更改全局 Python 版本:

```
pyenv global 3.10.7 
```

## 使用所选的 Python 版本

现在，让我们使用`pyenv`创建一个虚拟环境:

```
pyenv exec python -m venv .venv 
```

这使用了`pyenv exec`来运行`python`命令，在我们的例子中它将使用 Python 3.10.7。传递给`python`的`-m venv .venv`参数告诉它运行`venv`模块，并将其命名为`.venv`。

这将在名为`.venv`的文件夹中创建一个虚拟环境。

而且说实话，我就是这么用`pyenv`的。就这样，仅此而已！

如果您想在多个 Python 版本中测试您的应用程序，您也可以很容易地做到。Real Python 有一篇很有深度的博文[介绍了这一点，所以请随意查看。](https://realpython.com/intro-to-pyenv/#activating-multiple-versions-simultaneously)

就这些了！我希望你觉得这很有趣，并且学到了一些新东西。如果你想更深入地研究 Python 和它所提供的一切，请考虑加入我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)。这个大型课程涵盖了许多 Python 主题，目前正在出售中！

感谢阅读，我们下次再见。