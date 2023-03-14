# 第 14 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-14-exercise-solutions/>

以下是我们为 [30 天 Python](https://blog.teclado.com/30-days-of-python/) 系列中的[第 14 天练习](/30-days-of-python/python-30-day-14-files)提供的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)使用上下文管理器重写以下代码。

```py
`f = open("hello_world.txt", "w")
f.write("Hello, World!")
f.close()` 
```

我们的上下文管理器的第一行如下所示:

```py
`with open("hello_world.txt", "w") as f:
    pass` 
```

首先我们有`with`关键字，然后是`open`调用，然后我们使用`as`提供一个变量名，这样我们就可以很容易地引用文件内容。

在这一行下面，我们有一个缩进的块，包含文件打开时我们想做的所有事情。当这些操作完成后，Python 将为我们关闭文件。

```py
`with open("hello_world.txt", "w") as f:
    f.write("Hello, World!")` 
```

### 2)使用 append 模式将`"How are you?"`写入上面的`hello_world.txt`文件的第二行。

同样，我们应该在这里使用上下文管理器，第一行几乎完全相同。我们只需要把`"w"`换成`"a"`。

```py
`with open("hello_world.txt", "a") as f:
    pass` 
```

如果您愿意，您可以为任何值使用关键字参数，并且您可以在文档中找到`open`参数[的名称。](https://docs.python.org/3/library/functions.html?highlight=open#open)

现在我们只需要调用`write`并传入`"\nHow are you?"`作为字符串。

```py
`with open("hello_world.txt", "a") as f:
    f.write("\nHow are you?")` 
```

确保不要错过`\\n`，因为我们希望这一行出现在第二行。因此，我们需要在第一行之后添加一个换行符。

### 3)获取我们从 Iris flower 数据集创建的字典列表，并将其写入 CSV 格式的新文件。

在这里，我们基本上执行与之前相同的步骤，只是方向相反。

如果你愿意，你可以使用以前的 repl，但是如果你使用的是一个新的 repl，为了方便起见，这里有一个字典列表:

```py
`irises = [
    {'sepal_length': '5.1', 'sepal_width': '3.5', 'petal_length': '1.4', 'petal_width': '0.2', 'species': 'Iris-setosa'},
    {'sepal_length': '4.9', 'sepal_width': '3',   'petal_length': '1.4', 'petal_width': '0.2', 'species': 'Iris-setosa'},
    {'sepal_length': '4.7', 'sepal_width': '3.2', 'petal_length': '1.3', 'petal_width': '0.2', 'species': 'Iris-setosa'},
    {'sepal_length': '4.6', 'sepal_width': '3.1', 'petal_length': '1.5', 'petal_width': '0.2', 'species': 'Iris-setosa'},
    {'sepal_length': '5',   'sepal_width': '3.6', 'petal_length': '1.4', 'petal_width': '0.2', 'species': 'Iris-setosa'},
    {'sepal_length': '7',   'sepal_width': '3.2', 'petal_length': '4.7', 'petal_width': '1.4', 'species': 'Iris-versicolor'},
    {'sepal_length': '6.4', 'sepal_width': '3.2', 'petal_length': '4.5', 'petal_width': '1.5', 'species': 'Iris-versicolor'},
    {'sepal_length': '6.9', 'sepal_width': '3.1', 'petal_length': '4.9', 'petal_width': '1.5', 'species': 'Iris-versicolor'},
    {'sepal_length': '5.5', 'sepal_width': '2.3', 'petal_length': '4',   'petal_width': '1.3', 'species': 'Iris-versicolor'},
    {'sepal_length': '6.5', 'sepal_width': '2.8', 'petal_length': '4.6', 'petal_width': '1.5', 'species': 'Iris-versicolor'},
    {'sepal_length': '6.3', 'sepal_width': '3.3', 'petal_length': '6',   'petal_width': '2.5', 'species': 'Iris-virginica'},
    {'sepal_length': '5.8', 'sepal_width': '2.7', 'petal_length': '5.1', 'petal_width': '1.9', 'species': 'Iris-virginica'},
    {'sepal_length': '7.1', 'sepal_width': '3',   'petal_length': '5.9', 'petal_width': '2.1', 'species': 'Iris-virginica'},
    {'sepal_length': '6.3', 'sepal_width': '2.9', 'petal_length': '5.6', 'petal_width': '1.8', 'species': 'Iris-virginica'},
    {'sepal_length': '6.5', 'sepal_width': '3',   'petal_length': '5.8', 'petal_width': '2.2', 'species': 'Iris-virginica'}
]` 
```

首先，让我们把重点放在以 CSV 格式打印值上。

因为我们需要对列表中的每个字典进行一些转换，我们知道我们可能需要类似于`for`循环的东西，所以让我们设置一下。

将字典转换成 CSV 格式实际上相对简单。我们可以创建一个 f 字符串，其中的值用逗号分隔。

因为这一行会很长，我建议将字符串分成两行，然后连接起来，就像这样:

```py
`for iris in irises:
    print(
        f"{iris['sepal_length']},{iris['sepal_width']},{iris['petal_length']}," +
        f"{iris['petal_width']},{iris['species']}"
    )` 
```

这很好，但我认为我们可以做得更好。

如果我们想坚持 f-string 方法，我们可以做的一件事是用`iris.values()`获取值，并将它们析构为一系列变量。这将节省我们在 f 字符串中使用所有这些订阅表达式的时间。

```py
`for iris in irises:
    sepal_length, sepal_width, petal_length, petal_width, species = iris.values()
    print(f"{sepal_length},{sepal_width},{petal_length},{petal_width},{species}")` 
```

这无疑是朝着正确方向迈出的一步，但我认为我们还可以做得更好。

我们所有的字典值都是字符串，我们试图用逗号将这些字符串连接在一起，所以让我们使用`join`方法。

```py
`for iris in irises:
    print(",".join(iris.values()))` 
```

如你所见，`join`在这种情况下非常强大。代码非常短，也非常容易理解。

在这个阶段，我们应该明确地运行我们的代码，并确保一切看起来都是正确的。如果没有任何问题，我们可以用调用`write`来替换`print`，这样我们就可以写入文件。

```py
`with open("iris_2.csv", "w") as iris_file:
    for iris in irises:
        iris_file.write(",".join(iris.values()))` 
```

如果您运行这段代码，我们不会得到我们想要的，因为我们忘记了一个重要的细节:我们没有在字符串的末尾添加一个换行符。因此，所有内容都将写在一行上。

我们可以将字符串`"\\n"`连接到我们写入文件的每个字符串:

```py
`with open("iris_2.csv", "w") as iris_file:
    for iris in irises:
        iris_file.write(",".join(iris.values()) + "\n")` 
```

如果我们运行代码并看一看`iris_2.csv`，事情应该是这样的:

```py
`5.1,3.5,1.4,0.2,Iris-setosa
4.9,3,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5,3.6,1.4,0.2,Iris-setosa
7,3.2,4.7,1.4,Iris-versicolor
6.4,3.2,4.5,1.5,Iris-versicolor
6.9,3.1,4.9,1.5,Iris-versicolor
5.5,2.3,4,1.3,Iris-versicolor
6.5,2.8,4.6,1.5,Iris-versicolor
6.3,3.3,6,2.5,Iris-virginica
5.8,2.7,5.1,1.9,Iris-virginica
7.1,3,5.9,2.1,Iris-virginica
6.3,2.9,5.6,1.8,Iris-virginica
6.5,3,5.8,2.2,Iris-virginica` 
```