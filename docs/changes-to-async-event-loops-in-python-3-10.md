# Python 3.10 中异步事件循环的更改

> 原文：<https://blog.teclado.com/changes-to-async-event-loops-in-python-3-10/>

在 Python 3.10 中，对`asyncio`库的一些改变导致了我们整个 Python 课程中的一些问题。

我们使用了`asyncio.get_event_loop()`，并使用它的返回值来执行一些异步任务。现在，调用`get_event_loop()`已经过时了，你应该使用`asyncio.run()`。

以下是本课程展示的异步抓取几个网站的代码:

```
import aiohttp
import asyncio
import time

async def fetch_page(url):
    page_start = time.time()
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            print(f"Page took {time.time() - page_start}")
            return response.status

loop = asyncio.get_event_loop()
tasks = [fetch_page("http://books.toscrape.com") for i in range(10)]
start = time.time()
loop.run_until_complete(asyncio.gather(*tasks))

print(f"All took {time.time() - start}") 
```

但是现在，Python 发出警告，因为`asyncio.get_event_loop()`被弃用了。

Python 官方文档的更新向我们展示了一种更好的新方法:

```
import aiohttp
import asyncio
import time

async def fetch_page(url):
    page_start = time.time()
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            print(f'Page took {time.time() - page_start}')

async def main():
    tasks = [fetch_page('http://books.toscrape.com') for i in range(10)]
    await asyncio.gather(*tasks)

start_time = time.time()
asyncio.run(main())
print(f'All took {time.time() - start_time}') 
```

有两个主要变化:

1.  定义一个“主”协程，它是异步代码的入口点。我们在这里做`async def main()`。
2.  不叫`get_event_loop`和`run_until_complete`，只叫`asyncio.run(main())`。

这要简单得多，因为开发人员不必担心得到一个事件循环，或者哪个事件循环正在运行。只要打电话给`.run()`就能帮你做所有的事情。

如果你正在跟随我们的[完整的 Python 课程](https://go.tecla.do/complete-python-sale-30-days)并且你遇到了这个问题，我希望这篇文章能有所帮助！如果您想了解更多关于 Python 的知识，并且您没有注册我们的课程，请考虑尝试一下！

由[大卫·拉托雷·罗梅罗](https://unsplash.com/photos/0tNF_mHm_Ls)发布的照片