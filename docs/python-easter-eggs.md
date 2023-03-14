# 蟒蛇复活节彩蛋

> 原文：<https://blog.teclado.com/python-easter-eggs/>

在博客上做了所有这些[编码面试问题](https://blog.teclado.com/tag/coding-interview-problems/)之后，我想是时候我们放松一下，看一些不那么严肃的东西了。Python 毕竟是以一个喜剧团体 Monty Python 命名的，而不是一个蛇类家族，所以幽默和有趣应该是这门语言的核心。

Python 实际上有许多巧妙的隐藏特性。我只打算在这里展示一些，但是我鼓励你自己去寻找更多！

## `import this`

首先，我们有`import this`，它将 Python 的禅打印到控制台上:这是一个总结 Python 设计哲学的格言列表。

如果你查看`this.py`的源代码，你会发现一片混乱，就像这样:

```py
s = """Gur Mra bs Clguba, ol Gvz Crgref
Ornhgvshy vf orggre guna htyl.
Rkcyvpvg vf orggre guna vzcyvpvg.
Fvzcyr vf orggre guna pbzcyrk.
Pbzcyrk vf orggre guna pbzcyvpngrq.

... snip ... 
```

这里你可以自己看[。](https://github.com/python/cpython/blob/master/Lib/this.py)

内容实际上是使用旋转密码加密的。为什么？大概是为了不破坏惊喜吧！自己试试`import this`看看 Python 的真正禅意。

## `import __hello__`

有没有想过让 Python 不用自己编写问候应用就能问候你？`import __hello__`就是你的答案。

`import __hello__`非常简单:它只是将`Hello world!`打印到控制台。如果你在使用 Python 2，你会得到一个稍微不那么热情的`Hello world...`——可能是到了使用 Python 3 的时候了。

你也可以使用`import __phello__`来做同样的事情。

## `import antigravity`

这是我最喜欢的蟒蛇复活节彩蛋之一，所以我不会破坏这个惊喜。只需在您的终端中运行这一行并享受。

## 包扎

如果你发现任何其他的 Python 复活节彩蛋，如果你能在 [Twitter](https://twitter.com/TecladoCode) 上与我们分享，我们会很高兴。

如果你渴望继续改进你的 Python，请在周四回来查看另一篇完整的文章。您可能也会对我们最新更新的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)感兴趣，其中有关于测试、Selenium 和 Tkinter 的新章节！

在下面，我们也有一个表格来注册我们的邮件列表。我们定期向我们的订户发送折扣代码，因此这是确保您在我们的课程中始终获得最优惠的方式！