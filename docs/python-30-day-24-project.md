# 第 24 天项目:掷骰子| Teclado

> 原文：<https://blog.teclado.com/python-30-day-24-project/>

欢迎来到 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[天 24](/30-days-of-python/python-30-day-24-exceptions-advanced) 项目！在这个项目中，我们将使用标准库中一个名为`argparse`的模块，在程序运行之前从用户那里获取配置。

我们将要编写的程序演示这是一个命令行掷骰子的程序，它可以模拟各种骰子配置的掷骰子。

在我们真正做到这一点之前，我们需要了解一下当我们按下“Run”按钮时 repl.it 做了什么，我们还需要了解一下`argparse`模块本身。

另外，我们已经有了这篇博文的[视频预览](https://youtu.be/QueRWpsVy3o)！

## 运行 Python 代码

那些一直在本地开发环境中工作的人可能已经学会了如何运行 Python 程序，但是对于我们其他人来说，这一步已经被隐藏在 repl 后面了，它是“运行”按钮。

那么，当我们按下这个按钮时会发生什么呢？

当我们按下按钮时，repl.it 会运行一个如下所示的命令:

这就是 repl.it 总是运行`main.py`的原因。它运行`main.py`,因为这是它指定作为默认运行命令一部分的文件。

如果我们愿意，我们实际上可以配置 repl.it 使用不同的 run 命令，我们通过在 repl 中创建一个名为`.replit`的特殊文件来实现这一点。这个文件是以一种叫做 TOML 的格式编写的(它代表了 **T** om 的 **O** bvious，**M**inimal**L**language)，这是配置文件的一种常见格式，因为它很容易编写和读取。

让我们试试改变 run 命令，因为这是我们在这个项目中要做的事情。

首先，创建一个名为`app.py`的文件，并将一些代码放入其中，以便您可以验证它何时运行。类似这样的事情会发生:

app.py

```
`print("Hello from app.py")` 
```

现在创建一个名为`.replit`的文件，并将以下代码放入其中:

现在按下“运行”按钮。如果一切正常，你的`app.py`应该跑而不是`main.py`。

## 运行带有标志和参数的程序

我们可以对许多真实世界的控制台应用程序做的一件事是用标志和参数运行它们。这些标志用于配置程序的运行方式，或者通过打开某些设置，或者通过为各种选项提供值。

例如，如果我们想用`help`标志运行我们的`app.py`文件，我们可以这样做:

该`--help`标志通常用于找出关于如何使用程序的信息。

然而，目前我们还不能使用这面旗帜。我们的程序不知道这是什么意思。这就是`argparse`的用武之地:它让我们指定我们将接受哪些标志和参数，并且它给我们一种方法来访问用户在调用我们的应用程序时指定的值。

## 快速浏览`argparse`

在这里，我们将创建一个小程序，返回一个数字的特定幂，以了解一些`argparse`概念。

### 创建解析器

为了使用`argparse`,我们首先需要导入模块并创建一个`ArgumentParser`,如下所示:

app.py

```
`import argparse

parser = argparse.ArgumentParser()` 
```

如果我们愿意，我们可以在创建这个`ArgumentParser`时通过传入一个值来指定我们程序的描述。

app.py

```
`parser = argparse.ArgumentParser(description="Returns a number raised to a specified power.")` 
```

### 指定位置参数

现在我们有了这个`parser`，是时候开始指定参数了。首先，让我们在用户调用我们的应用程序时接受一个位置参数。

要接受这个论点，我们需要写出以下内容:

app.py

```
`import argparse

parser = argparse.ArgumentParser(description="Returns a number raised to a specified power.")
parser.add_argument("base", help="A number to raise to the specified power")` 
```

这里我们调用了`parser`上的`add_argument`方法，传入了两个值。

第一个，`"base"`是将要接受用户参数的参数名。稍后我们将使用这个名称来获取值。

我们使用关键字参数指定的第二个值，它将被`argparse`用来为我们的程序创建文档。

为了处理用户传入的值，我们还需要一件事:我们需要解析用户传入的参数。

app.py

```
`import argparse

parser = argparse.ArgumentParser(description="Returns a number raised to a specified power.")
parser.add_argument("base", help="A number to raise to the specified power")

args = parser.parse_args()

print(args.base)  # access the value of the base argument` 
```

现在我们可以将我们的`.replit`文件修改成这样:

```
`run = "python app.py --help"` 
```

只要确保将文件名更改为您编写所有代码的位置。如果一切正常，您应该能够按下 run 按钮并得到如下输出:

```
`usage: app.py [-h] base

Returns a number raised to a specified power.

positional arguments:
  base        A number to raise to the specified power

optional arguments:
  -h, --help  show this help message and exit` 
```

这是`argparse`为我们创建的文档。

现在让我们将`.replit`文件改为这样:

现在我们应该把数字`23`打印到控制台上，就像我们在文件中指定的那样。

### 指定可选参数

现在让我们快速看一下可选参数，它们是使用标志传递的。我们以同样的方式创建它们，但是我们在名称前面使用了一个`--`。

我们将使用可选参数指定一个指数，如果用户没有提供值，我们将设置默认值`2`。

app.py

```
`import argparse

parser = argparse.ArgumentParser(description="Returns a number raised to a specified power.")

parser.add_argument("base", help="A number to raise to the specified power")
parser.add_argument("--exponent", help="A power to raise the provided base to")

args = parser.parse_args()

print(args.base)
print(args.exponent)` 
```

您可能已经从帮助输出中注意到，`--help`有一个快捷形式:`-h`。我们可以对可选参数做同样的事情，为第二个名字提供一个`-`。

app.py

```
`import argparse

parser = argparse.ArgumentParser(description="Returns a number raised to a specified power.")

parser.add_argument("base", help="A number to raise to the specified power")
parser.add_argument("-e", "--exponent", help="A power to raise the provided base to")

args = parser.parse_args()

print(args.base)
print(args.exponent)` 
```

我们还可以做一些最后的事情来改进我们的程序。首先，我们应该为`exponent`设置一个默认值，并且我们应该为每个值指定我们期望的类型。

app.py

```
`import argparse

parser = argparse.ArgumentParser(description="Returns a number raised to a specified power.")

parser.add_argument("base", type=float, help="A number to raise to the specified power")
parser.add_argument(
    "-e",
    "--exponent",
    type=float,
    default=2,
    help="A power to raise the provided base to"
)

args = parser.parse_args()

print(args.base ** args.exponent)` 
```

现在我们可以将我们的`.replit`文件修改成这样:

```
`run = "python app.py 2 -e 5"` 
```

而我们的程序输出`32.0`，用的是 2⁵.

如果你想更详细地了解`argparse`，你可以在[这里](https://docs.python.org/3/howto/argparse.html)的文档中找到一个非常好的教程。

## 案情摘要

既然我们已经了解了一点，我们就可以开始这个项目的实质部分了。在这个项目中，我们将为 n 面骰子创建一个骰子滚动器。

用户将能够使用下面的语法指定骰子的选择，其中在`d`之前的数字代表骰子的数量，在`d`之后的数字代表这些骰子有多少面。

在这种情况下，用户请求了三个六面骰子。

使用`random`模块，我们将模拟用户请求的骰子滚动，我们将在控制台中输出一些结果，如下所示:

```
`Rolls: 1, 2, 4
Total: 7
Average: 2.33` 
```

这里我们有滚动的数字、值的总和以及滚动的平均值。

除了将这个结果打印到控制台，我们还将在一个名为`roll_log.txt`的文件中保存一个永久的掷骰子日志。如果用户愿意，他们可以使用名为`--log`的选项参数指定不同的日志文件。

```
`python main.py 2d10 --log rolls.txt` 
```

除了指定自定义日志文件，用户还可以使用`--repeat`标志指定掷骰子的次数。

```
`python main.py 6d4 --repeat 2` 
```

`--repeat`和`--log`都应该有适当的文档，用户应该能够使用`-r`和`-l`作为标志的简短版本。如果用户愿意，他们也可以使用`--repeat`和`--log`标志。

祝你好运！

## 我们的解决方案

首先，让我们设置我们的解析器。我将把它放在自己的`parser.py`文件中，以及任何处理参数解析的代码中。

parser.py

```
`import argparse

parser = argparse.ArgumentParser(description="A command line dice roller")

args = parser.parse_args()` 
```

我们需要为我们的应用程序注册三个参数:一个位置参数和两个可选参数。

position 参数将捕捉用户使用我们的`xdy`语法指定的骰子配置。

这两个可选参数将捕获滚动的重复次数，以及记录滚动的位置。这两个参数都需要默认值。

让我们从位置论点开始，我把它叫做`dice`。

parser.py

```
`import argparse

parser = argparse.ArgumentParser(description="A command line dice roller")

parser.add_argument("dice",  help="A representation of the dice you want to roll")

args = parser.parse_args()` 
```

我们在这里不需要做什么特别的事情。输入将是一个字符串，我们不需要指定任何标志或默认值。我们唯一需要做的就是为程序文档指定一些帮助文本。

两个可选参数要复杂得多。首先让我们处理一下`--repeat`的论点。

`--repeat`应该有一个默认值`1`，因为如果用户没有指定重复值，很可能是因为他们不想重复掷骰子。同样重要的是，我们要确保这里得到的是一个整数，而不是浮点数，或者不能用整数表示的东西。例如，重复一次掷骰子`2.6`没有多大意义。

考虑到这一点，我认为这个参数的一个合适的实现应该是这样的:

parser.py

```
`import argparse

parser = argparse.ArgumentParser(description="A command line dice roller")

parser.add_argument("dice",  help="A representation of the dice you want to roll")
parser.add_argument(
    "-r",
    "--repeat",
    metavar="number",
    default=1,
    type=int,
    help="How many times to roll the specifed set of dice"
)

args = parser.parse_args()` 
```

我在这里添加了一个参数`metavar`的值。这将改变程序文档中显示为值的占位符的内容。

现在让我们添加`--log`参数配置。

在这种情况下，我们希望为日志指定一个默认文件名，可以是您想要的任何名称。我要用`roll_log.txt`。

我们可能还想确保我们得到的值是一个字符串，所以我将为这个参数指定一个类型`str`。

parser.py

```
`import argparse

parser = argparse.ArgumentParser(description="A command line dice roller")

parser.add_argument("dice",  help="A representation of the dice you want to roll")
parser.add_argument(
    "-r",
    "--repeat",
    metavar="number",
    default=1,
    type=int,
    help="How many times to roll the specifed set of dice"
)
parser.add_argument(
    "-l",
    "--log",
    metavar="path",
    default="roll_log.txt",
    type=str,
    help="A file to use to log the result of the rolls"
)

args = parser.parse_args()` 
```

看起来不错！

这个`parser.py`文件中唯一要做的事情就是实际解析 dice 规范。假设用户的值一切正常，这应该和用字符`"d"`分割字符串并将结果列表中的值转换为`integers`一样简单。

parser.py

```
`def parse_roll(args):
    quantity, die_size = [int(arg) for arg in args.dice.split("d")]

    return quantity, die_size` 
```

然而，总是有用户输入的更改和无效的配置，所以我们需要做一些异常处理。

我们将首先捕获一个`ValueError`，它将捕获用户输入类似`fd6`、`d6`或`6d`的情况。

`d6`和`6d`将被捕获，因为如果在`"d"`之前或之后没有任何特征，`""`将出现在`strip`返回的列表中。如果我们试图将`""`传递给`int`，我们会得到一个`ValueError`。

parser.py

```
`def parse_roll(args):
    try:
        quantity, die_size = [int(arg) for arg in args.dice.split("d")]
    except ValueError:
        raise ValueError("Invalid dice specification. Rolls must be in the format of 2d6") from None

    return quantity, die_size` 
```

在这种情况下，我们实际上无法做任何事情来正确处理这个错误，因为我们不知道用户的意图，但是我提出了一个新的`ValueError`异常，对用户来说更有帮助。我决定使用`from None`来提高，这样用户就可以得到回溯的精简版本。

实际上,`ValueError`也给我们带来了另一个问题:有太多的值要解包。

尝试做类似下面例子的事情会导致一个`ValueError`:

如果我们想给用户提供更有帮助的反馈，我们可以把这种理解分解成不同的步骤，但是我认为这对于我们的情况已经足够好了。

现在让我们转向`main.py`，在这里我们将利用我们在`parser.py`中定义的这些东西。

很短，只是将我们在其他文件中定义的各种函数组合成一个有用的应用程序。

首先，我们将导入`parser`和`random`，并且我们将获得我们在`parser.py`中定义的`args`变量。

main.py

```
`import parser
import random

args = parser.args` 
```

现在我们已经掌握了这一点，我们可以调用`parser.parse_rolls`，传入这个`args`值。我们还可以获得指定的重复次数和指定的日志文件，并给它们起一个更好的名字。

main.py

```
`import parser
import random

args = parser.args

quantity, die_size = parser.parse_roll(args)
repetitions = args.repeat
log_file = args.log` 
```

现在我们有了所有我们需要的信息，我们可以开始实际模拟骰子滚动。对于单个滚动，逻辑将如下所示:

```
`rolls = [random.randint(1, die_size)  for _ in  range(quantity)]
total = sum(rolls)
average = total / len(rolls)` 
```

我们使用列表理解为用户指定的每个骰子调用一次`randint`。因此，如果我们得到了`3d6`，我们将生成一个包含 3 个结果的列表。

`randint`从包含的范围中为我们选择一个数字，所以我们只需要指定`1`为骰子的大小。

一旦我们将结果存储在`rolls`中，我们就可以计算`total`和`average`。

然而，所有这些逻辑将会形成一个循环，因为我们可能需要重复几次滚动。

main.py

```
`import parser
import random

args = parser.args

quantity, die_size = parser.parse_roll(args)
repetitions = args.repeat
log_file = args.log

for _ in  range(repetitions):
    rolls = [random.randint(1, die_size)  for _ in  range(quantity)]
    total = sum(rolls)
    average = total / len(rolls)` 
```

目前，我们并没有对任何结果做任何事情，所以让我们通过编写一些函数来处理结果的格式，并写入我们的日志文件来解决这个问题。

我将把所有这些功能保存在名为`output.py`的第三个文件中。随便你给它起什么名字。

这个文件的内容很容易理解，所以我们可以轻松地浏览一下。

output.py

```
`roll_template = """Rolls: {}
Total: {}
Average: {}
"""

def format_result(rolls, total, average):
    rolls = ", ".join(str(roll) for roll in rolls)
    return roll_template.format(rolls, total, average)

def log_result(rolls, total, average, log_file):
    with open(log_file, "a")  as log:
        log.write(format_result(rolls, total, average))
        log.write("-" * 30 + "\n")` 
```

首先，我定义了一个可以填充值的模板。像这样使用多行字符串有助于避免大量的`"\n"`字符。

然后我定义了一个名为`format_results`的函数，它实际上用值填充这个模板。它还负责将卷连接在一起，以便我们的输出中没有任何方括号。

`log_results`函数完全与写入日志文件有关。它接受一个日志文件作为参数，并使用上下文管理器以追加模式打开该文件。如果文件不存在，这将创建它。

打开文件后，`log_results`然后格式化卷并将结果写入文件，在新的一行上跟随 30 个`-`字符。这将作为文件中的分隔符。

这样，我们只需在`main.py`中导入`output`并调用我们的函数。

main.py

```
`import output
import parser
import random

args = parser.args

quantity, die_size = parser.parse_roll(args)
repetitions = args.repeat
log_file = args.log

for _ in  range(repetitions):
    rolls = [random.randint(1, die_size)  for _ in  range(quantity)]
    total = sum(rolls)
    average = total / len(rolls)

    print(output.format_result(rolls, total, average))
    output.log_result(rolls, total, average, log_file)` 
```

现在是时候用各种测试用例来测试我们的程序了。以下是一组有效参数的情况:

```
`> python main.py 8d10 -r 3
Rolls: 3, 3, 1, 3, 8, 8, 8, 9
Total: 43
Average: 5.375

Rolls: 10, 7, 5, 6, 10, 2, 3, 6
Total: 49
Average: 6.125

Rolls: 10, 6, 10, 6, 2, 4, 6, 4
Total: 48
Average: 6.0` 
```

而这里是`roll_log.txt`的内容:

```
`Rolls: 3, 3, 1, 3, 8, 8, 8, 9
Total: 43
Average: 5.375
------------------------------
Rolls: 10, 7, 5, 6, 10, 2, 3, 6
Total: 49
Average: 6.125
------------------------------
Rolls: 10, 6, 10, 6, 2, 4, 6, 4
Total: 48
Average: 6.0
------------------------------` 
```

我们还可以使用`-h`或`--help`标志来查看我们可爱的生成文档:

```
`usage: main.py [-h] [-r number] [-l path] dice

A command line dice roller

positional arguments:
  dice                  A representation of the dice you want to roll

optional arguments:
  -h, --help            show this help message and exit
  -r number, --repeat number
                        How many times to roll the specifed set of dice
  -l path, --log path   A file to use to log the result of the rolls` 
```

希望你能够自己解决这个问题，即使你用一种和我完全不同的方式。如果你想分享，我们很乐意在我们的 [Discord 服务器](https://discord.gg/BBWwyMq)上看到你的一些解决方案！

## 额外资源

如果你想更深入地了解`argparse`，那么你一定要查看一下[教程](https://docs.python.org/3/howto/argparse.html)和[主文档页面](https://docs.python.org/3/library/argparse.html#module-argparse)。