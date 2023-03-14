# 在 Linux 中访问文件(并介绍 Vi)

> 原文：<https://blog.teclado.com/contents-of-files-and-introduction-to-vi/>

先说文件的读取和修改。

本帖是[“sudo 是什么？”](http://jslvtr.com/what-is-sudo)系列，旨在帮助理解 Linux 和文本控制台。

## `cat`命令

使用`cat`命令，您可以查看文件的文本内容:

```
cat my_file.txt 
```

您也可以使用`cat`添加到一个文件，并覆盖一个文件。

**下面的代码将**“Hello，world”追加到文件`my_file.txt`中。

```
cat >> my_file.txt
Hello, world

[CTRL+C] 
```

**下面的代码用“Hello，world”这几个字覆盖了** `my_file.txt`。

```
cat > my_file.txt
Hello, world

[CTRL+C] 
```

## `vi`命令

> 注意，有时这个命令是`vim`(代表`vi`“改进”)。

Vi 是一个文本编辑器。您可以打开、编辑和保存文件。

它功能强大，但需要一些时间来适应。

像这样打开文件:

```
vi my_file.txt 
```

下面是命令的快速列表:

*   `i`进入“插入”模式。把这想象成你正常的文本编辑模式，在这里你可以写字符，使用退格键等等…
*   进入可视模式，在这里你可以选择角色。
*   `V`进入可视(线条)模式，在该模式下可以选择线条。
*   让你脱离任何模式。
*   不在模式下时，`:w`将文件保存到磁盘。
*   不在模式中时，`:q`退出当前文件。

可以像这样链接最后两个命令:`:wq`，所以保存并退出。

如果你想学习如何用好 Vi，我推荐这个:[http://vim-adventures.com](http://vim-adventures.com)