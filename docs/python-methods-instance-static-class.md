# Python 方法:实例、静态和类

> 原文：<https://blog.teclado.com/python-methods-instance-static-class/>

在这篇博文中，我们将解释存在哪些类型的方法，它们是如何工作的，以及它们彼此之间(以及与函数之间)有什么不同！).

在这篇文章的结尾，你应该对 Python 方法和面向对象编程有了很好的理解。

要了解什么是方法，可以把它们看作是一个类中的普通函数，只是稍微做了一点改动。

当我们调用一个方法时，Python 会在幕后做一些工作，这就是为什么它们的行为与普通函数不同。

有 3 种类型的方法:实例方法、类方法和静态方法。

# 什么是实例方法？

**实例方法是最常见的，也是默认类型的方法。**

它们看起来像普通的函数，存在于一个类中，需要一个特殊的参数，通常称为`self`。

当我们调用一个方法时，我们自己不会为第一个参数传入一个值。Python 会处理这些，并将**调用者对象**作为第一个参数传递给方法调用。这允许我们在方法中访问对象实例的不同属性。

什么是**调用者对象**？让我们看一个例子:

```py
class Student:
    def __init__(self, name, grades):
        self.name = name
        self.grades = grades

    def average(self):
        return sum(self.grades) / len(self.grades)

college_student = Student('Rolf', [24, 55, 98, 100])

print(college_student.average())  # 69.25 
```

这里我们创建了一个`Student`类，带有**两个实例方法** : `__init__`和`average`。

我们像普通函数一样调用对象实例上的方法，使用圆括号:`average()`，但是在对象和方法名之间有一个句点。这表明该方法在对象的内部*。*

让我们分别看一下每种方法:

```py
def __init__(self, name, grades):
    self.name = name
    self.grades = grades 
```

当我们创建一个新对象时，`__init__`方法被调用。它有`self`参数以及任何其他数量的参数。当创建一个对象时，我们为每个参数*传入值，除了`self`* ，就像这样:

```py
college_student = Student('Rolf', [24, 55, 98, 100]) 
```

我们类中的另一个方法叫做`average`:

```py
def average(self):
    return sum(self.grades) / len(self.grades) 
```

这个方法只有`self`参数。当我们对自己创建的对象调用该方法时，Python 会自动给这个参数赋值。

让我们创建一个对象并在其上调用方法:

```py
college_student = Student('Rolf', [24, 55, 98, 100])

print(college_student.average())  # 69.25 
```

**下面是这段代码中函数调用的流程:**

1.  首先我们用`Student('Rolf', [24, 55, 98, 100])`创建一个对象。这调用了我们类的`__init__`方法，并将两个参数作为值传递给`name`和`grades`。
    *   `self`的值由 Python 提供。它是一个空的物体。
    *   然后我们在对象内部设置两个属性，`self.name`和`self.grades`。我们也可以设置任何其他我们想要的属性！
2.  这将返回我们创建的对象，并将其赋给`college_student`变量。
3.  当我们调用`college_student.average()`时，Python 自动传递`college_student`作为`self`的值，因此在方法内部，我们可以访问我们之前在`__init__`内部设置的属性。

让我们再创建一个对象，看看会发生什么。

```py
working_student = Student('Anna', [88, 79, 90, 99, 100])

print(working_student.average())  # 91.2 
```

我们现在可以看到 instance 方法从每个单独的对象实例中访问属性。

下面是一个示例代码，说明 Python 在后台做了什么，以帮助您更好地理解这一理论:

```py
print(Student.average(working_student))  # Caller object 
```

## 什么时候应该使用实例方法？

*   当谈到 Python 面向对象编程时，实例方法是您的首选。
*   当方法要访问调用者对象的属性时，使用实例方法。

# 什么是类方法？

顾名思义，类方法可以通过类来访问。它们接收调用者的类作为参数，根本不需要创建对象。

类方法仍然需要一个特殊的参数，但是它通常被称为`cls`(class 的简称)，而不是`self`。为了定义一个类方法，我们使用了`@classmethod`装饰器。

这里有一个简短的例子:

```py
class Bank:
    @classmethod
    def make_loan(cls, amount):
        print(f"You applied for a loan of ${amount}.")

Bank.make_loan(1500)  # You applied for a loan of $1500. 
```

## 什么时候使用类方法？

*   当您需要访问类的状态而不仅仅是对象实例时，请使用类方法。
*   当您想要对类的所有实例进行更改时，它们会非常有用。
*   类方法的另一个好处是，如果您处于继承场景中，它们会自动使用子类。

