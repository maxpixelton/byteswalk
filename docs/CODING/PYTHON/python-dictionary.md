# 字典

## 一个简单的字典

假设现在要做一个包含外星人的游戏，这些外星人的颜色和分数各不相同。

下面是一个简单的字典，存储了有关特定外星人的信息：

```python
alien_0 = {
    'color': 'green',
    'points': 5
}

print(alien_0['color']) # 输出 green
print(alien_0['points']) # 输出 5
```

## 使用字典

在 Python 中，**字典（dictionary）** 是一系列 **键值对**。

每个 **键** 都与一个 **值** 关联，可以使用 **键** 来访问与之关联的 **值**。

与键相关联的值可以是数、字符串、列表、字典...事实上，可将任意 Python 对象用作字典中的值。

在 Python 中，字典用放在 **花括号 `{}`** 中的一系列键值对表示，如：`alien_0 = { 'color': 'green', 'points': 5 }`。

**键值对** 包含两个相互关联的值。当我们指定 **键** 时，Python 将返回与之关联的值。键和值之间用 **冒号 `:`** 分隔，而键值对之间用 **逗号 `,`** 分隔。在字典中，我们想存储多少个键值对都可以。

最简单的字典只有一个键值对，如修改后的字典 `alien_0` 所示：`alien_0 = { 'color': 'green'}`，这个字典只存储了一项有关 `alien_0` 的信息，具体地说是这个外星人的颜色。在这个字典中，字符串 `color` 是一个键，与之关联的值为 `green`。

### 访问字典中的值

要获取与键关联的值，可指定字典名并把键放在后面的方括号内，如下所示：

```python
alien_0 = { 'color': 'green' }

print(alien_0['color']) # green
```

### 添加键值对

字典是一种动态结构，可以随时在其中添加键值对。要添加键值对，可依次指定字典名、用方括号括起来的键和与该键关联的值。

接下来在字典 `alien_0` 中添加两项信息：外星人的 x 坐标和 y 坐标，以便在屏幕的特定位置上显示该外星人。
我们将这个外星人放在屏幕左边缘上，距离屏幕上边缘 25 像素。由于屏幕坐标系的原点通常在左上角，因此要将该外星人放在屏幕左边缘，可将 x 坐标设置为 0；要将该外星人放在距离屏幕上边缘 25 像素的地方，可将 y 坐标设置为 25，如下所示：

```python
alien_0 = { 
    'color': 'green', 
    'points': 5 
}

print(alien_0) # {'color': 'green', 'points': 5}

alien_0['x_position'] = 0    # 添加外星人的 x 坐标
alien_0['y_position'] = 25   # 添加外形人的 y 坐标

print(alien_0) # {'color': 'green', 'points': 5, 'x_position': 0, 'y_position': 26}

```

字典会保留定义时的元素排列顺序。如果将字典打印出来或遍历其元素，将发现元素的排列顺序与其添加顺序相同。

### 从创建一个空字典开始

大多时候，在空字典中添加键值对很方便，甚至是必须的。

为此，我们可以先用一对空花括号定义一个空字典，再分行添加各个键值对。

例如，下面演示如何以这种方式创建字典 `alien_0`:

```python
# 首先，定义空字典 alien_0
alien_0 = {}

# 然后，再在其中添加颜色和分数
alien_0['color'] = 'green'
alien_1['points'] = 5

print(alien_0) # {'color': 'green', 'points': 5}
```

当我们要使用字典来存储用户提供的数据或者编写能自动生成大量键值对的代码，通常需要先定义一个空字典。

#### 修改字典中的值

要修改字典中的值，可依次指定字典名、用方括号括起来的键和与该键关联的新值。假设随着游戏的进行，需要将一个外星人从绿色改为黄色：

```python
# 首先定义一个表示外星人 alien_0 的字典，其中只包含这个外星人的颜色
alien_0 = { 'color': 'green' }
print(f"The alien is {alien_0['color']}.") # 输出：The alien is green.

# 接下来，将与键 'color' 关联的值改为 'yellow'。输出表明，这个外星人确实从绿色变成了黄色
alien_0['color'] = 'yellow'
print(f"The alien is now {alien_0['color']}.") # 输出：The alien is now yellow. 
```

再看一个更有趣的例子：对一个能够以不同速度移动的外星人进行位置跟踪。为此，存储该外星人的当前速度，并据此确定该外星人应该向右移动多远：

