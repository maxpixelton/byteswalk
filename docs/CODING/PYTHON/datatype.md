# 数据类型

## 内置数据类型

变量可以存储不同类型的数据，并且不同类型可以执行不同的操作。

在这些类别中，Python 默认拥有以下内置数据类型：

| 类别    | 类型                 |
| ------------  | ------------------------------- |
| 文本（字符串）| `str`                    |
| 数值     | `int` `float` `complex`          |
| 序列     | `list` `tuple` `range`           |
| 映射     | `dict`                           |
| 集合     | `set` `frozenset`                |
| 布尔     | `bool`                           |
| 二进制   | `bytes` `bytearray` `memoryview` |


我们可以使用函数/方法 `type()` 获取或验证任何对象的数据类型。

在 Python 中，当我们为变量赋值时，即可以显示设定特定的数据类型，也可以隐式（不设置）设定特定的数据类型，示例如下：


数据类型       | 示例 1（隐式）                                  | 示例 2（显示）
:-----------  |:---------------------------------------------- |:-----------------------------------------------
`str`         | `x = "Hello World"`                            | `x = str("Hello World")`
`int`         | `x = 29`                                       | `x = int(29)`
`float`       | `x = 29.5`                                     | `x = float(29.5)`
`complex`     | `x = 1j`                                       | `x = complex(1j)`
`list`        | `x = ["apple", "banana", "cherry"]`            | `x = list(("apple", "banana", "cherry"))`
`tuple`       | `x = ("apple", "banana", "cherry")`            | `x = tuple(("apple", "banana", "cherry"))`
`range`       | `x = range(6)`                                 | `x = range(6)`
`dict`        | `x = {"name" : "Bill", "age" : 63}`            | `x = dict(name="Bill", age=36)`
`set`         | `x = {"apple", "banana", "cherry"}`            | `x = set(("apple", "banana", "cherry"))`
`frozenset`   | `x = frozenset({"apple", "banana", "cherry"})` | `x = frozenset(("apple", "banana", "cherry"))`
`bool`        | `x = True`                                     | `x = bool(5)`
`bytes`       | `x = b"Hello"`                                 | `x = bytes(5)`
`bytearray`   | `x = bytearray(5)`                             | `x = bytearray(5)`
`memoryview`  | `x = memoryview(bytes(5))`                     | `x = memoryview(bytes(5))`


## 数字（数值）

Python 中有三种数字类型：

- `int`。（整数，正数或负数，没有小数，长度不限）

- `float`。浮点数（包含小数的正数或负数），可以是带有 $e$ 的科学数字，表示 10 的幂，如 $-49.8e100$

- `complex`。复数，用 "j" 作为虚部编写：

```python
# int 整数
x1 = 10   
x2 = 37216654545182186317
x3 = -465167846
print(type(x1))
print(type(x2))
print(type(x3))

# float 浮点数
y1 = 3.50 
y2 = 27e4
y3 = -63.78
y4 = -49.8e100
print(type(y1))
print(type(y2))
print(type(y3))
print(type(y4))

# complex 复数
z1 = 2+3j
z2 = 7j
z3 = -7j  
print(type(z1))
print(type(z2))
print(type(z3))
```

### 随机数

Python 有一个名为 <font color=red>random</font>的内置模块，可用于生成随机数：

```python
import random

print(random.randrange(1,10))
```

详见[这里](./random-detail.md)了解有关 Random 模块的更多信息。




### 字符串

- python 中的，单行字符串字面量由<font color=red>单引号</font>或<font color=red>双引号</font>括起，即 `'hello'` 等同于 `"hello"`，


字符串内建方法参见[这里](./string-method.md)！

## 类型转换 Casting

有时我们可能需要为变量指定类型。这可以通过 casting 来完成。 Python 是一门面向对象的语言，因此它使用类来定义数据类型，包括其原始类型。

我们可以使用以下函数（方法）将一种类型转换为另一种类型，特别的，<font color=red>我们无法将复数转换为其他数字类型。</font>

- `int()`：作用于整数字面量、浮点数字面量（通常对数进行 **下舍入**），或者用表示完整数字的字符串字面量。

- `float()`：作用于整数字面量、浮点字面量，或字符串字面量构造浮点数（提供表示浮点数或整数的字符串）

- `complex()`：作用于整数字面量、浮点字面量，或字符串字面量构造浮点数（提供表示浮点数或整数的字符串）

- `str()`：作用于各种数据类型构造字符串，包括字符串，整数字面量和浮点字面量

```Python
x = 10 # int
y = 6.3 # float
z = 1j # complex

# 把整数转换为浮点数

a = float(x)

# 把浮点数转换为整数

b = int(y)

# 把整数转换为复数：

c = complex(x)

print(a)
print(b)
print(c)

print(type(a))
print(type(b))
print(type(c))

x1 = str("S2") # x 将是 'S2'
y1 = str(3)    # y 将是 '3'
z1 = str(4.0)  # z 将是 '4.0'
```

