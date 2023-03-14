# 第 30 天项目:蛇

> 原文：<https://blog.teclado.com/python-30-day-30-project/>

欢迎参加 Python 系列 [30 天](https://blog.teclado.com/30-days-of-python/)[最后一天](/30-days-of-python/python-30-day-30-graduation)的毕业设计。今天我们用`pygame`来建造蛇。

如果你还没有查看过[准备帖](/30-days-of-python/python-30-day-30-project-preparation)，我绝对推荐你去看看，因为我在那里会谈到如何使用`pygame`。

## 案情摘要

今天的简报相当简单。你的任务是创建经典的[蛇游戏](https://en.wikipedia.org/wiki/Snake_(video_game_genre))。

如果你不熟悉这个游戏，你扮演一条试图收集小块食物的蛇。蛇头每通过一个装有食物的方块，你就得一分，蛇也增长一定的量。

如果你撞到自己的身体或游戏区的墙壁，你就会死亡，游戏结束。

游戏的目的是尽可能多地得分。

对于这个实现，游戏应该由键盘按键控制。箭头键很有意义，但你可以选择任何你喜欢的键。按下这些不同的键会为蛇设定一个新的路线。

一次只能有一块食物放在游戏区，当蛇吃掉一块食物时，另一块食物应该随机放在游戏区内。这不应该与蛇的任何部分占据相同的空间。

最后，用户分数的累计应该显示在屏幕的左上角。

## 我们的解决方案

首先，让我们完成使用`pygame`时需要做的所有标准设置。

app.py

```py
`import pygame

WINDOW_HEIGHT = 840
WINDOW_WIDTH = 800
WINDOW_DIMENSIONS = WINDOW_WIDTH, WINDOW_HEIGHT

pygame.init()
pygame.display.set_caption("Snake")

clock = pygame.time.Clock()
screen = pygame.display.set_mode(WINDOW_DIMENSIONS)

def play_game():
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return

        clock.tick(30)

play_game()` 
```

我还将定义第二个名为`colours.py`的文件，它将包含我们希望在游戏中使用的所有颜色常量。

这个文件里没有什么复杂的东西，我们不需要在细节上纠缠太多。

colours.py

```py
`from collections import namedtuple

Colour = namedtuple("Colour", ["r", "g", "b"])

BACKGROUND = Colour(r=1, g=22, b=39)
SNAKE = Colour(r=255, g=0, b=25)
FOOD = Colour(r=255, g=253, b=65)
TEXT = Colour(r=255, g=255, b=255)` 
```

这里我定义了一个`Colour`元组，并用它来制作我打算在应用程序中使用的每种颜色的 RGB 表示。

让我们将它导入到我的`app.py`文件中，并设置应用程序的背景颜色，为深蓝色。

app.py

```py
`import pygame
import colours

WINDOW_HEIGHT = 840
WINDOW_WIDTH = 800
WINDOW_DIMENSIONS = WINDOW_WIDTH, WINDOW_HEIGHT

pygame.init()
pygame.display.set_caption("Snake")

clock = pygame.time.Clock()
screen = pygame.display.set_mode(WINDOW_DIMENSIONS)

def play_game():
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return

        screen.fill(colours.BACKGROUND)
        pygame.display.update() 

        clock.tick(30)

play_game()` 
```

我已经将`fill`调用放入循环中，因为我们想在`clock`的每个节拍都运行这个调用，以掩盖任何旧的绘图。我们还必须记住调用`update`函数，这样变化就会反映在我们想要绘制的表面上——在本例中是`screen`。

在这一点上，我们应该停下来，想想我们实际上是如何实现这里的一些逻辑的。例如，我们将如何处理移动蛇？

我认为一个好的方法是记录各个蛇段的位置，因为我们可以使用这些位置来绘制构成蛇身体的各个方块。

当我们移动蛇的时候，我将在蛇的头部添加一个新的线段，并且我将移除尾部的最后一个元素。这将确保蛇保持相同的长度，这意味着我们的蛇不断向我们想要的方向移动。

我们将需要一些逻辑来将方向映射到各种键上，以便我们知道蛇下一步应该向哪个方向移动，并且我们还需要跟踪当前方向，因为用户可能在刻度之间没有按任何键。

对于头部的实际运动，我将按照单个片段的大小来移动。这将使碰撞检测更加容易。因为我们会经常使用段大小，所以我们可能也应该将它作为一个常量添加进来。

让我们添加这个常量，并在屏幕上为蛇和食物绘制一些初始形状。

app.py

```py
`import colours
import pygame

WINDOW_HEIGHT = 840
WINDOW_WIDTH = 800
WINDOW_DIMENSIONS = WINDOW_WIDTH, WINDOW_HEIGHT

SEGMENT_SIZE  =  20

pygame.init()
pygame.display.set_caption("Snake")

clock = pygame.time.Clock()
screen = pygame.display.set_mode(WINDOW_DIMENSIONS)

def draw_objects(snake_positions, food_position):
    pygame.draw.rect(screen, colours.FOOD, [food_position, (SEGMENT_SIZE, SEGMENT_SIZE)])

    for x, y in snake_positions:
        pygame.draw.rect(screen, colours.SNAKE, [x, y, SEGMENT_SIZE, SEGMENT_SIZE])

def play_game():
    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = (160, 160)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        pygame.display.update() 

        clock.tick(30)

play_game()` 
```

为了绘制形状，我定义了另一个函数，因为我不想让`play_game`函数变得太混乱。

我还为蛇段和`play_game`里面的食物设定了一个起始位置。我们将这两者传递给`draw_objects`来绘制所有需要的形状。

实际的绘图相当简单。我们只是使用`rect`函数创建一些矩形，并使用传递给`draw_objects`的位置作为这些`rect`调用的参数。

目前，我们的食物位置总是一样的，但我希望以后这是随机的，我认为食物也有一个随机的开始位置会很好。

让我们定义一个新的函数来设置一个新的食物位置。

app.py

```py
`def set_new_food_position(snake_positions):
    while True:
        x_position = randint(0, 39) * SEGMENT_SIZE
        y_position = randint(2, 41) * SEGMENT_SIZE
        food_position = (x_position, y_position)

        if food_position not in snake_positions:
            return food_position` 
```

这可能看起来有点奇怪，但是让我们回顾一下我所做的。

因为我们不能让食物和任何一段蛇出现在同一个方块中，所以我们可能需要多次尝试来确定食物的位置。出于这个原因，我将所有代码放在了一个`while True`循环中。

在循环内部，我们使用来自`random`模块的`randint`函数生成一个随机数，然后乘以`SEGMENT_SIZE`常数。实际上，我们在这里做的是从网格中选择一个正方形，然后我们使用`SEGMENT_SIZE`将它转换成一个坐标。这确保了我们总是把食物放在一个区域内。

这里要记住的一件重要的事情是，矩形的位置是基于左上角的，所以我们应该相应地生成我们的随机数。我们不想错误地把食物放在游戏区之外。

你还会注意到`y_position`的数字不同于`x_position`的数字。这是为了让我们有得分的空间。

让我们将这个新函数插入到我们的其他代码中，并导入`random`。

app.py

```py
`import colours
import pygame

from random import randint

WINDOW_HEIGHT = 840
WINDOW_WIDTH = 800
WINDOW_DIMENSIONS = WINDOW_WIDTH, WINDOW_HEIGHT

SEGMENT_SIZE  =  20

pygame.init()
pygame.display.set_caption("Snake")

clock = pygame.time.Clock()
screen = pygame.display.set_mode(WINDOW_DIMENSIONS)

def draw_objects(snake_positions, food_position):
    pygame.draw.rect(screen, colours.FOOD, [food_position, (SEGMENT_SIZE, SEGMENT_SIZE)])

    for x, y in snake_positions:
        pygame.draw.rect(screen, colours.SNAKE, [x, y, SEGMENT_SIZE, SEGMENT_SIZE])

def set_new_food_position(snake_positions):
    while True:
        x_position = randint(0, 39) * SEGMENT_SIZE
        y_position = randint(2, 41) * SEGMENT_SIZE
        food_position = (x_position, y_position)

        if food_position not in snake_positions:
            return food_position

def play_game():
    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = set_new_food_position(snake_positions)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        pygame.display.update() 

        clock.tick(30)

play_game()` 
```

如果我们运行这个程序，食物会出现在一个随机的位置。

接下来，我要处理分数，因为这是一个相当简单的步骤。我们只需给`play_game`添加一个起始分数变量，然后我们需要渲染文本并将其传送到`screen`表面。

app.py

```py
`...

def play_game():
    score = 0

    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = set_new_food_position(snake_positions)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        font = pygame.font.Font(None, 28)
        text = font.render(f"Score: {score}", True, colours.TEXT)
        screen.blit(text, (10, 10))

        pygame.display.update() 

        clock.tick(30)

play_game()` 
```

接下来，我将处理移动蛇。因为我们在这里需要相当多的逻辑，因为我们需要检查蛇的方向，我将创建另一个函数。

app.py

```py
`def move_snake(snake_positions, direction):
    head_x_position, head_y_position = snake_positions[0]

    if direction == "Left":
        new_head_position = (head_x_position - SEGMENT_SIZE, head_y_position)
    elif direction == "Right":
        new_head_position = (head_x_position + SEGMENT_SIZE, head_y_position)
    elif direction == "Down":
        new_head_position = (head_x_position, head_y_position + SEGMENT_SIZE)
    elif direction == "Up":
        new_head_position = (head_x_position, head_y_position - SEGMENT_SIZE)

    snake_positions.insert(0, new_head_position)
    del snake_positions[-1]` 
```

我在这里做的第一件事是将头部位置分割成`x`和`y`坐标。这使我们在修改磁头位置时不必经常参考索引。

函数体的大部分被一个大的条件语句占据，在这里我们检查蛇的当前方向。然后，我们根据蛇前进的方向创建下一个头部位置。

一旦我们构建了这组新的坐标，我们使用`insert`方法将它添加到`snake_positions`列表的前面，然后我们删除尾部，因为尾部现在将占据原来的倒数第二个位置。

我们现在可以在`play_game`中设置蛇的初始方向，并调用这个函数让蛇移动。

app.py

```py
`...

def play_game():
    score = 0

    current_direction = "Right"
    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = set_new_food_position(snake_positions)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        font = pygame.font.Font(None, 28)
        text = font.render(f"Score: {score}", True, colours.TEXT)
        screen.blit(text, (10, 10))

        pygame.display.update()

        move_snake(snake_positions, current_direction)

        clock.tick(30)` 
```

问题是，这条蛇现在飞出了屏幕，所以我们需要修改一些东西。首先，我们需要让用户控制蛇的方向，其次，如果蛇与游戏区域的“墙壁”发生碰撞，我们需要结束游戏。

为了让用户控制这条蛇，我们需要做一些事情。首先，我们需要监听按键事件，这样我们就可以看到他们按下了什么键。一旦我们掌握了关键，我们需要弄清楚它实际上意味着什么。为此，我将创建一个字典来将方向键映射到像`"Up"`和`"Down"`这样的方向。

我还将编写一个函数，如果用户按下一个适当的键，它将设置一个新的方向，但如果这个方向与蛇当前的方向正好相反，则不会设置新的方向。我们不希望蛇能够通过折叠吃掉自己，所以我们将忽略这些按键。

让我们从处理循环中的事件开始。

app.py

```py
`def play_game():
    score = 0

    current_direction = "Right"
    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = set_new_food_position(snake_positions)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return
            elif event.type == pygame.KEYDOWN:
                current_direction = on_key_press(event, current_direction)

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        font = pygame.font.Font(None, 28)
        text = font.render(f"Score: {score}", True, colours.TEXT)
        screen.blit(text, (10, 10))

        pygame.display.update()

        move_snake(snake_positions, current_direction)

        clock.tick(30)

play_game()` 
```

我们在这里需要做的就是在条件语句中添加一个`elif`子句，当我们得到一个`pygame.KEYDOWN`事件时就会触发这个子句。然后我们将调用我们即将创建的`on_key_press`函数，它将处理这个键的实际处理。

然而，在该函数能够完成其工作之前，我们需要创建我们的按键代码到方向的映射，如下所示:

app.py

```py
`KEY_MAP = {
    273: "Up",
    274: "Down",
    275: "Right",
    276: "Left"
}` 
```

你可以通过打印出`event.__dict__`来得到这些数字，看看当你按下不同的方向键时会发生什么。

这样，我们可以创建`on_key_press`函数，如下所示:

app.py

```py
`def on_key_press(event, current_direction):
    key = event.__dict__["key"]
    new_direction = KEY_MAP.get(key)

    all_directions = ("Up", "Down", "Left", "Right")
    opposites = ({"Up", "Down"}, {"Left", "Right"})

    if (new_direction in all_directions
    and {new_direction, current_direction} not in opposites):
        return new_direction

    return current_direction` 
```

首先，我们从`event`中获取实际的关键代码，并查看该代码是否在我们的`KEY_MAP`字典中。如果不是，那么我们将`None`赋给`new_direction`，这意味着我们将从函数中返回当前方向。实际上，什么都不会改变。

如果我们得到了一个有效的方向，我们看看包含当前方向和新方向的集合是否与我们构建的对立集合相匹配。

因为集合不关心顺序，对于满足这个条件来说，重要的是我们有相反的方向。这将导致蛇吃掉自己，所以我们在这种情况下忽略按键，只返回当前方向。

如果我们得到了一个有效的方向，并且新的和旧的方向不是相反的，我们返回新的方向，这个方向在`play_game`中被赋值给 current_direction。

这样，我们只需要做几件事情。首先，我们需要检查与墙壁的碰撞，其次，我们需要检查与食物的碰撞。

这是我如何检查与墙壁的碰撞。

app.py

```py
`def check_collisions(snake_positions):
    head_x_position, head_y_position = snake_positions[0]

    return (
        head_x_position in (-20, WINDOW_WIDTH )
        or head_y_position in (20, WINDOW_HEIGHT)
        or (head_x_position, head_y_position) in snake_positions[1:]
    )` 
```

我在这里使用了一系列的`or`操作符来组合几个条件。因为它们都返回一个布尔值，我们将得到返回的`True`或`False`，其中`True`表示一个条件确实发生了。

当我们撞到任何墙壁时，或者当头部与其他部分占据相同空间时，就会发生这种情况。

我们将在每个 tick 调用一次`play_game`内部的`check_collisions`，如果`check_collisions`返回`True`，我们将从`play_game`返回。这将结束游戏。

app.py

```py
`...

def play_game():
    score = 0

    current_direction = "Right"
    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = set_new_food_position(snake_positions)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return
            elif event.type == pygame.KEYDOWN:
                current_direction = on_key_press(event, current_direction)

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        font = pygame.font.Font(None, 28)
        text = font.render(f"Score: {score}", True, colours.TEXT)
        screen.blit(text, (10, 10))

        pygame.display.update()

        move_snake(snake_positions, current_direction)

        if check_collisions(snake_positions):
            return

        clock.tick(30)

play_game()` 
```

拼图的最后一块检查食物碰撞，这就像检查食物坐标和头段坐标是否相同一样简单。

app.py

```py
`def check_food_collision(snake_positions, food_position):
    if snake_positions[0] == food_position:
        snake_positions.append(snake_positions[-1])

        return True` 
```

如果条件满足，我们将复制尾段，这样蛇在下一次移动时会增长一段。

然后我们返回`True`，因为我们也需要在`play_game`中做一些事情，比如生成一个新的食物位置，并增加`score`。

因此，最终的代码如下所示:

app.py

```py
`import colours
import pygame

from random import randint

WINDOW_HEIGHT = 840
WINDOW_WIDTH = 800
WINDOW_DIMENSIONS = WINDOW_WIDTH, WINDOW_HEIGHT

SEGMENT_SIZE  =  20

KEY_MAP = {
    273: "Up",
    274: "Down",
    275: "Right",
    276: "Left"
}

pygame.init()
pygame.display.set_caption("Snake")

clock = pygame.time.Clock()
screen = pygame.display.set_mode(WINDOW_DIMENSIONS)

def check_collisions(snake_positions):
    head_x_position, head_y_position = snake_positions[0]

    return (
        head_x_position in (-20, WINDOW_WIDTH )
        or head_y_position in (20, WINDOW_HEIGHT)
        or (head_x_position, head_y_position) in snake_positions[1:]
    )

def check_food_collision(snake_positions, food_position):
    if snake_positions[0] == food_position:
        snake_positions.append(snake_positions[-1])

        return True

def draw_objects(snake_positions, food_position):
    pygame.draw.rect(screen, colours.FOOD, [food_position, (SEGMENT_SIZE, SEGMENT_SIZE)])

    for x, y in snake_positions:
        pygame.draw.rect(screen, colours.SNAKE, [x, y, SEGMENT_SIZE, SEGMENT_SIZE])

def move_snake(snake_positions, direction):
    head_x_position, head_y_position = snake_positions[0]

    if direction == "Left":
        new_head_position = (head_x_position - SEGMENT_SIZE, head_y_position)
    elif direction == "Right":
        new_head_position = (head_x_position + SEGMENT_SIZE, head_y_position)
    elif direction == "Down":
        new_head_position = (head_x_position, head_y_position + SEGMENT_SIZE)
    elif direction == "Up":
        new_head_position = (head_x_position, head_y_position - SEGMENT_SIZE)

    snake_positions.insert(0, new_head_position)
    del snake_positions[-1]

def on_key_press(event, current_direction):
    key = event.__dict__["key"]
    new_direction = KEY_MAP.get(key)

    all_directions = ("Up", "Down", "Left", "Right")
    opposites = ({"Up", "Down"}, {"Left", "Right"})

    if (new_direction in all_directions
    and {new_direction, current_direction} not in opposites):
        return new_direction

    return current_direction

def set_new_food_position(snake_positions):
    while True:
        x_position = randint(0, 39) * SEGMENT_SIZE
        y_position = randint(2, 41) * SEGMENT_SIZE
        food_position = (x_position, y_position)

        if food_position not in snake_positions:
            return food_position

def play_game():
    score = 0

    current_direction = "Right"
    snake_positions = [(100, 100), (80, 100), (60, 100)]
    food_position = set_new_food_position(snake_positions)

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                return
            elif event.type == pygame.KEYDOWN:
                current_direction = on_key_press(event, current_direction)

        screen.fill(colours.BACKGROUND)
        draw_objects(snake_positions, food_position)

        font = pygame.font.Font(None, 28)
        text = font.render(f"Score: {score}", True, colours.TEXT)
        screen.blit(text, (10, 10))

        pygame.display.update()

        move_snake(snake_positions, current_direction)

        if check_collisions(snake_positions):
            return

        if check_food_collision(snake_positions, food_position):
            food_position = set_new_food_position(snake_positions)
            score += 1

        clock.tick(30)

play_game()` 
```

然而这还不是结束！我们有很多事情可以做得更好，我鼓励你自己尝试扩展这个项目。

例如，你可以添加一个游戏开始和结束的屏幕。你可以记录高分。你可以添加音乐！可能性是无限的。

我希望你喜欢这个最后的项目，以及整个系列。

祝您未来的项目好运，并祝您编码愉快！