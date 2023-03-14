# 编码面试问题:旋转列表

> 原文：<https://blog.teclado.com/coding-interview-problems-rotating-a-list/>

继续我们关于[编码面试问题](https://blog.teclado.com/tag/coding-interview-problems/)的系列，本周我们将处理旋转列表。这个问题很有趣，因为有许多变量需要考虑。列表应该向哪个方向旋转？作为这种轮换的一部分，物品应该移动多少个位置？在这篇文章中，我们将涵盖你需要知道的一切。

## 解决问题

首先，当我们谈论旋转列表时，我们到底指的是什么？

假设我们有一个这样的列表:

```py
base = [1, 2, 3, 4, 5] 
```

如果我们将列表向右旋转一个位置，我们会将每个值向右移动一个位置，列表末尾的项目会绕到前面:

```py
right_rotation = [5, 1, 2, 3, 4] 
```

向左旋转一个位置将得到以下结果:

```py
left_rotation = [2, 3, 4, 5, 1] 
```

旋转列表时，列表的长度不会改变，所有值都会保留。这就是我们最终采用这种换行行为的原因，因为移动列表项会在列表中造成漏洞，并且项也会超出该列表的有效索引范围。

让我们从这个问题最简单的版本开始，我们像上面的例子一样执行一个单一的位置旋转。我们的初始方向是向右旋转(顺时针)。

要考虑的一件事是，除了最右边的值，我们所有的值都是正确的顺序。这意味着我们不需要为列表中的每一项手动执行某些操作，因此循环可能是不必要的。

相反，对于单个位置顺时针旋转，我们可以获取最右边列表元素的值，存储它，然后从原始列表中删除它。这让我们有 90%的机会找到可行的解决方案。

然后，我们可以将原始的最终值和列表的剩余部分连接到一个新的列表中:这次是按照旋转的顺序。

让我们看看一些代码。

## 一次顺时针旋转

我们可以从获取基表的最终值开始。现在，我们可以从头开始使用我们的示例列表:

```py
base = [1, 2, 3, 4, 5]
final_value = base[-1]  # 5 
```

如果您对`-1`的意思有点困惑，可以将负索引与序列结合使用，它们的工作原理相反。所以`-1`表示最后一项，`-2`表示倒数第二项，以此类推。

当从原始列表中删除项目时，我们有两个主要选项。如果我们能保证所有项目都是唯一的，我们可以使用`remove`方法:

```py
base = [1, 2, 3, 4, 5]
final_value = base[-1]    # 5
base.remove(final_value)  # [1, 2, 3, 4] 
```

然而，在大多数情况下，您会想要使用`del`关键字，因为`del`依赖于索引，而不是值，因此在这种情况下不容易出错。

```py
base = [1, 2, 3, 4, 5]
final_value = base[-1]  # 5
del base[-1]            # [1, 2, 3, 4] 
```

现在，看到这里的一些人可能已经注意到有一个更短的方法:方法`pop`。

如果您不熟悉`pop`，这个方法既从列表中移除一个项目，又返回那个值。它还可以使用索引，所以对于包含重复元素的列表是安全的，默认情况下，它会删除最后一项。非常适合我们的用例。

```py
base = [1, 2, 3, 4, 5]
final_value = base.pop() 
```

### 创建新列表

既然我们已经将最终值从列表中分离出来，我们需要将这些项重新组合在一起。同样，我们有几种选择。

首先，我们可以使用`+`操作符进行简单的连接。在这种情况下，我们必须将`final_value`转换为单个条目列表，因为 Python 不允许我们连接字符串和整数。

```py
base = [1, 2, 3, 4, 5]
final_value = base.pop()
rotated_list = [final_value] + base  # [5, 1, 2, 3, 4] 
```

另一种选择是`insert`方法。`insert`让我们在我们喜欢的任何索引处插入一个条目，因此我们可以将`final_value`放在基本列表中的位置`0`。关于这个方法需要注意的一点是，我们并没有得到一个新的列表:我们只是修改了`base`列表。这可能适合您的用例，也可能不适合。

```py
base = [1, 2, 3, 4, 5]
final_value = base.pop()
base.insert(0, final_value)  # [5, 1, 2, 3, 4] 
```

我们可以使用`extend`方法采取几乎相反的方法。这允许我们通过传入 iterable 将一系列值追加到一个列表中。它本质上是`append`方法的多值版本。

同样，我们需要将`final_value`转换成一个列表才能工作。

```py
base = [1, 2, 3, 4, 5]
final_value = base.pop()
rotated_list = [final_value]
rotated_list.extend(base) 
```

关于这个方法需要注意的一点是，我们不执行赋值，所以我们不能在使用`extend`方法的同一行上将`final_value`转换成列表。这是因为`[final_value]`和`final_value`不是同一个对象，我们无法指代前者。

我们也不能通过赋值来解决这个问题，因为`extend`的返回值是`None`。

## 逆时针旋转一周

现在我们知道了如何执行单次顺时针旋转，我们需要改变什么来执行逆时针旋转？

事实上，不是很多。我们仍然可以使用`pop`，我们创建新列表的一些方法只要稍加调整就完全可以工作。

关于`pop`，我们现在需要提供一个索引作为参数，因为我们不再需要`pop`原始列表的最终值:我们需要的是第一个值。

```py
base = [1, 2, 3, 4, 5]
first_value = base.pop(0)  # 1 
```

使用串联，我们现在只需颠倒值的顺序，如下所示:

```py
base = [1, 2, 3, 4, 5]
first_value = base.pop(0)
rotated_list = base + [first_value]  # [2, 3, 4, 5, 1] 
```

我们也可以使用`insert`方法，但是对于这种情况来说这也有点大材小用，因为我们只想把`first_value`放在最后。因此，我们不妨使用`append`:

```py
base = [1, 2, 3, 4, 5]
first_value = base.pop(0)
base.append(first_value)  # [2, 3, 4, 5, 1] 
```

## 越来越花哨

上面的解决方案都很好，但是我们也有一些更好的方法来实现同样的事情。这并不一定意味着它们更好，但至少了解它们很有趣。

### `pop`一句话

关于上面的解决方案，我们实际上可以用一行代码来完成。因为`pop`返回一个值，所以我们可以将`pop`方法直接放在方括号内，在那里我们放置了`final_value`或`first_value`。这完全消除了对分配行的需要。

顺时针:

```py
base = [1, 2, 3, 4, 5]
rotated_list = [base.pop()] + base  # [5, 1, 2, 3, 4] 
```

逆时针方向:

```py
base = [1, 2, 3, 4, 5]
rotated_list = base + [base.pop(0)]  # [2, 3, 4, 5, 1] 
```

### 部分

这很好，但是如果我们愿意，我们可以比这更好一点。例如，我们可以使用切片。如果你不熟悉切片，我强烈建议你看看我们以前关于这个主题的帖子:

[https://blog.tecladocode.com/python-slices/](https://blog.teclado.com/python-slices/)

[https://blog.tecladocode.com/python-slices-part-2/](https://blog.teclado.com/python-slices-part-2/)

切片最酷的地方在于，它们根本不会影响原始列表。我们也可以在任何序列类型上使用它，而不仅仅是列表！

顺时针:

```py
base = [1, 2, 3, 4, 5]
rotated_list = base[-1:] + base[:-1]  # [5, 1, 2, 3, 4] 
```

逆时针方向:

```py
base = [1, 2, 3, 4, 5]
rotated_list = base[1:] + base[:1]  # [2, 3, 4, 5, 1] 
```

### 拆包

一个完全不同的选择是使用拆包。同样，我们的原始列表保持不变，但是这里的语法可能有点晦涩。

顺时针:

```py
base = [1, 2, 3, 4, 5]
*head, tail = base
rotated_list = [tail, *head]  # [5, 1, 2, 3, 4] 
```

逆时针方向:

```py
base = [1, 2, 3, 4, 5]
head, *tail = base
rotated = [*tail, head]  # [2, 3, 4, 5, 1] 
```

这个方法的工作方式是,`*`操作符将从多重赋值中收集所有未赋值的值。

在第一个例子中，我们有两个变量名，`head`和`tail`，但是`head`以`*`操作符开头。这意味着`tail`将得到一个单一的值(最终值)，而`head`将被赋予列表的剩余部分。

然后我们可以创建一个新的列表，首先放入`tail`，然后放入`head`中的值。为了避免第二个元素是整个列表，我们可以再次使用`*`操作符来解包`head`。如果你喜欢，你也可以在这里使用`extend`方法，就像我们以前做的那样。

在第二个版本中，`*`操作符的位置颠倒了，这意味着`head`得到一个值，而`tail`包含列表的其余部分。

### 德克斯

到目前为止，我们忽略的一个解决方案是使用`deque`集合类型。我们在本周的 Python 片段的[中介绍了 deques，在那里我们展示了它方便的`rotate`方法。](https://blog.teclado.com/python-deques/)

`deque`集合是`collections`模块的一部分，所以您必须导入它才能使用它。一旦我们导入了`deque`类型，执行旋转就像调用`rotate`方法一样简单，并提供一些位置来旋转序列。

顺时针:

```py
from collections import deque

base = deque([1, 2, 3, 4, 5])
base.rotate()  # deque([5, 1, 2, 3, 4] 
```

逆时针方向:

```py
from collections import deque

base = deque([1, 2, 3, 4, 5])
base.rotate(-1)  # deque([2, 3, 4, 5, 1] 
```

## 旋转多个位置

到目前为止，我们一直在执行单个位置的旋转，但是我们可能会被要求顺时针旋转一个列表 3 个位置。

如果我们可以选择使用一个`deque`，这是非常容易的，因为我们可以只提供数字`3`作为参数。然而，我们很可能不得不自己实现一些东西。

我们将假设旋转的次数是完全任意的，并且用户可能会请求超过列表长度的旋转。换句话说，我们将允许多次革命。

让我们首先定义一个函数并设置一些初始值:

```py
base = [1, 2, 3, 4, 5]
rotations = 3

def rotate(base_list, r):
    pass 
```

为了考虑多次旋转，我们可以利用模运算符(`%`)和我们旋转的列表的长度。如果我们用循环数除以列表的长度，余数就是我们实际需要执行的循环数。其他一切都代表着一场彻底的革命，因此无关紧要。

这里我们必须小心，因为模运算会消除`r`的符号，所以我们需要执行额外的检查来保留它。我们稍后会谈到这一点。

还有另一个关于模和负数的问题，我们已经在另一篇文章中[详细介绍过了，所以我们打算使用`abs`函数让`r`为这个操作的正数。](https://blog.teclado.com/pythons-modulo-operator-and-floor-division/)

```py
base = [1, 2, 3, 4, 5]
rotations = 3

def rotate(base_list, r):
    rotations = abs(r) % len(base_list) 
```

从这里开始，我们可以使用 for 循环来重复执行我们之前的解决方案之一。例如，我们可以像这样使用我们的`pop`一行程序解决方案，返回结果。

我们必须记住的一点是`base_list`和全局列表`base`实际上是同一个列表。如果我们从`base_list`向`pop`发送一个项目，`base`也会因此缺少一个项目。如果我们想要多次运行我们的函数，这是有问题的。

为了解决这个问题，我们可以给一个同名的变量分配一个`base_list`的副本。现在，当我们用`pop`方法修改`base_list`时，我们正在修改一个不同的列表对象。

```py
base = [1, 2, 3, 4, 5]
rotations = 3

def rotate(base_list, r):
    base_list = base_list.copy()
    rotations = abs(r) % len(base_list)

    for _ in range(rotations):
        base_list = [base_list.pop()] + base_list

    return base_list 
```

注意，我们在每次迭代中不使用`range`中的当前值，所以我们可以使用`_`来表示没有使用临时 for 循环变量。`range`这里只是一种设置具体迭代次数的方法。

上面的解决方案对于正旋转值很好，但是我们没有考虑负旋转值，这需要不同的连接顺序。

为了检查旋转的方向，我们可以再次使用`r`，因为`r`没有以任何方式改变，因此保留了旋转的原始符号。如果`r`小于零，我们知道用户输入了一个负的旋转值，因此想要逆时针旋转他们的列表。

我们只需要一个`if`声明来确定正确的方法:

```py
base = [1, 2, 3, 4, 5]
rotations = 3

def rotate(base_list, r):
    base_list = base_list.copy()
    rotations = abs(r) % len(base_list)

    if r < 0:
        for _ in range(rotations):
            base_list = base_list + [base_list.pop(0)]
    else:
        for _ in range(rotations):
            base_list = [base_list.pop()] + base_list

    return base_list

print(rotate(base, rotations))  # [3, 4, 5, 1, 2]
print(rotate(base, -12))        # [3, 4, 5, 1, 2] 
```

如下图所示，结果与使用`deque`类型时相同:

```py
deque_1 = deque([1, 2, 3, 4, 5])
deque_1.rotate(3)  # deque([3, 4, 5, 1, 2])

deque_2 = deque([1, 2, 3, 4, 5])
deque_2.rotate(-12)  # deque([3, 4, 5, 1, 2]) 
```

如果你愿意，你也可以使用我们在这篇文章中讨论的其他方法来编写解决方案，尽管相比之下，我认为上面的版本相对简单易读。

## 包扎

希望您现在已经很好地了解了可以用来解决此类问题的各种方法。对于所有的编码问题，有数不清的解决方案。如果你有一个我们在这里没有提到的，我们很想听听，所以在 [Twitter](https://twitter.com/TecladoCode) 上联系，或者在我们的 [Discord 服务器](https://discord.gg/BBWwyMq)上发帖。

如果你刚刚开始学习 Python，并且想要比这些博客帖子更全面的东西，我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)刚刚在 Udemy 上进行了重大更新，增加了关于测试、Selenium 和 GUI 开发的新章节。我们很希望你能来！

我们也有一个表格来注册我们下面的邮件列表。这是一个很好的方式来保持我们所有内容的更新，我们还为我们的订户张贴我们课程的定期折扣代码。