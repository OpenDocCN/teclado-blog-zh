# 第 18 天项目:JSON 阅读清单| Teclado

> 原文：<https://blog.teclado.com/python-30-day-18-project/>

欢迎来到 Python 系列 [30 天的第 18 天项目！今天我们将最后一次修改我们的阅读列表应用程序，来练习一个非常重要的模块`json`。](https://blog.teclado.com/30-days-of-python/)

`json`模块用于对 JSON 数据进行序列化和解序列化。这只是一种奇特的说法，它提供了一种将我们的 Python 代码转换成 JSON 格式，以及从 JSON 转换回 Python 的方法。

在我们开始简介之前，让我们稍微谈一下 JSON 格式是什么样子，以及我们将在`json`模块中使用的重要函数。

## JSON

JSON 代表**J**ava**S**script**O**object**N**rotation，和 CSV 一样，它是一种非常常见的数据存储格式。它在网络上被广泛使用，特别是在网站或应用程序和服务器之间共享信息。

JSON 格式实际上对我们来说非常熟悉，因为它几乎与 Python 字典和列表的格式相同。

下面是一个 JSON 对象的例子，它大致类似于一个字典:

```py
`{
    "title": "1Q84",
    "author": "Haruki Murakami",
    "year": 2009,
    "read": true
}` 
```

对于 JSON，有一些事情我们必须小心记住。首先是所有的字符串，包括键名，必须使用双引号(`"`)。单引号(`'`)在 JSON 中无效。

我们要注意的第二件事是关键字`true`。这在 JSON 中完全是小写的，不像在 Python 中我们使用`True`。在 JSON 中`false`也是一个有效的布尔值，和 Python 中的`False`一样。

如果我们希望有几个独立的 JSON 对象，我们的项目就是这种情况，我们需要将对象包装在一个 JSON 数组中，这类似于一个 Python 列表。

```py
`[
    {
        "title": "1Q84",
        "author": "Haruki Murakami",
        "year": 2009,
        "read": true
    },
    {
        "title": "The Picture of Dorian Gray",
        "author": "Oscar Wilde",
        "year": 1890,
        "read": false 
    }
]` 
```

为了使 JSON 有效，我们必须总是有一个顶级 JSON 对象，或者一个顶级 JSON 数组。这意味着空文件是**而不是**有效的 JSON。

我们可以尽可能深地嵌套这些结构，所以拥有一个包含填充了其他对象的对象数组的对象数组是完全合法的。这种非常复杂的格式的 JSON 并不少见。

虽然这有点困难，但我们可以编写代码来遍历这些 JSON 数据，并将其转换为 Python，但谢天谢地，我们自己不必这么做。这是一个非常常见的操作，因此我们在标准库中有一个`json`模块专门用于这个目的。

## `json`模块

在`json`模块中，我们需要了解两个重要的功能。

第一个函数是`json.load`，它将允许我们从文件中获取 JSON 数据，并将其转换为标准的 Python 类型，如字典、列表、整数、字符串、*等*。

这个函数要求文件是有效的 JSON 格式，所以我们必须小心不要违反我之前提到的任何规则。这包括不要试图读取空文件！

我们将这样使用它:

```py
`with open("books.json", "r") as reading_list:
    books = json.load(reading_list)` 
```

我们需要了解的第二个函数是`json.dump`。这个函数需要一个字典或列表，以及一个要写入的文件。

当使用`dump`时，我们将以写模式(`"w"`)写入文件，这将截断文件。然后，我们将向包含图书馆中所有书籍的文件中写入一个新的 JSON 数组。

我们之所以要采用这种方法，是因为我们的文件中必须有一个顶级数组或对象来满足 JSON 规范，所以我们不能简单地向现有文件内容添加新对象。

别担心。虽然这看起来有些浪费，但是对于非常大量的数据来说，这将是一个近乎即时的操作。当性能成为一个问题时，我们可能已经转移到像数据库这样的存储上了。

下面是一个我们将如何使用`dump`函数的例子:

```py
`with open("books.json", "w") as reading_list:
    json.dump(books, reading_list)  # books is the list of books we want to write` 
```

### 注意

还有一个版本的`dump`和`load`叫做`dumps`和`loads`，但是它们做的事情不一样。

函数名中的“s”指的是字符串，因为这些函数要么接受 JSON 作为字符串，要么给我们一个我们已经转换成 JSON 的代码的字符串表示。你可以在`json`模块[文档](https://docs.python.org/3/library/json.html)中找到大量的例子。

## 案情摘要

这个项目的起点将是我们在第 14 天写的代码。如果您还没有尝试过第 14 天项目的[更难版本](/30-days-of-python/python-30-day-14-project-hard/)，我建议您在继续这里之前尝试一下，因为您将最终得到一个具有更令人印象深刻的功能的应用程序。

这个项目的任务相对简单。我们需要更新阅读列表应用程序的实现，以便我们的数据存储在一个名为`books.json`的新文件中。我们也将从这个新文件中检索我们所有的图书数据。

如果你正在为这个项目创建一个新的 repl 或 workspace，没有必要跨`books.csv`复制，因为我们将不再使用它。

由于空文件不是有效的 JSON，我建议创建带有空方括号的`books.json`文件。这是一个空的 JSON 数组，完全有效，我们可以用 JSON 对象填充这个 JSON 数组。每个对象代表一本不同的书，你可以把这个文件结构想象成一个字典列表。

在项目演练期间，我将向您展示一种以编程方式创建文件的方法，这样我们就不需要预先有一个`books.json`文件。明天我们将学习我们需要的工具，所以这不是你必须自己照顾自己的事情。

正如我在上面的 JSON 介绍中提到的，每当我们做出更改时，我们都必须完全替换`books.json`文件的内容，因为简单地添加新的 JSON 对象会很快导致文件不符合 JSON 规范。与其考虑这一点，不如简单地替换整个文件。

祝你好运！

## 我们的解决方案

我们的解决方案都在下面，或者你可以观看演练的[视频版本](https://youtu.be/83EMCbvJeJA)。

我将从项目较难版本的代码开始，看起来像这样:

```py
`def add_book():
    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    with open("books.csv", "a") as reading_list:
        reading_list.write(f"{title},{author},{year},Not Read\\\\n")

def delete_book(books, book_to_delete):
    books.remove(book_to_delete)

def find_books():
    reading_list = get_all_books()
    matching_books = []

    search_term = input("Please enter a book title: ").strip().lower()

    for book in reading_list:
        if search_term in book["title"].lower():
            matching_books.append(book)

    return matching_books

# Helper function for retrieving data from the csv file
def get_all_books():
    books = []

    with open("books.csv", "r") as reading_list:
        for book in reading_list:
            # Extracts the values from the CSV data
            title, author, year, read_status = book.strip().split(",")

            # Creates a dictionary from the csv data and adds it to the books list
            books.append({
                "title": title,
                "author": author,
                "year": year,
                "read": read_status
            })

    return books

def mark_book_as_read(books, book_to_update):
    index = books.index(book_to_update)
    books[index]['read'] = "Read"

def update_reading_list(operation):
    books = get_all_books()
    matching_books = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.csv", "w") as reading_list:
            for book in books:
                reading_list.write(f"{book['title']},{book['author']},{book['year']},{book['read']}\\\\n")
    else:
        print("Sorry, we didn't find any books matching that title.")

def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print(f"{book['title']}, by {book['author']} ({book['year']}) - {book['read']}")

    print()

menu_prompt = """Please enter one of the following options:

- 'a' to add a book
- 'd' to delete a book
- 'l' to list the books
- 'r' to mark a book as read
- 's' to search for a book
- 'q' to quit

What would you like to do? """

# Get a selection from the user
selected_option = input(menu_prompt).strip().lower()

# Run the loop until the user selected 'q'
while selected_option != "q":
    if selected_option == "a":
        add_book()
    elif selected_option == "d":
        update_reading_list(delete_book)
    elif selected_option == "l":
        # Retrieves the whole reading list for printing
        reading_list = get_all_books()

        # Check that reading_list contains at least one book
        if reading_list:
            show_books(reading_list)
        else:
            print("Your reading list is empty.")
    elif selected_option == "r":
        update_reading_list(mark_book_as_read)
    elif selected_option == "s":
        matching_books = find_books()

        # Checks that the seach returned at least one book
        if matching_books:
            show_books(matching_books)
        else:
            print("Sorry, we didn't find any books for that search term")
    else:
        print(f"Sorry, '{selected_option}' isn't a valid option.")

    # Allow the user to change their selection at the end of each iteration
    selected_option = input(menu_prompt).strip().lower()` 
```

这里有相当多的代码，但是不要着急浏览。如果你有任何理解上的困难，我建议你看看从第 14 天开始的项目演练。

这个程序的大部分将会完全相同。我们当前实施中需要更改的功能有:

*   `add_book`
*   `get_all_books`
*   `update_reading_list`

我们的其他函数都不处理这个文件，所以它们的实现将保持不变。我们的菜单代码也是如此，它与我们的存储方式无关。它简单地向用户呈现一个界面，并将功能委托给我们的函数。

让我们从在文件顶部导入`json`模块开始，因为我们将需要在几个地方使用它。

随着`json`的导入，我们可以开始修改`get_all_books`。这是一个很好的起点，因为我们在几个不同的地方依赖这个函数，并且我们在新的`add_book`实现中也需要它。

```py
`def get_all_books():
    books = []

    with open("books.csv", "r") as reading_list:
        for book in reading_list:
            # Extracts the values from the CSV data
            title, author, year, read_status = book.strip().split(",")

            # Creates a dictionary from the csv data and adds it to the books list
            books.append({
                "title": title,
                "author": author,
                "year": year,
                "read": read_status
            })

    return books` 
```

此刻，我们阅读我们的旧文件；我们处理文件的每一行；构建字典；并将其添加到`books`列表中。然后，我们返回这个字典列表，供应用程序中的其他地方使用。

我们的新实现实际上要简单得多，因为我们不再需要自己处理任何数据。`json.load`函数将能够看到我们有一个 JSON 数组，可能填充了 JSON 对象，它将返回给我们一个对应于该数据的字典列表。

```py
`def get_all_books():
    with open("books.json", "r") as reading_list:
        books = json.load(reading_list)

    return books` 
```

这展示了使用其他模块的强大功能。解析 JSON 并不是一项非常简单的任务，这里我们只用一行代码就完成了。

作为参考，还有一个`csv`模块可以让我们在处理 CSV 数据时做一些非常类似的事情。我们有一个简短的视频介绍如何使用这个[在这里](https://www.youtube.com/watch?v=W7QByFjVom8)。

我们还可以使我们的`get_all_books`函数更短，因为我们并不真正需要这个`books`变量。我们可以从上下文管理器中返回字典列表。

```py
`def get_all_books():
    with open("books.json", "r") as reading_list:
        return json.load(reading_list)` 
```

现在让我们转向`add_book`。这个函数将会看到一些相当重要的变化，因为我们不能再在文件中多写一行了。我们需要首先获得当前文件的内容，添加一个额外的记录，然后将新的内容写回到文件中。

幸运的是，我们已经有了一个获取文件内容的函数:最新更新的`get_all_books`函数。姑且称之为`add_book`的顶部吧。然后我们可以直接把原始的提示放在后面，因为我们仍然需要它们。

```py
`def add_book():
    books = get_all_books()

    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()` 
```

我们的下一步是构建一个字典，我们可以将它插入到我们的`books`列表中。我们可以这样创建它，作为调用`append`方法的一部分:

```py
`def add_book():
    books = get_all_books()

    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    books.append({
        "title": title,
        "author": author,
        "year": year,
        "read": "Not read"
    })` 
```

现在我们在`books`列表中有了新记录，我们可以将整个`books`列表写入 JSON 文件。通过以写模式打开文件，我们可以截断原始文件内容，并用新数据完全替换它。

为了写入数据，我们将使用`json.dump`。

```py
`def add_book():
    books = get_all_books()

    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    books.append({
        "title": title,
        "author": author,
        "year": year,
        "read": "Not read"
    })

    with open("books.json", "w") as reading_list:
        json.dump(books, reading_list)` 
```

现在我们可以进行最后的修改了:函数`update_reading_list`。

```py
`def update_reading_list(operation):
    books = get_all_books()
    matching_books = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.csv", "w") as reading_list:
            for book in books:
                reading_list.write(f"{book['title']},{book['author']},{book['year']},{book['read']}\\\\n")
    else:
        print("Sorry, we didn't find any books matching that title.")` 
```

在这里，我们实际上能够进行另一种简化，因为我们不再需要手动格式化我们写入文件的内容。我们只要用`dump`把`books`列表翻译成 JSON 就够了。

```py
`def update_reading_list(operation):
    books = get_all_books()
    matching_books = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.json", "w") as reading_list:
            json.dump(books, reading_list)
    else:
        print("Sorry, we didn't find any books matching that title.")` 
```

至此，我们已经完成了所有需要的更改，但是我们还可以做一些其他的事情。

首先，我们可以使用新的`**`解包语法来缩短我们的`show_books`函数。目前我们有这个:

```py
`def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print(f"{book['title']}, by {book['author']} ({book['year']}) - {book['read']}")

    print()` 
```

为这里的每一个值都编写一个订阅表达式有点冗长，实际上我们有几个备选方案。

首先，我们可以将`book`分解成几个变量，并像这样使用它们:

```py
`def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        title, author, year, read = book.values()
        print(f"{title}, by {author} ({year}) - {read}")

    print()` 
```

这很好，但是我们可以通过使用带有`format`的命名占位符来避免预先定义所有这些变量。如果你需要复习，我们在[第三天](/30-days-of-python/python-30-day-3-string-formatting/)已经讨论过了。

请记住，使用`**`解包字典会给我们一系列关键字参数，这正是我们使用`format`给命名占位符赋值的方式。因此，我们可以这样做:

```py
`def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print("{title}, by {author} ({year}) - {read}".format(**book))

    print()` 
```

升级代码的另一个方法是进行某种检查，以确保文件存在。这有点超前于明天的内容，但是应该很容易理解。

我们将创建一个这样的函数:

```py
`def create_book_file():
    try:
        with open("books.json", "x") as reading_list:
            json.dump([], reading_list)
    except FileExistsError:
        pass` 
```

这里我们说 Python 应该尝试使用`"x"`模式创建一个名为`books.json`的文件，这意味着独占创建。如果这个文件已经存在，`open`将引发一个名为`FileExistsError`的特殊异常。使用一个`except`子句，我们告诉 Python 监听这个错误，如果它在运行上面的代码时遇到这个错误，它应该忽略它。

现在我们只需在开始程序时调用`create_book_file`。

就这样，我们结束了！完整的代码可以在下面找到:

```py
`import json

def add_book():
    books = get_all_books()

    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    books.append({
        "title": title,
        "author": author,
        "year": year,
        "read": "Not read"
    })

    with open("books.json", "w") as reading_list:
        json.dump(books, reading_list)

def create_book_file():
    try:
        with open("books.json", "x") as reading_list:
            json.dump([], reading_list)
    except FileExistsError:
        pass

def delete_book(books, book_to_delete):
    books.remove(book_to_delete)

def find_books():
    reading_list = get_all_books()
    matching_books = []

    search_term = input("Please enter a book title: ").strip().lower()

    for book in reading_list:
        if search_term in book["title"].lower():
            matching_books.append(book)

    return matching_books

# Helper function for retrieving data from the csv file
def get_all_books():
    with open("books.json", "r") as reading_list:
        return json.load(reading_list)

def mark_book_as_read(books, book_to_update):
    index = books.index(book_to_update)
    books[index]['read'] = "Read"

def update_reading_list(operation):
    books = get_all_books()
    matching_books = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.json", "w") as reading_list:
            json.dump(books, reading_list)
    else:
        print("Sorry, we didn't find any books matching that title.")

def show_books(books):
    # Adds an empty line before the output
    print()

    for book in books:
        print("{title}, by {author} ({year}) - {read}".format(**book))

    print()

create_book_file()

menu_prompt = """Please enter one of the following options:

- 'a' to add a book
- 'd' to delete a book
- 'l' to list the books
- 'r' to mark a book as read
- 's' to search for a book
- 'q' to quit

What would you like to do? """

# Get a selection from the user
selected_option = input(menu_prompt).strip().lower()

# Run the loop until the user selected 'q'
while selected_option != "q":
    if selected_option == "a":
        add_book()
    elif selected_option == "d":
        update_reading_list(delete_book)
    elif selected_option == "l":
        # Retrieves the whole reading list for printing
        reading_list = get_all_books()

        # Check that reading_list contains at least one book
        if reading_list:
            show_books(reading_list)
        else:
            print("Your reading list is empty.")
    elif selected_option == "r":
        update_reading_list(mark_book_as_read)
    elif selected_option == "s":
        matching_books = find_books()

        # Checks that the seach returned at least one book
        if matching_books:
            show_books(matching_books)
        else:
            print("Sorry, we didn't find any books for that search term")
    else:
        print(f"Sorry, '{selected_option}' isn't a valid option.")

    # Allow the user to change their selection at the end of each iteration
    selected_option = input(menu_prompt).strip().lower()` 
```

和往常一样，我鼓励你继续做这个项目，看看你能如何改进它。这可能意味着添加更多的功能，使代码更简单，或者使现有的功能更健壮。

看看你能发现什么。编码快乐！