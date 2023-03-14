# 使用日期时间在 Python 中处理日期和时间

> 原文：<https://blog.teclado.com/how-to-work-with-dates-times-python-datetime/>

`datetime`模块是 Python 中的一个内置模块，允许我们轻松处理日期和时间。在这个模块中我们可以找到几个类，但是最常用的是`datetime`和`timedelta`。

这通常意味着我们在`datetime`模块中使用`datetime`类——有些令人困惑，但这就是为什么当你查看使用它的 Python 代码时，你通常会看到像`datetime.datetime`这样的东西。

## 什么是`datetime`对象？

简单地说，`datetime`对象是存储特定时间点信息的对象。年、月、日、小时、分钟和秒等信息。有了这些信息，我们就可以用一个`datetime`对象来指代某个特定的时刻。例如:

```py
import datetime

today = datetime.datetime(year=2019, month=12, day=23, hour=11, minute=49, second=30)
print(today)  # 2019-12-23 11:49:30 
```

除了存储特定时间点的数据，它还有一些方法可以帮助我们以一种有意义的方式与数据交互或处理数据。

例如，给定两个`datetime`物体，你可以比较它们，看看哪一个更接近未来。

```py
import datetime

today = datetime.datetime(year=2019, month=12, day=23, hour=11, minute=49, second=30)
tomorrow = datetime.datetime(year=2019, month=12, day=24, hour=11, minute=49, second=30)
print(today > tomorrow)  # False 
```

这里我们将打印出`False`，因为`today`相对于`tomorrow`来说是过去的事了。这就是我们如何比较两个日期，例如，判断过去是否发生了什么事情。

## 如何获得今天的日期

因为获取今天的日期非常常见，所以`datetime`类附带了一个方法，您可以使用该方法获取今天的日期的新的`datetime`对象:

```py
import datetime

print(datetime.datetime.now())
# 2019-12-23 11:54:13.151509 
```

您也可以使用`datetime.datetime.today()`来获得当前的时间和日期，但有时可能不够精确。此外，它现在允许我们给它时区信息(稍后会有更多！)

请注意，当我们这样做时，我们得到的是微秒以及其他时间度量。这可能是不必要的，但在处理计算机时，它有时会很有用。

## 如何修改日期

你不能把两个日期加在一起，因为那很少有意义。比如把“今天”加到“今天”上应该会怎么样？

```py
import datetime

print(datetime.datetime.now() + datetime.datetime.now())
# Error 
```

相反，当你想改变一个日期时——例如增加几天——我们使用`timedelta`类。数学中的一个“delta”就是“变化”的意思，所以这个名字就是这么来的。

```py
import datetime

today = datetime.datetime.now()
one_week = datetime.timedelta(days=7)

print(today + one_week) # 2019-12-30 11:58:52.073407 
```

您可以将`timedelta`与这些参数一起使用:

*   `days`
*   `seconds`
*   `microseconds`
*   `milliseconds`
*   `minutes`
*   `hours`
*   `weeks`

但是对象本身只会存储`days`、`seconds`和`microseconds`。所有其他参数都将被转换成这些参数(例如，`minutes * 60`将被添加到`seconds`)。

## 如何显示日期

您可以使用内置的`str()`函数将日期`print`转换成字符串，这样它们就会以这种格式显示:

```py
import datetime

print(datetime.datetime.now())
# 2019-12-23 11:54:13.151509 
```

有时，您可能希望在如何打印方面有更大的灵活性。也许您只想打印出日期部分。或者也许，只是“小时和分钟”。

你可以通过使用`.strftime()`来做到这一点，它代表“**str**ing**f**format**time**”。

```py
import datetime

today = datetime.datetime.now()

print(today.strftime("%Y-%m-%d")
# 2019-12-23

print(today.strftime("%H:%M")
# 11:54 
```

这样做根本不会修改`today`对象，它只是创建一个表示日期或时间的字符串，如所传递的“格式字符串”所示。

