# 基本 Linux 控制台命令

> 原文：<https://blog.teclado.com/basic-linux-console-commands/>

就像浏览桌面一样，能够浏览控制台也很重要。

本帖是[“sudo 是什么？”](http://jslvtr.com/what-is-sudo)系列，旨在帮助理解 Linux 和文本控制台。

## 处理文件

除了根文件夹之外，系统中的每个文件夹都在另一个文件夹中。这个文件夹是父文件夹。根文件夹，或最顶层的文件夹，是包含系统中所有内容的文件夹。

根文件夹是:`/`。

您系统中的一些用户已经关联了“个人”文件夹。你可以把这个文件夹想象成用户通常放他们自己的“文档”、“照片”等的地方。

无论何时使用系统，您都必须以特定用户的身份登录。我们来看看你现在的用户是什么。

打开控制台，键入以下命令:

```
whoami 
```

这将告诉您当前登录的用户名。

接下来，让我们导航到您用户的“个人”文件夹:

```
cd ~ 
```

`cd`命令执行一个 **c** 焊割 **d** 工厂命令。`~`文件夹是当前用户的“个人”文件夹。

### 创建文件和文件夹

要创建一个文件夹，我们使用`mkdir`命令。
创建一个空文件，我们使用`touch`命令。

```
# This creates a folder 'documents' inside your home folder.
mkdir ~/documents

# This creates an empty file 'hello.txt' in the documents folder.
touch ~/documents/hello.txt 
```

如果我们想在另一个文件夹中创建一个文件夹，而这两个文件夹都不存在:

```
mkdir -p ~/documents/work/journal 
```

这会创建一个名为“work”的文件夹，然后在其中创建一个名为“journal”的文件夹。如果我们省略`-p`，那么它会给出一个错误，因为它试图在一个尚不存在的文件夹中创建‘journal’文件夹(‘work’)。

### 删除文件和文件夹

最后，让我们看看删除。我们使用`rm`命令删除。

```
# This deletes the file 'hello.txt'
rm ~/documents/hello.txt
# This deletes the folder 'work', including everything it contains
rm -rf ~/documents/work 
```

删除文件夹需要`-rf`参数。这是因为我们希望删除文件夹中的所有内容，以及文件夹。`r`代表“递归”,它告诉`rm`命令进入每个子文件夹并删除其中的所有内容。`f`参数试图删除文件，即使它们不属于当前登录的用户。

## 回顾

*   `whoami`显示当前登录的用户。
*   `cd [args]`将目录切换到另一个目录。
*   创建一个目录。
*   创建一个空文件。
*   `rm [-rf] [args]`删除文件或目录。