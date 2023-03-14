# Python 对字符串的划分方法

> 原文：<https://blog.teclado.com/python-partition-method/>

在这篇 [Python 片段文章](https://blog.teclado.com/tag/python-snippets/)中，我们将看看一个不太为人所知的叫做`partition`的字符串方法。这很像`split`方法，但是有一些关键的区别。

## 快速回顾一下

对于那些不熟悉`split`的人来说，这是一个字符串方法，用于根据某种分隔符将字符串分成多个部分。例如，我们可能有这样的东西:

```py
price = 12.50
split_price = price.split(".")  # ['12', '50'] 
```

通常你会把这个列表分解成几个变量，如下所示:

```py
prince = 12.50
dollars, cents = prince.split(".")

print(dollars)  # 12
print(cents)    # 50 
```

这里有几点需要注意。例如，`split`只关心分隔符两边的值。分离器本身就被扔掉了。

第二件要注意的事情是，我们不知道我们将从`split`中得到多少值，至少不知道，除非我们在一些额外的配置中指定。`split` will 每次在字符串中找到分隔符时，都会不断将字符串分割成新的列表项。

## 与`partition`的差异

那么`partition`呢？让我们来看看:

```py
price = 12.50
partition_price = price.partition(".")  # ('12', '.', '50') 
```

我们马上可以看到一个不同之处:`partition`保留了分隔符。这很有趣，我认为当我们想要在一些修改之后重用这个分隔符来重组字符串时，它会非常有用。在利用好名字的同时，我们可以非常动态地使用它。

那么，当我们遇到分隔符的多个实例时怎么办呢？

```py
file_name = "grand_canyon.005.png"
partition_file_name = file_name.partition(".")  # ("grand_canyon", ".", "005.png") 
```

尽管字符串中有两个`.`实例，我们仍然以一个三项元组结束。而且没错，是元组，不是列表。

`partition`总会给我们一个三项元组。第一项将包含直到分隔符的第一个实例的字符串；第二项是分离器本身；第三项是分隔符第一个实例之后的所有内容。

那么如果分隔符不在字符串中呢？

```py
name = "Jose"
partition_name = name.partition(".")  # ('Jose', '', '') 
```

我们再次得到一个三项元组，但是当分隔符没有找到时，第二项和第三项是空字符串。第一项将包含调用该方法的整个字符串。

## `rpartition`变种

除了`partition`之外，还有一个`rpartition`方法，它从字符串的末尾开始扫描。这意味着第一个遇到的分隔符实例是字符串中最后一个出现的。例如，这对于一次性获取文件扩展名、分隔符和文件名可能很有用。

```py
file_name = "grand_canyon.005.png"

name, seperator, extension = file_name.rpartition(".") 
```

## 包扎

这篇文章就写到这里吧！希望你学到了一些新东西，我希望你在自己的项目中找到`partition`的用处！

如果你刚刚开始学习 Python，或者想提升你的技能，我们希望你参加我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)。你可能也想订阅我们下面的邮件列表。这是了解我们所有最新内容的好方法，我们还会定期发布我们课程的优惠券！