这是一个很好的参考，所有不同的事情你都可以传给`strftime`:[https://strftime.org/](https://strftime.org/)([https://strftime.org/](https://strftime.org/))。

## 如何解析日期

与打印特定格式的日期非常相似，我们可以读入特定格式的日期。

例如，假设您的用户给了您一个描述今天日期的字符串:`23-12-2019`。很明显，这个字符串的格式是“日-月-年”。以 Python `datetime`格式:`"%d-%m-%Y"`。

我们可以使用`.strptime`将日期字符串解析成`datetime`对象:

```py
import datetime

user_date = input("Enter today's date: ")
# Assume they entered 23-12-2019

today = datetime.datetime.strptime(user_date, "%d-%m-%Y")
print(today)  # 2019-12-23 00:00:00 
```

与我们在`strftime`中使用的相同的格式字符串可以在`strptime`中使用

## 什么是时间戳？

时间戳是自 1970 年 1 月 1 日午夜以来经过的秒数。

我们使用时间戳是因为处理单个数字(尽管很大)比处理多个数字更容易，每个数字描述一个不同的度量。

“1970 年 1 月 2 日，午夜”的时间戳将是`86400`:一天中的秒数。

要获得时间戳，您可以调用任何`datetime`对象的`timestamp()`方法:

```py
import datetime

today = datetime.datetime.now()
print(today.timestamp())  # 1577104357.558527 
```

因为现在获取*的时间戳*非常常见，所以我们可以使用`time`模块更容易地获取它:**

```py
import time

print(time.time())  # 1577104492.7515678 
```

## Timezones

时区是使用相同标准时间的区域。

例如，许多西欧国家使用相同的标准时间——西班牙、法国、意大利、德国等。它们都处于“中欧时间”时区。在夏季，当夏令时生效时，它们都处于“中欧夏令时”时区。

如果你在一台使用 CET 的计算机上运行 Python，那么做`datetime.datetime.now()`会给你计算机的本地时间。

然而，如果你拿到一台使用太平洋标准时间的不同电脑，那么`datetime.datetime.now()`会给你一个不同的时间。

这就是为什么告诉`datetime`对象它们所代表的时间是在哪个时区是很重要的:这样无论什么计算机运行代码，所代表的时间总是相同的。

“中部”时间称为协调世界时(UTC)。所有其他时区都可以用 UTC 作为参考来描述。例如，CET 是 UTC + 1 小时。PST 是 UTC - 6 小时。

因此，我们将 CET 写成`+01:00`，PST 写成`-06:00`。

我们称之为“偏移量”，通常放在日期和时间之后。例如:

```py
2019-12-23 11:54:13+01:00 
```

以上时间为 11:54，偏移 1 小时。

这意味着该时区可能是 CET(或任何其他与 UTC 相差+1 小时的时区)。

我们可以通过从时间中减去 1 小时来计算 UTC 时间，这将使我们处于 10:54。

## 获取 UTC 中的当前日期和时间

我们了解到，您可以像这样获取当地时间:

```py
import datetime

today = datetime.datetime.now()
print(today) # 2019-12-23 15:35:56.133138 
```

这样做的时候，你可以让 Python 获取当前时间，但是要转换成不同的时区。下面我们将`datetime.timezone.utc`传递给`.now()`,这样 Python 就会给出 UTC 时区的当前时间。

```py
import datetime

today = datetime.datetime.now(datetime.timezone.utc)
print(today) # 2019-12-23 15:35:56.133138+00:00 
```

你可以看到在我的例子中，时间是一样的。这是因为此时我的时区也有+00:00 的偏移量，所以它与 UTC 一致。

如果您的时区与 UTC 不匹配，您将看到这两个时间是不同的(实际上，这种差异就是您的时区与 UTC 的偏移量)。

注意，当我们在不传递时区的情况下执行`.now()`时，我们打印的字符串不包含偏移量。但是当我们做`.now(datetime.timezone.utc)`时，打印的字符串包含了`+00:00`偏移量。

### 天真与清醒的约会时间

如果你的`datetime`对象知道它所代表的日期和时间在哪个时区，那么我们称之为**感知** `datetime`对象。如果它没有时区信息，我们称之为**幼稚**对象。

感知物体代表世界上特定地方的特定时间点。幼稚对象只代表特定的时间点。

注意，使用简单对象可能是危险的，因为不同的代码段可能会将简单对象解释为使用不同的时区。例如，您的一些代码可能使用一个简单的对象*，就好像它是 UTC 中的*。其他一些代码可能会使用一个简单的对象*，就好像它是当前本地时区的*。

因此，将时区信息存储在您的`datetime`对象中几乎总是一个好主意。

## 使用 pytz 从一个时区转换到另一个时区

您可以使用内置的`datetime`模块从一个时区转换到另一个时区，但是您需要通过为每个时区编写一个子类`datetime.tzinfo`类的类来告诉 Python 存在哪些时区以及可以使用哪些时区。您还必须做大量的手工工作，以便您的时区类跟踪夏令时。

或者，您可以在 Python 代码中使用(或许应该使用)用于处理时区的`pytz`模块。

首先，按照[的安装说明](http://pytz.sourceforge.net/#installation)或使用`pip install pytz`安装`pytz`。

### 从 pytz 获取时区

`pytz`模块带有一个完整的时区数据库，可以通过它们的[官方名称](https://timezonedb.com/time-zones)访问:

```py
import pytz

eastern = pytz.timezone("US/Eastern")
amsterdam = pytz.timezone("Europe/Amsterdam") 
```

### 将时区信息添加到本机日期时间中

如果您有一个简单的`datetime`对象(来自内置模块)，您可以使用`pytz`来添加时区信息:

```py
import datetime
import pytz

eastern = pytz.timezone("US/Eastern")

local_time = datetime.datetime.now()
print(local_time)  # 2020-02-12 11:47:21.817708
eastern_time = eastern.localize(local_time)
print(eastern_time)  # 2020-02-12 11:47:21.817708-05:00 
```

注意，这样做不会改变`datetime`对象中的时间信息。

### 从一个时区转换到另一个时区

如果你有一个 aware `datetime`对象，你想把它转换到另一个时区，那么你也可以用`pytz`来做这件事:

```py
import datetime
import pytz

eastern = pytz.timezone("US/Eastern")
amsterdam = pytz.timezone("Europe/Amsterdam")

local_time = datetime.datetime.now()
eastern_time = eastern.localize(local_time)
print(eastern_time)  # 2020-02-12 11:47:21.817708-05:00

amsterdam_time = eastern_time.astimezone(amsterdam)
print(amsterdam_time)  # 2020-02-12 17:47:21.817708+01:00 
```

现在显示的时间已经改变，因为它是相同的日期和时间，但在不同的时区。`eastern_time`和`amsterdam_time`指的是同一个确切的时间点。

您可以通过自己将每一项转换为 UTC 来确保这一点。在`eastern_time`上加 5 小时，在`amsterdam_time`上减 1 小时，得到+00:00 的偏移量。你会发现时间是一样的。

### 应对夏令时

`pytz`模块与您一起处理夏令时。例如，`US/Eastern`时区有时可能在`-05:00`而其他时间在`-04:00`，这取决于一年中的时间。

下面是一个例子

## 在软件应用程序中处理日期和时间

通常我会建议在您的应用程序中始终使用 UTC。向用户询问当地时间和时区，并将其转换为 UTC。将 UTC 存储在数据库中。仅当您向用户显示日期和时间时，才变回用户的本地时间(并且仅当您的用户希望以他们的本地时间查看时间时)。

这样，您就不必担心应用程序逻辑中的时区问题；只有在处理用户交互的时候。

`pytz`模块带有一个`pytz.utc`时区，你可以在构造`datetime`对象时使用它来简化这个过程:

```py
import datetime
import pytz

eastern = pytz.timezone("US/Eastern")

user_date = datetime.datetime(2020, 4, 15, 6, 0, 0, tzinfo=eastern)
utc_date = user_date.astimezone(pytz.utc)
print(utc_date)  # 2020-04-15 10:56:00+00:00 
```

只要您始终使用 UTC，并且在显示信息时只转换到用户的本地时区，这就相对简单了！

## 包扎

感谢您阅读这篇关于使用`datetime`和`pytz`在 Python 中处理日期和时间的博文。希望对你有用！

如果你想从整体上了解更多关于 Python 的知识，请查看我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)。这是一个 30 小时的视频课程，带你从 Python 初学者到专家。我们很想在那里见到你！