# 带条件的 Python 理解

> 原文：<https://blog.teclado.com/python-list-comprehensions-conditionals/>

在我们的[上一篇文章](https://blog.teclado.com/python-list-comprehensions/)中，我们介绍了列表理解，但是还有更多的内容要介绍。我们还可以在我们的列表理解中包含条件子句，以过滤掉我们不想包含在新列表中的项目。

在列表理解中包含一个条件从句是非常简单的。我们只需在循环定义后使用 if 关键字:

```
names = ["Matthew", "John", "Helen", "Stephen", "Alexandra", "Rolf"]
short_names = [name for name in names if len(name) < 6]

# ['John', 'Helen', 'Rolf'] 
```

这里我们过滤掉所有超过 5 个字符的名字。

人们常犯的一个错误是把一个条件放在列表理解的开始。这是合法的语法，但完全是另外一种意思。

```
names = ["Matthew", "John", "Helen", "Stephen", "Alexandra", "Rolf"]
short_names = [len(name) < 6 for name in names]

# [False, True, True, False, False, True] 
```

这里，列表理解为`names`中的每个`name`添加一个布尔值到新列表中。这在适当的情况下非常有用，但通常不是您想要的。

与 for 子句一样，列表理解允许多个 if 子句按顺序出现:

```
names = ["Matthew", "John", "Helen", "Stephen", "Alexandra", "Rolf"]
short_final_n = [name for name in names if len(name) < 6 if name[-1] == "n"]

# ['John', 'Helen'] 
```

这里的名字只有在长度都少于 6 个字符并且以`n`结尾时才会出现在我们的新列表中。我不太喜欢在理解中像这样将条件链接在一起，但肯定有这样的情况，它将是完全可读和容易理解的。

## 包扎

如果你有兴趣学习更多关于 list comprehensions 的知识，一定要查看官方文档，或者尝试我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)！

下面我们也有一个表格来注册我们的邮件列表。我们定期为我们的订户发布折扣代码，因此这是确保您在我们的课程中始终获得最优惠的方式。