# 第 12 天项目:阅读清单|泰克拉多

> 原文：<https://blog.teclado.com/python-30-day-12-project/>

欢迎来到[Python](https://blog.teclado.com/30-days-of-python)30 天[第 12](/30-days-of-python/python-30-day-12-functions) 天项目！我们将创建一个程序，可以存储和显示用户阅读列表中的书籍。

下面是完整的项目简介以及我们解决方案的演示。请记住，您的解决方案不必与我们的完全一致！

## 案情摘要

对于这个项目，应用程序需要具有以下功能:

*   用户应该能够通过提供书名、作者姓名和出版年份将一本书添加到他们的阅读列表中。
*   该程序应该将所有这些书籍的信息存储在一个 Python 列表中。
*   用户应该能够显示他们阅读列表中的所有书籍，并且这些书籍应该以用户友好的格式打印出来。
*   用户应该能够从文本菜单中选择这些选项，并且他们应该能够在不重启程序的情况下执行多个操作。你可以在 while loops(第 8 天)的文章中看到一个工作菜单的例子。

这个项目比我们以前处理过的项目要大一点，所以要确保一次处理一个项目。

这个项目需要注意的是，因为阅读列表中的书籍存储在 Python 列表中，所以当程序结束时，阅读列表数据将会丢失。

几天后，在第 14 天，我们将回到这个项目并扩展它，确保我们在程序结束时不会丢失数据！

祝你好运，希望你玩得开心！

## 我们的演练

我们的完整演练如下，但如果你更喜欢视频版本，看看这里:

项目演练[

参考书目

15:50

](https://www . YouTube . com/watch？v=muWDANDvjA8)

我们开发这个程序的第一步是创建一个地方来存放我们的阅读清单。目前，这将是一个 Python 列表:

然后，我们可能还想定义用户将与之交互的菜单。尽早这样做也给了我们一些继续编码的结构。通过写出菜单，我们可以清楚地知道用户需要从哪些选项中选择:

```
`reading_list = []
menu_prompt = """Please enter one of the following options:

- 'a' to add a book
- 'l' to list the books
- 'q' to quit

What would you like to do? """` 
```

现在我们有了这个，我们可以开始写菜单了。我们将为此使用 while 循环和 if 语句:

```
`reading_list = []
menu_prompt = """Please enter one of the following options:

- 'a' to add a book
- 'l' to list the books
- 'q' to quit

What would you like to do? """

selected_option = input(menu_prompt).strip().lower()

while selected_option != "q":
    if selected_option == "a":
        print("Adding...")
    elif selected_option == "l":
        print("Displaying...")
    else:
        print(f"Sorry, '{selected_option}' isn't a valid option.")

    # Allow the user to change their selection at the end of each iteration
    selected_option = input(menu_prompt).strip().lower()` 
```

请注意，我们已经编写了循环和 if 语句，但是目前除了打印用户选择的内容之外，它们没有做什么。

在这一点上，我们应该运行我们的程序，以确保一切按预期工作！

请注意，如果您愿意，您可以*像这样编写 while 循环:*

```
`while True:
    selected_option = input(menu_prompt).strip().lower()

    if selected_option == "q":
        break
    elif selected_option == "a":
        print("Adding...")
    elif selected_option == "l":
        print("Displaying...")
    else:
        print(f"Sorry, '{selected_option}' isn't a valid option.")` 
```

这样我们就减少了`selected_option`变量的重复。

不过这完全取决于你，两者都完全有效。选择对你来说最自然的方式吧！

在制作这些菜单时，我们经常想要为 if 语句的每个分支创建一个函数。

这是因为每个分支做自己的事情，创建一个函数来处理这种情况可以使我们的程序更容易阅读。

我们现在要做的是:

```
`reading_list = []
menu_prompt = """Please enter one of the following options:

- 'a' to add a book
- 'l' to list the books
- 'q' to quit

What would you like to do? """

selected_option = input(menu_prompt).strip().lower()

def add_book():
    print("Adding...")

def show_books():
    print("Displaying...")

while selected_option != "q":
    if selected_option == "a":
        add_book()
    elif selected_option == "l":
        show_books()
    else:
        print(f"Sorry, '{selected_option}' isn't a valid option.")

    # Allow the user to change their selection at the end of each iteration
    selected_option = input(menu_prompt).strip().lower()` 
```

这种方法的好处是，现在我们可以在每个功能内部工作，知道它们将独立和分开操作。

另外，它使我们的循环比把所有代码都放在里面更短！

要添加新书，我们将向用户询问书籍的详细信息。然后我们会把它们放进字典，并把它们添加到阅读列表中:

```
`def add_book():
    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    new_book = {
        "title": title,
        "author": author,
        "year": year
    }

    reading_list.append(new_book)` 
```

当该函数在循环中运行时，它会要求用户提供每本书所需的三条信息。然后它创建一个字典，并将它存储在`reading_list`中。

请注意，我们可以创建字典并将其追加到同一行中。这一点有时会更清楚:

```
`def add_book():
    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    reading_list.append({
        "title": title,
        "author": author,
        "year": year
    })` 
```

尝试现在运行项目并添加一本新书！

应该都可以用了，书会加进去的。

然而，我们还没有一种方法来显示阅读列表中的书籍。让我们继续下一个:

```
`def show_books():
    print("Displaying...")` 
```

这里我们要遍历`reading_list`变量，打印每本书的信息。

我们可以只使用 for 循环，并使用 f 字符串打印字典中的属性:

```
`def show_books():
    for book in reading_list:
        print(f"{book['title']}, by {book['author']} ({book['year']})")` 
```

或者，我们可以使用解包和字典的`.values()`方法来使代码更加清晰:

```
`def show_books():
    for book in reading_list:
        title, author, year = book.values()
        print(f"{title}, by {author} ({year})")` 
```

我认为对于像这段代码这样的东西，创建单独的变量可能有点过头了。我只是想让你知道你可以做到这一点！如果它能让你更清楚或更易读，那就去做吧！

目前，我们的代码如下所示:

```
`reading_list = []
menu_prompt = """Please enter one of the following options:

- 'a' to add a book
- 'l' to list the books
- 'q' to quit

What would you like to do? """

selected_option = input(menu_prompt).strip().lower()

def add_book():
    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    reading_list.append({
        "title": title,
        "author": author,
        "year": year
    })

def show_books():
    for book in reading_list:
        title, author, year = book.values()
        print(f"{title}, by {author} ({year})")

while selected_option != "q":
    if selected_option == "a":
        add_book()
    elif selected_option == "l":
        show_books()
    else:
        print(f"Sorry, '{selected_option}' isn't a valid option.")

    selected_option = input(menu_prompt).strip().lower()` 
```

我们可以对此再做一个改进。

如果我们调用`show_books()`，但是`reading_list`是空的，那么这个函数什么也不做。该循环根本不会运行。

因此，如果`reading_list`为空，让我们打印一条漂亮的消息！

我们会在循环中进行。`show_books()`函数应该与显示书籍有关，而不是显示书籍*或*如果没有书籍的消息。

让你的函数只做一件事是简化代码的好方法！

在循环中，我们将添加一个 if 语句来检查阅读列表是否为空:

```
`...
    elif selected_option == "l":
        if reading_list:
            show_books()
        else:
            print("Your reading list is empty.")
...` 
```

记住，执行`if reading_list`会将`reading_list`传递给`bool()`函数。如果`reading_list`为空，则计算结果为`False`。否则，评估为`True`。

这样，我们就完成了这个项目！

我们还可以添加更多改进:

1.  我们可以使用实参、参数和返回值(明天将介绍)，这样我们的函数就不会直接与全局变量`reading_list`交互。不与全局变量交互的函数通常更容易思考，并使代码更少纠缠在一起。
2.  我们可以允许用户通过例如作者来搜索书籍。那么我们只会展示那个作者的书。
3.  我们可以在每次修改时将`reading_list`的内容永久保存到一个文件中，这样当程序结束时用户的阅读列表不会丢失。

我们将在第 14 天做出这些改变，敬请关注！在那之前，如果你喜欢挑战，可以试着自己实现它！

感谢您的阅读，明天见！