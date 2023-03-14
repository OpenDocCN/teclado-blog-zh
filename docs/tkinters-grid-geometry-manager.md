# Tkinter 的网格几何管理器

> 原文：<https://blog.teclado.com/tkinters-grid-geometry-manager/>

你好。本周我们将回归 Tkinter 的 GUI 开发，在本帖中，我们将与`grid`几何图形管理器打交道。

如果你错过了 Tkinter 上的早期帖子，你可以通过下面的链接查看。在这些帖子中，我们介绍了一些使用 Tkinter 的基础信息，还介绍了另一个名为`pack`的几何管理器。

[https://blog . tecladocode . com/tkinters-pack-geometry-manager/](https://blog.teclado.com/tkinters-pack-geometry-manager/)

[https://blog . tecladocode . com/side-values-in-tkinters-pack-geometry-manager/](https://blog.teclado.com/side-values-in-tkinters-pack-geometry-manager/)

## 为什么要关心`grid`？

正如我刚才提到的，我们已经为另一个叫做`pack`的几何图形管理器发布了两个帖子，为什么我们还需要另外一个呢？

这实际上是一个很好的问题，事实证明`grid`是 Tk 中比`pack`更新的一个，它的引入是为了克服`pack`几何图形管理器的一些缺点。二十年来，它一直是 Tk 的主要内容，所以它一定做对了什么！

正如我在关于`pack`的第一篇文章中提到的，当使用`pack`几何图形管理器时，复杂的界面很快变得势不可挡，因为很难记住给定容器中的各种小部件是如何交互的。有许多移动的部分，以及许多要跟踪的配置。

`grid`比`pack`需要更多的手动操作，所以我们必须减少对 Tkinter 自动协商所有相关部件的可接受布局的依赖。使用`grid`，我们定义了一个二维表格结构，并告诉窗口小部件占据表格中的某些单元格。这使得在更复杂的界面中推理小部件的位置变得更加容易。

一个小缺点是，我们通常必须预先指定更多的信息，所以`pack`及其非常简洁的布局定义对于更简单的界面来说仍然很棒。

事不宜迟，让我们开始看看如何使用`grid`来定位我们的小部件。

## 定义网格

使用`grid`的第一步通常是定义实际网格的性质:我们将用来放置小部件的二维表格。

网格的配置是在父容器中完成的，而不是在与`grid`对齐的小部件上完成的。这很有意义，因为我们在这里定义的配置对所有这些小部件都是通用的。所有小部件需要关心的是它们在表中的位置。

定义一个网格并不是绝对必要的，如果我们不提供网格，Tkinter 将从我们提供给子部件的配置中推断出一个表结构。稍后会有更多的介绍。

我们有两种方法可以用来定义网格的布局:`columnconfigure`和`rowconfigure`。

它们都有相同的参数，但是顾名思义，它们控制网格的不同维度。

大多数情况下，您将为`columnconfigure`和`rowconfigure`指定两个配置:您正在配置的列或行，以及这些列或行的权重。

行或列的`weight`决定了一行或一列相对于其他行或列应该占据多少可用空间。例如，`weight`为`2`的列将是`weight`为`1`的列的两倍宽，假设有足够的空间来放置小部件。

在代码中，我们可以像这样定义两列，其中小部件的容器是根容器:

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

root.mainloop() 
```

我们又一次回到了我们可靠的固定大小窗口的设置，我们只是包含了我们通常的 Tkinter boilterplate。如果你对上面的代码不清楚，可以看看我们之前的`pack`帖子，在那里我们讨论了很多这种东西:[https://blog . tecladocode . com/tkinters-pack-geometry-manager/](https://blog.teclado.com/tkinters-pack-geometry-manager/)

除了通常的代码，我们还定义了两个网格列，列`0`和列`1`，其中列`1`的宽度是列`0`的两倍。

如果我们想为多个列指定相同的配置，我们可以传入一组列号，而不仅仅是一个列号:

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure((0, 2, 3), weight=1)
root.columnconfigure(1, weight=2)

root.mainloop() 
```

现在我们有四个已定义的列，其中列`1`的宽度是其他列的两倍。

`rowconfigure`工作方式完全一样。

现在我们已经看到了如何配置容器来建立网格结构，我们需要学习如何在网格中放置项目。

对于这个例子，我们将返回到我们信任的矩形，我们将从使用上面定义的第一个更简单的网格开始。

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")

root.mainloop() 
```

这里我们使用`Label`小部件定义了几个彩色矩形。如果你对这里使用的选项不清楚，看看我们的第一篇`pack`帖子。

现在我们已经定义了矩形，让我们使用`grid`将它们放置在窗口中。正如 pack 示例一样，我们将开始使用没有任何意义的配置的`grid`,这样我们可以看到默认行为。就像之前一样，我们将使用`ipadx`和`ipady`添加少量内部填充，以使`Label`文本更易于阅读。

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(ipadx=10, ipady=10)

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(ipadx=10, ipady=10)

root.mainloop() 
```

正如我们所见，`grid`的默认配置看起来与`pack`非常相似。我们只需调用相关小部件上的`grid`方法，任何配置都会作为参数传入。

这在橱窗里看起来像什么？

![](img/0cebe2883dabc1a4244333fdfd6c1452.png)

我不知道你怎么想，但这不是我所期望的。我们不是为主窗口指定了两列网格吗？

事实证明，`grid`总是将我们的小部件放在新的行中，除非我们另外指定。这有一个很好的理由，那就是实际上有无限多的行和列。即使我们只配置了两列，我们实际上可以把东西放在第 2、5 或 67 列，空列折叠到零宽度。

因此 Tkinter 选择将窗口小部件一个放在另一个上面，因为垂直滚动是相对常见的，并且 60 个窗口小部件并排放置的默认布局通常是不可取的。将小部件放在需要的同一行中通常需要较少的配置，而不是为每个小部件定义一个行值。

那么我们如何指定小部件应该放在哪里呢？我们可以使用`row`和`column`参数。

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, ipadx=10, ipady=10)

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, ipadx=10, ipady=10)