```python
# 首先，定义一个外星人，其中包含初始 x 坐标和 y 坐标，还有速度 'medium'
# 出于简化考虑，这里省略了颜色和分数，但即便包含这些键值对，这个示例的工作原理也不会有任何变化。
alien_0 = { 
    'x_position': 0, 
    'y_position': 25,
    'speed': 'a'
}
# 打印了 x_position 的初始值，旨在让用户知道这个外星人向右移动了多远。
print(f"Original position: {alien_0['x_position']}")

# 向右移动外星人
# 根据当前速度确定外星人向右移动多远
# 使用了一个 if-elif-else 语句来确定外星人应该向右移动多远，并将这个值赋给变量 x_increment。
# - 如果外星人的速度为 'slow'，它将向右移动 1 个单位；
# - 如果速度为 'medium'，将向右移动 2 个单位；
# - 如果速度更快，将向右移动 3 个单位
if alien_0['speed'] == 'slow':
    x_increment = 1
elif alien_0['speed'] == 'medium':
    x_increment = 2
else:
    # 这个外星人的移动速度肯定很快
    x_increment = 3

# 新位置为旧位置加上移动距离
# 确定移动量后，将其与 x_position 的当前值相加，再将结果关联到字典中的键 x_position。
alien_0['x_position'] = alien_0['x_position'] + x_increment

print(f"New position: {alien_0['x_position']}")
```

### 删除键值对

对于字典中不再需要的信息，可使用 `del` 语句将相应的键值对彻底删除。

在使用 del 语句时，必须指定字典名和要删除的键。

例如，下面代码从字典 `alien_0` 中删除键 `points` 以及其值：

```python
alien_0 = { 'color': 'green', 'points': 5 }

print(alien_0) # 输出：{'color': 'green', 'points': 5}

del alien_0['points'] # del 语句让 Python 将键 'points' 从字典 alien_0 中删除，同时删除与这个键关联的值。输出表明，键 'points' 及其值 5 已被从字典中删除，但其他键值对未受影响

print(alien_0) # {'color': 'green', 'x_position': 0, 'y_position': 26}
```

注意：删除的键值对永远消失了

### 由类似的对象组成的字典


在上文中，字典存储的是一个对象（游戏中的一个外星人）的多种信息，但也可以使用字典来存储众多对象的同一种信息。

假设我们要调查很多人，询问他们喜欢的编程语言，可使用一个字典来存储这种简单调查的结果，如下所示：

```python
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}
```

如上所示，我们将一个较大的字典放在了多行中。每个键都是一个被调查者的名字，而每个值都是被调查者喜欢的语言。当确定需要使用多行来定义字典时，先在输入左花括号后按回车键，再在下一行缩进 4 个空格，指定第一个键值对，并在它后面加上一个逗号。此后再按回车键，文本编辑器将自动缩进后续键值对，且缩进量与第一个键值对相同。

定义好字典后，在最后一个键值对的下一行添加一个右花括号，并且也缩进 4 个空格，使其与字典中的键对齐。一种不错的做法是，在最后一个键值对后面也加上逗号，为以后添加键值对做好准备。

!!! notes

    对于较长的列表和字典，大多数编辑器提供了以类似方式设置格式的功能。对于较长的字典，还有其他一些可行的格式设置方式，因此在你的编辑器或其他源代码中，你可能会看到稍微不同的格式设置方式。

给定被调查者的名字，可使用这个字典轻松地了解他喜欢的语言：

```python
favorite_languages = {
    'jen': 'python',
    'sarah': 'c',
    'edward': 'rust',
    'phil': 'python',
}

language = favorite_languages['sarah'].title() # 这种语法可用来从字典中获取任何人喜欢的语言。
print(f"Sarah's favorite language is {language}.")
```

#### 使用 `get()` 来访问值

使用放在方括号内的键从字典中获取感兴趣的值，可能会引发问题：如果指定的键不存在，就将出错。

如果你要求获取外星人的分数，而这个外星人没有分数，结果将如何呢？下面来看一看：

```python
alien_0 = {
    'color': 'green',
    'speed': 'slow'
}

print(alien_0['points'])
```

这将导致 Python 显示 traceback，指出存在键值错误（KeyError）：

```python
Traceback (most recent call last):
  File "D:\byteswalk\maxpixeltonpro\python-snail\quick-start-python\unit06\alien_no_points.py", line 6, in <module>
    print(alien_0['points'])
          ~~~~~~~^^^^^^^^^^
KeyError: 'points'
```

关于这类错误的处理以后再说。

就字典而言，为避免出现这样的错误，可使用 `get()` 方法在指定的键不存在时返回一个默认值。`get()` 方法的第一个参数用于指定键，是必不可少的；第二个参数为当指定的键不存在时要返回的值，是可选的：

```python
alien_0 = {
    'color': 'green',
    'speed': 'slow'
}

# 如果字典中有键 'points'，将获得与之关联的值；
# 如果没有，将获得指定的默认值。
point_value = alien_0.get('points', 'No point value assigned.')

# 虽然这里没有键 'points'，但是我们将获得一条清晰的消息，不会引发错误：
print(point_value) # 输出：No point value assigned.
```

如果指定的键有可能不存在，应考虑使用 `get()` 方法，而不要使用方括号表示法。


!!! notes

    在调用 `get()` 时，如果没有指定第二个参数且指定的键不存在，Python 将返回值 None，这个特殊的值表示没有相应的值。这并非错误，None 只是一个表示所需值不存在的特殊值

