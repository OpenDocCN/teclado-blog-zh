# 如何在 Mac、Linux 和 Windows 上安装 NodeJS

> 原文：<https://blog.teclado.com/how-to-install-nodejs-on-mac-linux-and-windows/>

安装 NodeJS 可能是一个痛苦和令人困惑的经历。有官方安装程序，包管理器，脚本...你应该用哪一个？

## 在 UNIX 上(OS X 和 Linux)

如果您还没有安装 NodeJS 的版本，最简单的安装方法是使用节点版本管理器。

官方指令是[页面中的](https://github.com/creationix/nvm#installation)，或者在控制台中执行以下命令:

```py
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash 
```

然后，重启终端并执行`nvm`命令。您应该会看到除“未找到命令”之外的内容。

如果没有，运行`touch ~/.bash_profile`并再次运行安装脚本:

```py
touch ~/.bash_profile

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash 
```

如果这仍然不能解决您的问题，那么运行以下命令并重新启动您的终端:

```py
echo "source ~/.bashrc" >> ~/.bash_profile 
```

现在，我们准备使用 nvm 安装最新版本的 Node:

```py
nvm install node 
```

> 如果这是您随 nvm 安装的第一个 node 版本，它将自动成为默认版本。例如，如果您想更改默认设置，可以通过`nvm alias default 8.5.0`来实现。

## 在 Windows 上

如果你只需要 NodeJS 的一个版本(即你不需要任何东西的多个版本)，那么[官方安装程序](https://nodejs.org/en/download/)是 Windows 中最简单的方法。

一旦安装完毕，你应该能够打开`cmd.exe`并运行`node`和`npm`命令！