root.mainloop() 
```

这里我只定义了一个列值，将第一个矩形放在列`0`中，将第二个矩形放在列`1`中。我们可能希望 Tkinter 现在将小部件放在同一行，但事实并非如此。相反，它们最终出现在指定的列中，但是在不同的行上。

![](img/98a0fe0b565e48de74683253e0017490.png)

因此，我们必须为小部件指定`row`和`column`值来定义我们想要的布局，在这种情况下:

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10)

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, row=0, ipadx=10, ipady=10)

root.mainloop() 
```

现在，两个矩形位于同一行，占据两个不同的列:

![](img/edc6e890e591d356a02f5d427088e32d.png)

注意，我们实际上从未为此行定义任何配置，所以 Tkinter 使用默认的行配置。这意味着只为小部件分配了容纳小部件所需的垂直空间。

因为这一行使用完全默认的配置，我们也可以指定我们想要的任何`row`值，只要两个小部件共享相同的`row`值，布局就不会发生任何变化。正如我前面提到的，空行和空列分别折叠为 0 高度和宽度。

也就是说，使用逻辑行和列编号绝对是一个好主意。

您可能注意到的一件事是，我们的小部件位于两个不同权重的列的中间，但是没有一个小部件占用我们分配给它的空间。使用`pack`,我们有了`fill`和`expand`属性，允许我们给小部件分配更多的空间，并告诉它们占用这些空间。当使用`grid`时，我们有一个不同的属性叫做`sticky`。

`sticky`接受罗盘方向作为值，这些方向的不同组合会产生不同的结果。你可以把`sticky`看作是对齐和填充选项的组合。

指定一个指南针方向会导致小部件“粘”在指定区域的一边。例如，我们可以用`"W"`将`rectangle_1`设置为贴在其指定区域的左侧，用`"E"`将`rectangle_2`设置为贴在其指定区域的右侧。

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky="W")

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, row=0, ipadx=10, ipady=10, sticky="E")

root.mainloop() 
```

这给了我们这样的东西:

![](img/dcf81114e381a9643997b68993492b83.png)

但是，如果我们指定相反的指南针方向，小部件会粘在两边，这将导致小部件拉伸:

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky="EW")

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, row=0, ipadx=10, ipady=10, sticky="EW")

root.mainloop() 
```

![](img/10710ae94709c4f40b472cdd91146ac8.png)

我们现在可以非常清楚地看到我们的列权重在起作用，`rectangle_2`占用的空间是`rectangle_1`的两倍。

那么，当我们将一个小部件设置为粘在其指定区域的所有边缘时，会发生什么呢？

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky="NSEW")

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, row=0, ipadx=10, ipady=10, sticky="NSEW")

root.mainloop() 
```

这里我们指定了一个`"NSEW"`作为两个小部件的粘滞值，这意味着它会粘在指定区域的所有边上。如果您愿意，也可以使用元组，Tkinter 定义了常数，您可以使用这些常数来代替字符串:

```py
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky=("N", "S", "E", "W"))

rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky=(tk.N, tk.S, tk.E, tk.W))

rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky=tk.NSEW) 
```

它们完全一样，所以使用你喜欢的版本。

但是，代码可能不会产生您预期的结果:

![](img/f6dce92da0f2c5fd5397320dde3a3af4.png)

请记住,`sticky`选项会使小部件粘在指定区域的给定边缘，但是我们前面说过，默认情况下，Tkinter 只分配与内部小部件高度相等的行空间。毕竟，他们不需要更多了。

我们可以为行`0`指定一点配置来纠正这一点，给它一个权重`1`。

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

root.rowconfigure(0, weight=1)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky="NSEW")

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, row=0, ipadx=10, ipady=10, sticky="NSEW")

root.mainloop() 
```

就这样，我们的小部件填满了整个窗口:

![](img/ff6c9c5ae136514f864ee07aef06e39e.png)

请注意，默认情况下，小部件的`sticky`值为`None`，因此它们将位于指定区域的中心。添加一个行配置，但是没有`sticky`值，将会得到这样的结果:

![](img/32f454aea11ce7e95bd3c175c097da63.png)

## 为空行赋予权重

在这篇文章的前面，我注意到空行折叠到零高度，但是这只有当它们的权重为`0`时才成立。

我们可以给空行分配权重来影响其他行中的小部件布局。

例如，我们可以给主内容下面的空行一个权重`1`,以将第一行限制在屏幕的一半:

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

root.rowconfigure((0, 1), weight=1)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, ipadx=10, ipady=10, sticky="NSEW")

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=1, row=0, ipadx=10, ipady=10, sticky="NSEW")

root.mainloop() 
```

![](img/479e0f0ab2469da5b73ec9418d64b799.png)

## 跨越行和列

我们要讨论的最后一件事是跨越多行多列。在定义网格时，我们通常关心窗口中较小部件的布局，但是如果我们需要一个标题栏呢？或者我们需要一个大的选择框，就像音乐播放器一样。

幸运的是，这在 Tkinter 中非常容易，只需要一个简单的配置。

让我们添加第三个矩形，这样我们最终得到一个跨越整个顶行的矩形:

```py
import tkinter as tk

root = tk.Tk()
root.geometry("600x400")

root.columnconfigure(0, weight=1)
root.columnconfigure(1, weight=2)

root.rowconfigure(1, weight=1)

rectangle_1 = tk.Label(root, text="Rectangle 1", bg="green", fg="white")
rectangle_1.grid(column=0, row=0, columnspan=2, ipadx=20, ipady=20, sticky="NSEW")

rectangle_2 = tk.Label(root, text="Rectangle 2", bg="red", fg="white")
rectangle_2.grid(column=0, row=1, ipadx=10, ipady=10, sticky="NSEW")

rectangle_3 = tk.Label(root, text="Rectangle 3", bg="blue", fg="white")
rectangle_3.grid(column=1, row=1, ipadx=10, ipady=10, sticky="NSEW")

root.mainloop() 
```

这里有相当多的变化，所以让我们确保正确理解发生了什么。

首先，我们的`rowconfigure`方法被设置为只影响第二行`1`。因此，第一行将使用默认配置。

`rectangle_1`现在有了一点额外的内部填充，并且有了一个新的关键字参数`columnspan=2`。这就是允许小部件跨越多列的原因，我们马上就会看到。

`rectangle_2`现在位于行`1`上，并且位于第一列。

添加了一个新的`rectangle_3`，它位于第二行和第二列，但在其他方面与`rectangle_2`相同，只是有一些不同的文本和不同的颜色背景。

这就是它看起来的样子:

![](img/f9338aec05dba06eb20464f9a292c039.png)

如果您需要一个小部件跨越多行而不是多列，您可以使用`rowspan`来代替，但是它以完全相同的方式工作。

## 包扎

有了它，你应该能够用 Tkinter 和`grid`几何图形管理器创建一些非常有趣的布局。我真的希望你学到了新的东西。

如果你有兴趣了解更多关于 Tkinter 的知识，我们已经发布了一个深入的课程，在那里我们会教你你需要知道的一切，此外我们还构建了大量很酷的 Tkinter 应用程序。查看使用 Python 和 Tkinter 进行 GUI 开发的[课程！](https://go.tecla.do/tkinter-gui-course-sale)