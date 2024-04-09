# 字符串（文本）的方法

Python 中一组可以在字符串上使用的内建方法。

实现了所有[一般]()序列的操作，并额外提供了以下列出的一些附加方法。

字符串支持两种字符串格式化样式：

- 一种具有很大灵活性和可定制性（参见[format()]()）

!!! note

    所有字符串方法都返回新值，它们不会改变原始字符串。

## 简介
方法 | 描述 |
------------ | ------------- |
`capitalize()`   | 把首字符转换为大写。  |
`casefold()`     | 把字符串转换为小写。|
`center()`       | 返回居中的字符串。|
`count()`        | 返回指定值在字符串中出现的次数。|
`encode()`       | 返回字符串的编码版本。|
`endswith()`     | 如果字符串以指定值结尾，则返回 true。|
`expandtabs()`   | 设置字符串的 tab 尺寸。  |
`find()`         | 在字符串中搜索指定的值并返回它被找到的位置。|
`format()`       | 格式化字符串中的指定值。|
`format_map()`   | 格式化字符串中的指定值。|
`index()`        | 在字符串中搜索指定的值并返回它被找到的位置。|
`isalnum()`      | 如果字符串中的所有字符都是字母数字，则返回 True。|
`isalpha()`      | 如果字符串中的所有字符都在字母表中，则返回 True。|
`isdecimal()`    | 如果字符串中的所有字符都是小数，则返回 True。|
`isdigit()`      | 如果字符串中的所有字符都是数字，则返回 True。|
`isidentifier()` | 如果字符串是标识符，则返回 True。|
`islower()`      | 如果字符串中的所有字符都是小写，则返回 True。|
`isnumeric()`    | 如果字符串中的所有字符都是数，则返回 True。 |
`isprintable()`  | 如果字符串中的所有字符都是可打印的，则返回 True。|
`isspace()`      | 如果字符串中的所有字符都是空白字符，则返回 True。|
`istitle()`      | 如果字符串遵循标题规则，则返回 True。|
`isupper()`      | 如果字符串中的所有字符都是大写，则返回 True。|
`join()`         | 把可迭代对象的元素连接到字符串的末尾。 |
`ljust()`        | 返回字符串的左对齐版本。|
`lower()`        | 把字符串转换为小写。|
`lstrip()`       | 返回字符串的左修剪版本。|
`maketrans()`    | 返回在转换中使用的转换表。|
`partition()`    | 返回元组，其中的字符串被分为三部分。|
`replace()`      | 返回字符串，其中指定的值被替换为指定的值。|
`rfind()`        | 在字符串中搜索指定的值，并返回它被找到的最后位置。|
`rindex()`       | 在字符串中搜索指定的值，并返回它被找到的最后位置。|
`rjust()`        | 返回字符串的右对齐版本。|
`rpartition()`   | 返回元组，其中字符串分为三部分。|
`rsplit()`       | 在指定的分隔符处拆分字符串，并返回列表。|
`rstrip()`       | 返回字符串的右边修剪版本。|
`split()`        | 在指定的分隔符处拆分字符串，并返回列表。|
`splitlines()`   | 在换行符处拆分字符串并返回列表。|
`startswith()`   | 如果以指定值开头的字符串，则返回 true。|
`strip()`        | 返回字符串的剪裁版本。|
`swapcase()`     | 切换大小写，小写成为大写，反之亦然。|
`title()`        | 把每个单词的首字符转换为大写。|
`translate()`    | 返回被转换的字符串。|
`upper()`        | 把字符串转换为大写。|
`zfill()`        | 在字符串的开头填充指定数量的 0 值 |


## `capitalize()`

### 示例

```python
>>> txt = "hello, and welcome to my world."
>>> x = txt.capitalize()
>>> print(x)
Hello, and welcome to my world.
```

### 定义和用法

返回原字符串的副本，其首个字符大写，其余为小写。

在 3.8 版本发生变更：第一个字符现在被放入了 titlecase 而不是 uppercase。这意味着复合字母类字符将只有首个字母改为大写，而不再是全部字符大写。

### 语法

```python
string.capitalize()
```

## `casefold()`

### 示例
```python
>>> txt = "Hello, And Welcome To My World!"
>>> x = txt.casefold()
>>> print(x)
hello, and welcome to my world!
```

### 定义和用法

返回原字符串消除大小写的副本。 消除大小写的字符串可用于忽略大小写的匹配。

消除大小写类似于转为小写，但是更加彻底一些，因为它会移除字符串中的所有大小写变化形式。 例如，德语小写字母 'ß' 相当于 "ss"。 由于它已经是小写了，[`lower()`](#stringlower) 不会对 'ß' 做任何改变；而 casefold() 则会将其转换为 "ss"。

大小写折叠算法参见 [在 Unicode 标准 3.13 节 'Default Case Folding' 中描述](https://www.unicode.org/versions/Unicode15.0.0/ch03.pdf)。

此方法与 [`lower()`](#stringlower) 方法相似，但是 `casefold()` 方法更强大，更具攻击性，这意味着它将更多字符转换为小写字母，并且在比较两个用 `casefold()` 方法转换的字符串时会找到更多匹配项。

在 3.3 版本加入。

### 语法

```python
string.casefold()
```

## `center()`

### 示例

```python
>>> txt = "banana"
>>> len(txt)
6
>>> x = txt.center(20)
>>> print(x)
       banana
>>> x = txt.center(20, '*')
>>> print(x)
*******banana*******
```

### 定义和用法

返回长度为 width 的字符串，原字符串在其正中。 使用指定的 fillchar 填充两边的空位（默认使用 ASCII 空格符），即使用指定的字符（默认为空格）作为填充字符使字符串居中对齐。。 如果 width 小于等于 len(s) 则返回原字符串的副本。 

### 语法

```python
string.center(width[, fillchar])
```

#### 参数

| 参数 | 描述 |
| ------------ | ------------- |
| length | 必需。所返回字符串的长度。  |
| character | 可选。填补两侧缺失空间的字符。默认是 " "（空格）。  |

## `string.lower()`