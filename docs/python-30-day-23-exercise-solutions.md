# 第 23 天:锻炼解决方案| Teclado

> 原文：<https://blog.teclado.com/python-30-day-23-exercise-solutions/>

以下是我们对 Python 系列 [30 天中](https://blog.teclado.com/30-days-of-python/)[第 23 天练习](/30-days-of-python/python-30-day-23-generators-yield)的解决方案。在检查解决方案之前，请确保您亲自尝试了这些练习！

### 1)编写一个生成器，生成指定范围内的素数。

我将使用[第 8 天练习](/30-days-of-python/python-30-day-8-while-loops/)中的解决方案作为起点，因为逻辑几乎完全相同:

```py
`for dividend in range(2, 101):
    for divisor in range(2, dividend):
        if dividend % divisor == 0:
            break
    else:
        print(dividend)` 
```

如果你不确定为什么会这样，我建议你看看[原始练习演练](/30-days-of-python/python-30-day-8-exercise-solutions/)。

基于我们之前的解决方案，我们需要做的第一件事是创建一个函数并将所有代码放入其中。

```py
`def gen_primes(limit):
    for dividend in range(2, limit + 1):
        for divisor in range(2, dividend):
            if dividend % divisor == 0:
                break
        else:
            print(dividend)` 
```

在这里，我添加了一个名为`limit`的参数，它将决定我们的生成器最终停止的位置。它决定了我们要检查的最高数字。

就这样，我们差不多完成了。现在我们只需要通过使用`yield`关键字将我们的函数变成一个生成器。我们将把它放在内部`for`循环的`else`分支中，而不是打印`dividend`。

```py
`def gen_primes(limit):
    for dividend in range(2, limit + 1):
        for divisor in range(2, dividend):
            if dividend % divisor == 0:
                break
        else:
            yield dividend` 
```

当我们调用函数时，我们得到一个生成器迭代器:

```py
`def gen_primes(limit):
    for dividend in range(2, limit + 1):
        for divisor in range(2, dividend):
            if dividend % divisor == 0:
                break
        else:
            yield dividend

primes = gen_primes(101)  # <generator object gen_primes at 0x7f02ca556c80>` 
```

我们可以使用这个生成器迭代器，使用`next`函数一次检索一个素数:

```py
`...

primes = gen_primes(101)  # <generator object gen_primes at 0x7f02ca556c80>

print(next(primes))  # 2
print(next(primes))  # 3
print(next(primes))  # 5` 
```

### 2)下面我们有一个使用`map`处理列表中名字的例子。使用生成器表达式重写这段代码。

```py
`names = [" rick", " MORTY  ", "beth ", "Summer", "jerRy    "]

names = map(lambda name: name.strip().title(), names)` 
```

生成器表达式语法就像我们之前写的理解一样。唯一的区别是，我们使用常规圆括号，而不是方括号或花括号。

```py
`names = [" rick", " MORTY  ", "beth ", "Summer", "jerRy    "]

names = (name.strip().split() for name in names)` 
```

对于像这样的情况，我们被迫使用 lambda 表达式，生成器表达式通常会更短，可读性更好。

## 3)编写一个小程序，为德州扑克游戏发牌。

交易顺序有点复杂，但在第 23 天的帖子中有详细的解释[，所以如果你需要提醒，请查看那里。](/30-days-of-python/python-30-day-23-generators-yield)

首先，我们需要创建我们的卡片组。有几种方法可以做到这一点。

你可能会考虑这样一个选项:

```py
`ranks = (2, 3, 4, 5, 6, 7, 8, 9, 10, "jack", "queen", "king", "ace")
suits = ("clubs", "diamonds", "hearts", "spades")

cards = []

for rank in ranks:
    for suit in suits:
        cards.append((rank, suit))` 
```

我们可以通过打印`cards`和`len(deck)`来验证所有的牌都在那里，对于一副标准的 52 张牌来说，这应该是`52`。

因为我们正在从现有的集合中创建一个新的列表，所以也可以用如下的理解来完成:

```py
`ranks = (2, 3, 4, 5, 6, 7, 8, 9, 10, "jack", "queen", "king", "ace")
suits = ("clubs", "diamonds", "hearts", "spades")

cards = [(rank, suit) for suit in suits for rank in ranks]` 
```

我们可以通过打印`cards`的内容和集合的长度来再次验证我们得到了我们想要的东西。

然而，我们可以调用`itertools.product`来代替编写这个理解，它做了非常相似的事情，并且它返回给我们一个迭代器。

```py
`import itertools

ranks = (2, 3, 4, 5, 6, 7, 8, 9, 10, "jack", "queen", "king", "ace")
suits = ("clubs", "diamonds", "hearts", "spades")

cards = list(itertools.product(ranks, suits))` 
```

这些选项中的任何一个都是完全有效的。

现在我们应该创建一个函数来洗牌并返回一个新的迭代器，这样我们就可以从中抽牌了。为了洗牌，我们将使用`random`模块中的`shuffle`函数。

```py
`import itertools
import random

def shuffle_deck(cards):
    deck = list(cards)
    random.shuffle(deck)

    return iter(deck)

ranks = (2, 3, 4, 5, 6, 7, 8, 9, 10, "jack", "queen", "king", "ace")
suits = ("clubs", "diamonds", "hearts", "spades")

cards = list(itertools.product(ranks, suits))` 
```

这个`shuffle_deck`函数涉及到几件事情。

我们接受一组`cards`作为参数，用来组成一个`deck`。这个`deck`是一个很好的列表:`random.shuffle`函数接受一个序列，并将执行一次就地洗牌。这意味着我们需要一个可变的序列类型。

然后，我们通过将`deck`传递给`random.shuffle`函数来打乱它。一旦完成，返回一个洗牌后的`deck`的迭代器，我们通过将`deck`传递给`iter`函数来创建这个迭代器。

接下来我们需要处理一些用户交互，因为我们需要知道用户想要和多少玩家一起玩。

在这种情况下，我实际上要定义一个函数，因为我想处理任何无效的输入，如果用户提供无效的输入，我想重新提示用户。我还想检查一下，确保玩家的数量在练习中指定的范围内(2 - 10)。

```py
`def get_players():
    while True:
        number_of_players = input("How many players are there? ").strip()

        try:
            number_of_players = int(number_of_players)
        except ValueError:
            print("You must enter an integer.")
        else:
            if number_of_players in range(2, 11):
                return number_of_players
            elif number_of_players < 2:
                print("You must have at least 2 players.")
            else:
                print("You can have a maximum of 10 players.")` 
```

试试看，确保一切正常。

假设没有问题，是时候让我们的函数发牌了。我认为有一个功能作为发牌者是有意义的，它将洗牌，并委托给专门的功能向玩家发牌，并向桌面发牌。

拥有庄家功能意味着我们可以轻松地玩多手牌。

让我们定义所有的函数，并在进行过程中构建它们:

```py
`import itertools
import random

def deal(cards, number_of_players):
    deck = shuffle_deck(cards)

    deal_to_players(deck, number_of_players)
    deal_to_table(deck)

def deal_to_players(deck, number_of_players):
    pass

def deal_to_table(deck)
    pass

def get_players():
    while True:
        number_of_players = input("How many players are there? ").strip()

        try:
            number_of_players = int(number_of_players)
        except ValueError:
            print("You must enter an integer.")
        else:
            if number_of_players in range(2, 11):
                return number_of_players
            elif number_of_players < 2:
                print("You must have at least 2 players.")
            else:
                print("You can have a maximum of 10 players.")

def shuffle_deck(cards):
    deck = list(cards)
    random.shuffle(deck)

    return iter(deck)

ranks = (2, 3, 4, 5, 6, 7, 8, 9, 10, "jack", "queen", "king", "ace")
suits = ("clubs", "diamonds", "hearts", "spades")

cards = list(itertools.product(ranks, suits))` 
```

我已经填写了`deal`函数，因为它相当简单。它只是记录组成我们的牌组的牌，以及这一轮的玩家人数。

在函数体中，我们从已定义的卡片中构造我们的洗牌，然后我们调用其他发牌函数来完成所有困难的工作。

让我们转向`deal_to_players`。首先，我们必须弄清楚如何分配牌，以便每个玩家依次得到一张牌，每个玩家最终得到两张牌。

为此，我将使用一对列表理解，如下所示:

```py
`def deal_to_players(deck, number_of_players):
    first_cards = [next(deck)  for _ in  range(number_of_players)]
    second_cards = [next(deck)  for _ in  range(number_of_players)]` 
```

重要的是，我们在这里使用列表理解，而不是生成器表达式，因为我们实际上希望这些卡现在被抽出*。一会儿你就会明白为什么这很重要。*

 *一旦我们有了列表中的第一张和第二张牌，我们就可以将它们连在一起，为每个玩家创建一手牌。

```py
`def deal_to_players(deck, number_of_players):
    first_cards = [next(deck)  for _ in  range(number_of_players)]
    second_cards = [next(deck)  for _ in  range(number_of_players)]

    hands = zip(first_cards, second_cards)` 
```

这个`zip`步骤非常重要，我们不能将懒惰的集合分配给`first_cards`和`second_cards`。不是说`zip`不能处理它们——它可以——但是我们必须考虑当我们使用这个`zip`对象时会发生什么。

当我们从`zip`请求一个项目时，它会做什么？

首先，它从`first_cards`那里要一张卡片，然后从`second_cards`那里要一张卡片，它把它们捆绑成一个元组，然后交给我们。但是`first_cards`和`second_cards`都只是从`deck`那里请求一张卡来给`zip`它想要的值，并且它们都来自同一个叠代器。

这意味着每场比赛将连续发两张牌，而不是每个球员发一张牌，然后在每个球员发了一张牌后再发一张。这是一个微妙的点，但重要的是，当我们与懒惰类型的人一起工作时，要小心这样的事情。

幸运的是，使用列表理解解决了这个问题，因为当我们创建`first_cards`时从`deck`请求卡片，当我们请求`second_cards`时再次请求卡片。当我们到达`zip`步骤时，我们已经有了卡片；我们正等着把它们捆起来。

一旦我们有了玩家的手，我们只需要打印它们。

```py
`def deal_to_players(deck, number_of_players):
    first_cards = [next(deck)  for _ in  range(number_of_players)]
    second_cards = [next(deck)  for _ in  range(number_of_players)]

    hands = zip(first_cards, second_cards)

    print()

    for i,  (first_card, second_card)  in  enumerate(hands, start=1):
        print(f"Player {i} was dealt: {first_card}, {second_card}")

    print()` 
```

现在让我们来处理应用程序的最后一部分，即将牌分发到桌子上。这相对简单，因为我们不需要担心玩家的数量，或者类似的事情。

首先，我们需要烧一张牌，然后一口气发三张牌到桌上:

```py
`def deal_to_table(deck):
    next(deck)  # burn
    flop = ', '.join(str(next(deck))  for _ in  range(3))
    print(f"The flop: {flop}")` 
```

这里需要注意的一点是`join`需要一个包含字符串的 iterable。因此，我们必须使用`str`将从`next`得到的元组转换成字符串。

从这里开始，它只是一个燃烧和处理一个单一的卡为接下来的两个步骤的处理过程。

```py
`def deal_to_table(deck):
    next(deck)  # burn
    flop = ', '.join(str(next(deck))  for _ in  range(3))
    print(f"The flop: {flop}")

    next(deck)  # burn
    print(f"The turn: {next(deck)}")

    next(deck)  # burn
    print(f"The river: {next(deck)}")
    print()` 
```

这样，我们只需要调用`deal`并确保一切正常运行。确保您可以多次调用您的`deal`函数！

```py
`import itertools
import random

def deal(cards, number_of_players):
    deck = shuffle_deck(cards)

    deal_to_players(deck, number_of_players)
    deal_to_table(deck)

def deal_to_players(deck, number_of_players):
    first_cards = [next(deck)  for _ in  range(number_of_players)]
    second_cards = [next(deck)  for _ in  range(number_of_players)]

    hands = zip(first_cards, second_cards)

    print()

    for i,  (first_card, second_card)  in  enumerate(hands, start=1):
        print(f"Player {i} was dealt: {first_card}, {second_card}")

    print()

def deal_to_table(deck):
    next(deck)  # burn
    flop = ', '.join(str(next(deck))  for _ in  range(3))
    print(f"The flop: {flop}")

    next(deck)  # burn
    print(f"The turn: {next(deck)}")

    next(deck)  # burn
    print(f"The river: {next(deck)}")
    print()

def get_players():
    while True:
        number_of_players = input("How many players are there? ").strip()

        try:
            number_of_players = int(number_of_players)
        except ValueError:
            print("You must enter an integer.")
        else:
            if number_of_players in range(2, 11):
                return number_of_players
            elif number_of_players < 2:
                print("You must have at least 2 players.")
            else:
                print("You can have a maximum of 10 players.")

def shuffle_deck(cards):
    deck = list(cards)
    random.shuffle(deck)

    return iter(deck)

ranks = (2, 3, 4, 5, 6, 7, 8, 9, 10, "jack", "queen", "king", "ace")
suits = ("clubs", "diamonds", "hearts", "spades")

cards = list(itertools.product(ranks, suits))

deal(cards, get_players())` 
```

这是一个很长的练习，有一些棘手的部分，但我希望你能够管理。如果你遇到困难，可以随时在我们的 [Discord 服务器](https://discord.gg/BBWwyMq)上提出任何问题。*