这里我们可以看到一个继承场景的例子，我们使用了类方法`from_sum` **:**

```py
class FixedFloat:
    def __init__(self, amount):
        self.amount = amount

    def __repr__(self):
        return f'<FixedFloat {self.amount:.2f}>'

    @classmethod
    def from_sum(cls, value1, value2):
        return cls(value1 + value2)

class Euro(FixedFloat):
    def __init__(self, amount):
        super().__init__(amount)
        self.symbol = '€'

    def __repr__(self):
        return f'<Euro {self.symbol}{self.amount:.2f}>'

print(FixedFloat.from_sum(16.7565, 90))  # <FixedFloat 106.76>
print(Euro.from_sum(16.7565, 90))  # <Euro €106.75> 
```

我们已经定义了扩展了`FixedFloat`类的`Euro`类。它有一个调用父节点的`__init__`的`__init__`方法和一个覆盖父节点的`__repr__`方法。它没有继承的`from_sum`方法，我们将只使用父类定义的方法。

现在你可能想知道:为什么不像平常一样创建对象，用`FixedFloat(value1 + value2)`代替`cls(value1 + value2)`？

让我们看看如果用`FixedFloat`硬编码方法会发生什么。密切注意上课的方法:

```py
class FixedFloat:
    def __init__(self, amount):
        self.amount = amount

    def __repr__(self):
        return f'<FixedFloat {self.amount:.2f}>'

    @classmethod
    def from_sum(cls, value1, value2):
        return FixedFloat(value1 + value2)

class Euro(FixedFloat):
    def __init__(self, amount):
        super().__init__(amount)
        self.symbol = '€'

    def __repr__(self):
        return f'<Euro {self.symbol}{self.amount:.2f}>' 
```

我们稍微修改了一下代码，现在`from_sum`方法调用的是`FixedFloat(value1 + value2)`而不是`cls(value1 + value2)`。

然而，看看当我们试图使用来自`Euro`类的方法时会发生什么:

```py
print(FixedFloat.from_sum(16.7565, 90))  # <FixedFloat 106.76>
print(Euro.from_sum(16.7565, 90))  # <FixedFloat €106.75 
```

由于`Euro.from_sum`正在使用超类实现，它返回一个`FixedFloat`！这很可能不是我们想要的，这也是使用`cls`参数代替`@classmethod`的主要原因。

Python 会自动给我们调用者类，而不是我们硬编码在方法本身中的那个。这意味着代码更加灵活，我们不必为了返回正确的对象类型而在子类中重新实现同样的东西。

# 什么是静态方法？

静态方法是存在于类中的常规函数，用来表示与类的某种关系。

默认情况下，它们不需要特殊的参数，也不接收对象实例或类实例作为参数。相反，他们需要的是`@staticmethod`装饰师。

我们称它们为自包含方法，因为它们不像其他类型的方法那样可以访问实例或类。

## 什么时候应该使用静态方法？

当我们不需要访问类或实例时，应该使用静态方法。例如，当打印数据或执行算术运算时。

在这里，我们使用静态方法为用户生成一个由 ***k*** 字符组成的随机字符串:

```py
import random

class User:
    def __init__(self, username, password):
        self.username = username
        self.password = password

    @staticmethod
    def generate_password():
        return ''.join(random.choices('abc1234567890', k=8))

print(User.generate_password()) 
```

我们可以看到，通过使用`@staticmethod`装饰器将`generate_password`作为静态方法放置在`User`类中，该方法不再需要特殊的参数，或者实际上根本不需要任何必需的参数。

使用静态方法相对于普通函数的一个优点是，将它们组合在一个类中更有意义，因为静态方法表明它们只能在与该类相关的情况下使用。

# 摘要

*   **实例方法**接收 ***调用者对象*** 作为第一个参数，它不需要装饰器。
*   **类方法**接收 ***调用者类*** 作为第一个参数，它需要`@classmethod`装饰器。
*   **静态方法**没有任何必要的参数，它需要`@staticmethod`装饰器。

# 结论

我们走吧！我们已经学习了每一种 Python 方法，现在你可以区分和使用它们了！我们希望你喜欢这篇博文。不要忘记订阅我们的时事通讯，以便在我们发布新的博客帖子时得到通知！

**趣闻:**你知道静态方法和类方法的区别是求职面试中被问得最多的问题之一吗？

让我们知道你的想法！今天就加入我们的不和谐社区！
[https://discord.gg/rC2wKa6dXb](https://discord.gg/rC2wKa6dXb)