# 第 21 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-21-exercise-solutions/>

下面是我们对[Python](https://blog.teclado.com/30-days-of-python)30 天[第 21 天练习](/30-days-of-python/python-30-day-21-multiple-files)的解决方案！请记住，在通读我们的解决方案之前，自己动手做练习。

## 1)将代码拆分成文件

这个练习既是阅读和理解代码的练习，也是分解代码的练习。

但是真的，这才是重点！当您阅读并理解代码时，您可以开始将代码视为由做不同事情的不同部分组成。

对我们来说，不同的部分是相对清晰的:我们有一组面向用户的功能和一组面向数据的功能。

与用户交互的功能有:

*   `menu()`
*   `prompt_add_book()`
*   `list_books()`
*   `prompt_read_book()`
*   `prompt_delete_book()`

以及与数据存储(本例中是一个 JSON 文件)交互的函数:

*   `create_book_table()`
*   `get_all_books()`
*   `insert_book()`
*   `save_all_books()`
*   `mark_book_as_read()`
*   `delete_book()`

考虑到这一点，我们可以将文件分成两个文件:每个文件对应一段代码。

我们最终会得到类似于`main.py`的包含面向用户代码的东西:

main.py

```py
`import database

USER_CHOICE = """
Enter:
- 'a' to add a new book
- 'l' to list all books
- 'r' to mark a book as read
- 'd' to delete a book
- 'q' to quit

Your choice: """

def menu():
    database.create_book_table()
    user_input = input(USER_CHOICE)
    while user_input != 'q':
        if user_input == 'a':
            prompt_add_book()
        elif user_input == 'l':
            list_books()
        elif user_input == 'r':
            prompt_read_book()
        elif user_input == 'd':
            prompt_delete_book()

        user_input = input(USER_CHOICE)

def prompt_add_book():
    name = input('Enter the new book name: ')
    author = input('Enter the new book author: ')

    database.insert_book(name, author)

def list_books():
    for book in database.get_all_books():
        # book[3] will be a falsy value (0) if not read
        read = 'YES' if book['read'] == '1' else 'NO'
        print(f"{book['name']} by {book['author']} — Read: {read}")

def prompt_read_book():
    name = input('Enter the name of the book you just finished reading: ')

    database.mark_book_as_read(name)

def prompt_delete_book():
    name = input('Enter the name of the book you wish to delete: ')

    database.delete_book(name)

menu()` 
```

和一个包含数据存储代码的`database.py`文件:

database.py

```py
`import json

BOOKS_FILE = 'books.json'

def create_book_table():
    try:
        with open(BOOKS_FILE, 'x') as file:
            json.dump([], file)  # initialize file as empty list
    except FileExistsError:
        pass

def get_all_books():
    with open(books_file, 'r') as file:
        return json.load(file)

def insert_book(name, author):
    books = get_all_books()
    books.append({'name': name, 'author': author, 'read': False})
    save_all_books(books)

def save_all_books(books):
    with open(books_file, 'w') as file:
        json.dump(books, file)

def mark_book_as_read(name):
    books = get_all_books()
    for book in books:
        if book['name'] == name:
            book['read'] = '1'
    save_all_books(books)

def delete_book(name):
    books = get_all_books()
    books = [book for book in books if book['name'] != name]
    save_all_books(books)` 
```

如果您最终得到了不同的代码分割方式，不要担心！其实并没有什么对错之分。然而，我们很乐意在我们的 [Discord 服务器](https://discord.gg/BBWwyMq)上与你讨论这个问题。我们都可以学到新的东西！

感谢您参加我们的练习，记得还要处理今天的项目。今天是相当大的一个！

祝你好运，我们在那里见！

## 额外资源

如果您有兴趣了解关于`list_books`函数中这段语法的更多信息:

```py
`read = 'YES' if book['read'] == '1' else 'NO'` 
```

我们有[个帖子，你可以在我们的博客上查看](https://blog.teclado.com/python-pythons-ternary-operator/)。