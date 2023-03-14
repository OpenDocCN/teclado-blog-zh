# Python 中 Else 的更多用途

> 原文：<https://blog.teclado.com/python-more-uses-for-else/>

Python 的 else 语句非常通用，不仅可以用于 if 条件语句。还可以将 else 与 for 和 while 循环一起使用，并作为 try / except 块的一部分。

对于 for 循环，如果主循环没有被`break`语句或异常终止，else 块就会执行。对于 while 循环，当循环条件在新的迭代开始时评估为`False`时，else 块运行。如果遇到了`break`语句，或者发生了异常，循环条件不会被再次检查，所以这两种情况都会阻止 else 子句运行，就像 for 循环一样。

在 try / except 的情况下，如果在 try 块中没有遇到异常，else 块就会运行。

```py
# Print whether or a not an integer is prime for all integers from 2 to 100
for dividend in range(2,101):
    divisor = 2

    while dividend >= divisor ** 2:
        result = dividend / divisor
        if result.is_integer():
            print(f"{dividend} is not prime")
            # Break the while loop when a number is proven non-prime
			break
        divisor += 1

	# The else block runs if no break was encountered in the while loop
    else:
        print(f"{dividend} is prime")

"""
Convert the user input to an integer value and check the user's age is
over 18 if the conversion is successful. Otherwise, print an error message.
"""
try:
    age = int(input("Enter your age: "))
except:
    print ("Please only enter numerical characters.")

# The else block only runs if no exception gets raised
else:
    if age < 18:
        print("Sorry, you're too young to watch this movie.")
    else:
        print("Enjoy the movie!") 
```

你可以在[官方文档](https://docs.python.org/3/tutorial/controlflow.html?highlight=try%20else#break-and-continue-statements-and-else-clauses-on-loops)中阅读更多关于使用循环 else 语句的信息。

## 包扎

我希望你喜欢这个帖子，并且你学到了新的东西！

如果你真的想进一步提升你的 Python 技能，你可能想试试我们的[完整 Python 课程](https://www.udemy.com/the-complete-python-course/?couponCode=BLOGGER)，或者加入我们的 [Discord 服务器](https://discord.gg/BBWwyMq)！您也应该考虑注册我们下面的邮件列表，因为您将获得我们课程的常规折扣代码，确保您总能获得优惠。