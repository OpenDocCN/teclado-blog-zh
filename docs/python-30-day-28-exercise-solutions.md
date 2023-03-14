# 第 28 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-28-exercise-solutions/>

这是我们在 Python 系列 [30 天中](https://blog.teclado.com/30-days-of-python/)[第 28 天练习](/30-days-of-python/python-30-day-28-type-hinting)的解决方案。在检查解决方案之前，请确保您亲自尝试了该练习！

今天的练习是给我们的 [day 14](/30-days-of-python/python-30-day-14-files) 项目解决方案添加类型注释。我将使用我在项目的[难度版本中的解决方案，你可以在下面找到。](/30-days-of-python/python-30-day-14-project-hard)

```py
`def add_book():
    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    with open("books.csv", "a") as reading_list:
        reading_list.write(f"{title},{author},{year},Not Read\n")

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
                reading_list.write(",".join(book.values()) + "\n")
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

这里有很多代码，所以让我们一次处理一部分，从顶部开始使用`add_book`函数。

```py
`def add_book():
    title = input("Title: ").strip().title()
    author = input("Author: ").strip().title()
    year = input("Year of publication: ").strip()

    with open("books.csv", "a") as reading_list:
        reading_list.write(f"{title},{author},{year},Not Read\n")` 
```

这个函数没有太多要注释的。它不需要任何参数，并且隐式返回，所以我们只需为变量添加一些注释。

在这种情况下，它们都是字符串。

```py
`def add_book():
    title: str = input("Title: ").strip().title()
    author: str = input("Author: ").strip().title()
    year: str = input("Year of publication: ").strip()

    with open("books.csv", "a") as reading_list:
        reading_list.write(f"{title},{author},{year},Not Read\n")` 
```

接下来我们有`delete_book`。

```py
`def delete_book(books, book_to_delete):
    books.remove(book_to_delete)` 
```

这次我们在函数中没有任何变量，但是我们有一些参数需要注释。

如果我们看一下我们的`get_all_books`函数，我们可以看到，每当我们从文件中检索书籍时，它们都存储在一个列表中。列表中的每本书都用字典表示，其中的键和值都是字符串。

因为这将是一个相当复杂的注释，所以我们最好定义一些别名来使我们的注释更容易理解。我们还必须记住导入`typing`模块，因为我们需要访问`List`和`Dict`注释。

### 注意

对于任何具有类似字典结构的东西，有一个更通用的类型，叫做`Mapping`。在这里我们不需要担心，但这是你应该知道的。

```py
`from typing import List, Dict

Book = Dict[str, str]

def add_book():
    title: str = input("Title: ").strip().title()
    author: str = input("Author: ").strip().title()
    year: str = input("Year of publication: ").strip()

    with open("books.csv", "a") as reading_list:
        reading_list.write(f"{title},{author},{year},Not Read\n")

def delete_book(books: List[Book], book_to_delete: Book):
    books.remove(book_to_delete)` 
```

既然已经对`delete_book`进行了适当的注释，我们就可以开始处理`find_books`了。

```py
`def find_books():
    reading_list = get_all_books()
    matching_books = []

    search_term = input("Please enter a book title: ").strip().lower()

    for book in reading_list:
        if search_term in book["title"].lower():
            matching_books.append(book)

    return matching_books` 
```

`find_books`没有参数，但是有返回值。注意，我们仍然可以在这里使用我们的`List[Book]`注释，尽管列表有时可能是空的。空列表在技术上并不违反这个注释，因为它只是一个零`Book`元素的列表。

```py
`def find_books() -> List[Book]:
    reading_list: List[Book] = get_all_books()
    matching_books: List[Book] = []

    search_term: str = input("Please enter a book title: ").strip().lower()

    for book in reading_list:
        if search_term in book["title"].lower():
            matching_books.append(book)

    return matching_books` 
```

接下来是`get_all_books`，我们读取文件和格式化数据的辅助函数。

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

这里的标注和`find_books`很像。这里我们没有任何参数，但是我们有一个需要注释的变量，以及一个返回值。这些应该是完全一样的，因为我们要返回那个变量。

```py
`def get_all_books() -> List[Book]:
    books: List[Book] = []

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

`mark_book_as_read`是我们的下一个函数，它将和`delete_book`几乎完全一样。

```py
`def mark_book_as_read(books: List[Book], book_to_update: Book):
    index: int = books.index(book_to_update)
    books[index]['read'] = "Read"` 
```

现在我们来看一些更复杂的东西。

```py
`def update_reading_list(operation):
    books = get_all_books()
    matching_books = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.csv", "w") as reading_list:
            for book in books:
                reading_list.write(",".join(book.values()) + "\n")
    else:
        print("Sorry, we didn't find any books matching that title.")` 
```

这个函数很复杂，因为它是高阶函数。它需要一个函数作为参数。我们如何注释一个函数？

为此，我们需要使用`Callable`类型，我建议为此创建一个新的注释别名，因为这会变得很长。

在使用`Callable`的时候，我们要为想要传入的函数提供一个签名。例如，下面将说明一个接受两个整数并返回一个浮点数的函数。

```py
`Callable[[int, int], float]` 
```

参数类型在一组方括号中提供，返回值在不带方括号的参数后提供。

在我们的例子中，操作的函数签名如下所示:

```py
`Callable[[List[Book], Book], Any]` 
```

我们需要提供`Any`，因为我们的函数没有返回类型注释。别忘了导入！

我将把这个新注释分配给别名`Operation`。

现在我们可以将`update_reading_list`注释如下:

```py
`def update_reading_list(operation: Operation):
    books: List[Book] = get_all_books()
    matching_books: List[Book] = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.csv", "w") as reading_list:
            for book in books:
                reading_list.write(",".join(book.values()) + "\n")
    else:
        print("Sorry, we didn't find any books matching that title.")` 
```

我们需要注释的最后一个函数是`show_books`，非常简单。它只接受一个参数。

```py
`def show_books(books: List[Book]):
    # Adds an empty line before the output
    print()

    for book in books:
        print(f"{book['title']}, by {book['author']} ({book['year']}) - {book['read']}")

    print()` 
```

我们还可以继续注释菜单中使用的变量，等等。在这种情况下，我们完成的应用程序将如下所示:

```py
`from typing import Any, Callable, Dict, List

Book = Dict[str, str]
Operation = Callable[[List[Book], Book], Any] 

def add_book():
    title: str = input("Title: ").strip().title()
    author: str = input("Author: ").strip().title()
    year: str = input("Year of publication: ").strip()

    with open("books.csv", "a") as reading_list:
        reading_list.write(f"{title},{author},{year},Not Read\n")

def delete_book(books: List[Book], book_to_delete: Book):
    books.remove(book_to_delete)

def find_books() -> List[Book]:
    reading_list: List[Book] = get_all_books()
    matching_books: List[Book] = []

    search_term: str = input("Please enter a book title: ").strip().lower()

    for book in reading_list:
        if search_term in book["title"].lower():
            matching_books.append(book)

    return matching_books

# Helper function for retrieving data from the csv file
def get_all_books() -> List[Book]:
    books: List[Book] = []

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

def mark_book_as_read(books: List[Book], book_to_update: Book):
    index: int = books.index(book_to_update)
    books[index]['read'] = "Read"

def update_reading_list(operation: Operation):
    books: List[Book] = get_all_books()
    matching_books: List[Book] = find_books()

    if matching_books:
        operation(books, matching_books[0])

        with open("books.csv", "w") as reading_list:
            for book in books:
                reading_list.write(",".join(book.values()) + "\n")
    else:
        print("Sorry, we didn't find any books matching that title.")

def show_books(books: List[Book]):
    # Adds an empty line before the output
    print()

    for book in books:
        print(f"{book['title']}, by {book['author']} ({book['year']}) - {book['read']}")

    print()

menu_prompt: str = """Please enter one of the following options:

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
        reading_list: List[Book] = get_all_books()

        # Check that reading_list contains at least one book
        if reading_list:
            show_books(reading_list)
        else:
            print("Your reading list is empty.")
    elif selected_option == "r":
        update_reading_list(mark_book_as_read)
    elif selected_option == "s":
        matching_books: List[Book] = find_books()

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

注意，我省略了对`selected`选项的注释，因为`mypy`会抱怨我们在给定的范围内重新定义变量。

如果你使用了一个`while True`风格的循环，这不是一个你需要担心的问题。