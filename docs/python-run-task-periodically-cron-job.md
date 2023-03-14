# 如何在 Render.com 定期运行任务

> 原文：<https://blog.teclado.com/python-run-task-periodically-cron-job/>

在我们上一期的时事通讯中，我们谈到了使用 rq 的[任务队列，虽然可以将周期性任务添加到队列中，但是有一种更简单的方法！](https://blog.teclado.com/exploring-python-task-queues-celery-and-rq/)

定期运行任务的推荐方式是使用 **cron 作业**。大多数虚拟主机提供商，如 Heroku，不允许你很容易地运行 cron 作业。如果你拥有服务器，你可以运行 cron 作业，就像使用 DigitalOcean 一样，但这也带来了一系列挑战。