# Python 集合运算符

> 原文：<https://blog.teclado.com/python-set-operators/>

集合是一种非常有用的集合类型，除了提供大量比较集合的简便方法之外，还允许非常快速的成员检查。

这些方法包括并集、交集和差集。它们是这样工作的:

```py
set_1 = {1, 2, 3, 4, 5}
set_2 = {3, 4, 5, 6, 7}

# Combine set_1 and set_2
print(set_1.union(set_2))  # {1, 2, 3, 4, 5, 6, 7}

# Find common elements in set_1 and set_2
print(set_1.intersection(set_2))  # {3, 4, 5}

# Find elements in set_1 which are not in set_2
print(set_1.difference(set_2))  # {1, 2} 
```

这些方法很棒，但是其中一些方法的名字很长，而且占用了很多空间。我们可以不使用这种方法语法，而是使用特殊的集合运算符，如下所示:

```py
set_1 = {1, 2, 3, 4, 5}
set_2 = {3, 4, 5, 6, 7}

# Combine set_1 and set_2
print(set_1 | set_2)  # {1, 2, 3, 4, 5, 6, 7}

# Find common elements in set_1 and set_2
print(set_1 & set_2)  # {3, 4, 5}

# Find elements in set_1 which are not in set_2
print(set_1 - set_2)  # {1, 2} 
```

我们还可以将这些操作链接在一起，例如，找到三个集合的并集:

```py
set_1 = {1, 2, 3, 4, 5}
set_2 = {3, 4, 5, 6, 7}
set_3 = {5, 6, 7, 8, 9}

# Combine set_1 and set_2 and set_3
print(set_1 | set_2 | set_3)  # {1, 2, 3, 4, 5, 6, 7, 8, 9} 
```

需要注意的一点是，当使用这些集合操作符而不是方法语法时，两个操作数**都必须是集合。使用方法语法时，参数可以是任何可迭代类型。**

如果你需要一个完整的关于 set 的复习，请查看我们的《Python 30 天》的 [Day 11: Sets](https://teclado.com/30-days-of-python/python-30-day-11-sets/) ，或者你可以尝试我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)，在那里我们会进入更多关于 set 和 set 操作的细节。希望在那里见到你！