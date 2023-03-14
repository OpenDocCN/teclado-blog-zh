# Python 中的@property 装饰器

> 原文：<https://blog.teclado.com/property-decorator-in-python/>

Python 的特性很多，学的时候很容易不知所措。今天我们将重点学习其中的一个特性:装饰器。

我们将解释什么是属性，为什么您可能想要使用它们，以及它们在 Python 的类结构中的位置。

当学习编程时，在你学会基础知识之后，通常会有一个陡峭的学习曲线。教程和现实项目差距很大！装饰器是你很少在教程中看到的特性之一，但有时会非常有用。

## 什么是属性？什么是方法？

在讨论编程语言时，像`property`和`method`这样的术语经常被抛来抛去。放慢速度来定义 Python 中每个术语的实际含义是很有用的。

一个**属性**是一个特定于类对象的变量。如果你对什么是“对象”不熟悉，请查看[这篇文章](https://blog.teclado.com/introduction-to-object-oriented-programming-in-python/)进行快速回顾。例如，如果您有一个名为`Human`的类，那么属性将是该类的每个对象上存在的任何变量。您可以使用`self`关键字来创建属性。“属性”只是我们给属于一个类的变量起的名字。这里有一个使用`Human`类的例子:

```
class Human:
    def __init__(self, age):
        self.age = age 
```

`self.age`指的是现在在`Human`类的对象上的属性。我们可以设置许多属性，这取决于我们需要类对象拥有什么。例如，我们可以将`self.hair_color`或`self.eye_color`设置为`Human`类的对象的属性。

一个**方法**是属于一个类的函数。例如，在名为`Human`的类中，方法可以是存在于该类中的任何函数。这里有一个例子，使用我们的`Human`类:

```
class Human:
    def __init__(self, age):
        self.age = age

    def get_age(self):
        return self.age 
```

`get_age`函数被称为“类 Human 上的一个方法”。重要:方法总是接收参数`self`作为它们的第一个参数。这是因为方法总是连接到一个类。在这种情况下，`get_age`就是所谓的“实例方法”，并且将总是接收被调用的对象作为第一个参数。要了解每种方法的更多信息，请查看[这篇文章](https://blog.teclado.com/python-methods-instance-static-class/)。

属性是一种特殊类型的属性。它是一个分配了两个方法的属性:一个“getter”和一个“setter”。让我们看一些代码来进一步解释:

```
class Human:
    def __init__(self, age):
        self.age = age

    def get_age(self):
        return self.age

    def set_age(self, new_age):
        self.age = new_age

    age = property(get_age, set_age) 
```

如你所见，我们使用了很多相同的代码。我们包含了一个名为`set_age`的新“setter”函数，然后我们将`age`变量标识为该类的一个属性，getter 为`get_age`，setter 为`set_age`。这就是`property()`函数正在做的事情:它为第一个参数获取一个 getter，为第二个参数获取一个 setter，并在该类上创建一个**属性**变量，该变量存储在您指定的名称中。

类的属性很有用，因为它们允许我们在获取或设置属性时对属性进行一些动态处理。例如，如果您的类有属性`age`，但是您想确保年龄值总是以月为单位给出，您可以通过将`age`属性设为属性来确保这一点。看看这段代码，明白我的意思:

```
class Human:
    def __init__(self, age):
        self.age = age

    def get_age_in_months(self):
        return self.age * 12

    def set_age(self, new_age):
        self.age = new_age

    age = property(get_age_in_months, set_age) 
```

## 装修工

为了进一步简化创建属性的过程，我们将使用`@property`装饰器。如果你从未用过/见过装修工，跟随我们的[迷你课程](https://blog.teclado.com/decorators-in-python/)快速上手。

`@property`装饰器是使用我们之前使用的`property()`函数的快捷方式。最重要的是，它使代码更加简洁。此外，`@property`装饰器在类初始化时首先执行，所以在它成为属性之前没有机会意外地使用它。让我们深入研究一下这段代码:

```
class Human:
    def __init__(self, age):
        self.age = age

    @property
    def age(self):
        return self._age

    @property 
    def months(self):
        return self._age * 12

    @age.setter
    def age(self, new_age):
        self._age = new_age 
```

这里，我们在 getter 和 setter 函数上使用了 decorators，我们现在已经将它重命名为属性的名称。让我们具体看一下每一个。

getter 方法`age`是应用了`@property`装饰器的方法。我们可以用它来格式化和改变每次属性被引用时 getter 输出的内容。

但是由于`age`是一个函数，我们仍然需要在某个地方存储人的年龄值。这就是`_age`属性发挥作用的地方。

当初始化类`Human`的对象时，`__init__`方法将运行。这将在`self`对象上设置`age`属性。因为我们为`age`属性编写了一个自定义设置器，自定义设置器将会运行。

在自定义设置器中，我们获取传入的`age`值，并在`self`对象上设置`_age`属性。此时，使用 setter，我们可以更好地控制如何设置`_age`属性。

当`__init__`方法结束运行时，对象将有:

*   一个`_age`属性，包含创建对象时传入的值。
*   一项`age`财产:
    *   当被访问时调用`@property`方法
    *   更改时使用`@age.setter`方法
*   一个可以被访问但其值不能被改变的`months`属性(因为它没有设置器)。

为了说明我的意思，考虑下面的代码:

```
jeff = Human(26)

print('I am ')
print(jeff.age)
print(' years old.') 
```

你觉得这会印出什么？

这将输出以下内容:

```
I am 
26
years old. 
```

用`@property`装饰器来装饰 getter 函数允许我们改变属性将如何提供给任何调用它的人。例如，我可以通过创建另一个属性`months`来使用代码返回年龄所对应的月数:

```
@property
def months(self):
        return self._age * 12 
```

现在，每当调用变量`jeff.months`时，它都会返回:

```
312 
```

让我们来看看二传手。现在我们已经使用`@property`装饰器定义了一个属性，我们可以使用`@[property_name].setter`为它编写一个 setter。记住在你的类中用实际的属性名替换`property_name`。让我们看看在`Human`类中我们是如何做到这一点的:

```
class Human:
    def __init__(self, age):
        self.age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, new_age):
        self._age = new_age 
```

属性`age`上的 setter 是用`@age.setter` decorator 修饰的，这允许我们修改如何设置这个属性。例如，我们可以这样将某人的年龄限制为整数:

```
@age.setter
def age(self, new_age):
    if not isinstance(new_age, int):
    raise TypeError("Age must be an integer.")
    self._age = new_age 
```

现在，如果我们尝试运行以下代码:

```
print(jeff.age)

jeff.age = 44.5

print(jeff.age)

jeff.age = 50

print(jeff.age) 
```

以下是输出结果:

```
26
Traceback (most recent call last):
  File "main.py", line 3, in <module>
TypeError: Age must be an integer. 
```

因此，您可以看到我们的自定义 setter 函数将阻止除整数之外的任何内容被设置到`age`属性上。因为我们引发了一个 TypeError，所以每当我们试图在 age 属性上设置一个非整数值时，程序就会停止运行。错误信息和回溯一起显示，告诉我们*在*哪里发生了错误。

## 结论

简而言之，这就是室内装潢师！总之，这是一种非常强大的方法，可以更好地控制属性在类中的设置和格式化。通过使用自定义 getters，您可以修改属性的显示方式。通过使用自定义设置器，您可以控制允许如何设置每个属性。

如果你想学习更多关于 Python 的知识，可以考虑参加我们的[完整 Python 课程](https://go.tecla.do/complete-python-sale)，它将带你从初级到高级(包括 OOP、web 开发、异步开发等等！).我们有 30 天的退款保证，所以你试一试也不会有什么损失。我们很希望你能来！

照片由[萨法·萨法罗夫](https://unsplash.com/@safarslife?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)T5 拍摄