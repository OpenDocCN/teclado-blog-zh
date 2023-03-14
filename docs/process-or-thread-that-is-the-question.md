# 如何以及何时在 Python 代码中使用进程和线程

> 原文：<https://blog.teclado.com/process-or-thread-that-is-the-question/>

如果你现在买一台笔记本电脑，它通常是一台四核(或更好的)电脑，有时带有 T2 超线程技术和 T4 涡轮加速技术。然而，本文不是关于这些花哨的术语。对软件工程师来说，重要的是知道如何利用不断增长的计算能力。

> 你买了一辆法拉利，开起来像菲亚特一样。
> 
> ——兹拉坦·伊布拉希莫维奇在巴塞罗那的日子

# 这个问题

如果你学过操作系统相关的课程，你可能知道什么是进程和线程。有大量的教程和文章定义和比较这些概念。

> 简单来说，进程就是一个正在执行的程序。一个或多个线程在进程的上下文中运行。线程是操作系统分配处理器时间的基本单元。一个线程可以执行进程代码的任何部分，包括另一个线程当前正在执行的部分。
> 
> -[https://docs . Microsoft . com/en-us/windows/win32/proc thread/processes-and-threads](https://docs.microsoft.com/en-us/windows/win32/procthread/processes-and-threads)

然而，我个人发现在实践中很难联系起来:我知道我的笔记本电脑有几个内核，我知道程序可能运行在多个进程和/或多个线程上，但是它们如何协同工作，我应该选择哪一个呢？

# 进程与线程

在我们回答这个问题之前，让我们从编程的角度来看一下进程和线程。从天真的角度来看，你的程序**是**一个进程，它将在**的一个**内核上运行。它包含**一个**线程，按照你给的指令。该线程是流程的子集，因为流程包含更多信息(上下文)。

然而，随着 CPU 变得越来越强大，单线程进程将无法充分利用处理器。如果我们可以将工作分配到几个线程中，CPU 将有足够的能力在它们之间切换，并在很短的时间内依次执行每个线程。那么看起来他们是同时在跑。这就是所谓的**并发**。

你可能已经猜到了，既然我们的 CPU 现在是多核的，为什么不让程序在所有的核上运行呢？这就是多处理发挥作用的时候了。一个进程只能在一个内核上运行。这是因为进程比线程重得多，所以 CPU 不能像线程那样在一个内核上无缝切换。因此，我们将它们分配到不同的核心。每个内核都有自己的计算能力，因此 CPU 可以同时执行与内核数量一样多的进程。这就是所谓的**平行度**。

现在我们知道了进程和线程在现实生活中是如何工作的，剩下的问题是:我如何选择使用哪一个？

# 精辟的回答

为了回答这个问题，我将提供两个现实生活中的例子，它们是我遇到的，并分别用多重处理和多线程解决的。希望这些例子能阐明一个概括的答案。

## 多线程批处理 API 调用

一个场景是，我需要向第三方 API 发送一堆 API 调用。更具体地说，我想为我的网站获取 50 个用户的数据。以下是我的要求:

*   我不关心请求的顺序，这对于并行编程来说是完美的(无论是使用并发还是并行)。
*   我希望它尽快完成，否则，我会让我的用户等待。

我选择使用多线程而不是多重处理，因为:

*   我没有 50 核的电脑。
*   每个任务都很轻。
*   我这边真的不需要任何计算。(查询数据由 API 主机完成，我只是请求结果。)

我是这样做的(没有什么比代码更好的解释了):

```py
from concurrent.futures import ThreadPoolExecutor, as_completed

def fetch_user_info(user_id):
    # call third party API here
    pass

with ThreadPoolExecutor(max_workers=20) as executor:
    # submit each job and map future to user id
    future_to_id = {
        executor.submit(fetch_user_info, user_id): user_id
        for user_id in user_ids
    }
    # collect each finished job
    for future in as_completed(future_to_id):
        res = future.result()  # read the future object for result
        user_id = future_to_id[future]  # match result back to user
        # TODO: process the result 
```

原谅我的半伪代码，这里有一些解释。

*   `ThreadPoolExecutor`提供了一个上下文管理器，它让你不用担心线程在应该开始的时候分叉，在结束后关闭，这是一个非常好的特性。
*   顾名思义，`ThreadPoolExecutor`提供了一个线程“池”,您可以通过`max_workers`参数指定最多可以同时运行多少个线程。你可以将超过`max_workers`个工作分派到池中，但是一次只能接受`max_workers`个。请注意，这取决于您想要使用多少资源，而且，对我来说更重要的是，这取决于第三方 API 允许您发送请求的频率(我尝试了 50 次，但被禁止了)。
*   `executor`返回一个 Future 对象，一旦任务完成，它就通过`concurrent.futures.as_completed`函数进行通信。它允许我们以同步的方式与异步代码进行交互。

## 多重处理的文本处理

在这种情况下，我试图通过自然语言处理库在本地执行一些自然语言处理(NLP ):

*   我得到了一个文章列表，NLP 图书馆将从这些文章中分析和提取一些特定的术语。
*   就计算能力而言，NLP 工作非常昂贵。这将占用大量 CPU 资源。

我是这样做的:

```py
from concurrent.futures import ProcessPoolExecutor, as_completed

def run_nlp(filename):
    # execute NLP
    pass

with ProcessPoolExecutor(max_workers=MAX_WORKERS) as executor:
    future_to_file = {
        executor.submit(run_nlp, filename): filename for filename in filenames
    }

    for future in as_completed(future_to_file):
        res = future.result()  # read the future object for result
        filename = future_to_file[future]  # match result back to filename
        # TODO: process the result 
```

代码注释:

*   我没有提供 res 解析代码，但是可以像前面的例子一样进行处理。
*   除了使用不同的“池执行器”之外，它几乎与前面的示例相同。对于这个例子，我只是希望提供一个不同的场景，而不是实现。
*   我通过一个环境变量设置了`max_workers`，我将在下面解释。

### 利用 CPU

我通过环境变量配置`max_workers`的原因是，在这种情况下，并行性的水平不取决于我的用例，而是取决于计算能力。我有三个不同的环境:

*   本地:在我的笔记本电脑上
*   开发:在开发服务器上
*   生产:在生产服务器上

我的笔记本电脑是 4 核的，有睿频加速功能。我最初在本地将`MAX_WORKERS`设置为 4，这是有意义的，因为我可以一次将 4 个繁重的任务分别分配给 4 个内核。我的开发服务器也有 4 个内核，生产服务器有 16 个内核。所以我分别设置了`MAX_WORKERS` 4，4，16。

我发现程序在本地运行比在开发服务器上运行快得多。但是当然，这两种设置都是由生产服务器提供的。通过一点研究，我发现 dev 服务器使用了一个更便宜的 CPU，时钟频率更低。

然后我尝试了其他的东西，这导致了一个有趣的发现。我尝试局部增加`MAX_WORKERS`，结果确实有所改善。改进停止在`MAX_WORKERS` =8，这比`MAX_WORKERS` =4 花费了大约一半的时间。然后，即使增加了更多的工人，这种改善也消失了。

然而，这种模式在 dev 服务器上并不存在。这有点奇怪，不是吗？然后，我突然意识到这一定与涡轮增压有关。它允许每个内核超出其正常速率(在短时间内大约是正常速率的两倍)，看起来好像是内核的两倍。然而，通常使用更便宜的 CPU 的服务器没有这种能力。

因此，我从这个例子中得出的结论是，`MAX_WORKERS`可以设置为计算机中的内核数量，以充分利用其功能。根据任务的重量和内核的能力，您甚至可以将`MAX_WORKERS`设置为内核数量的倍数。最佳配置需要一些基准测试工作。

# 结论

通过这两个例子，我们可以得出这个简单的经验法则:

*   如果您想批量启动轻量级作业，多线程是您的好朋友。
*   如果你的任务是资源密集型的(需要大量的计算)，考虑多重处理。

还有其他限制，比如您的请求是否是上下文无关的。回想一下，我们在本文前面提到过，**进程**包含了**线程**的上下文。换句话说，线程共享相同的上下文。根据具体情况，这可能是有利的，也可能是麻烦，我们可以在另一篇文章中深入探讨这个问题。

最后，让我们花些时间来欣赏 Python 的魅力，它为这些问题提供了优雅的解决方案和模式。