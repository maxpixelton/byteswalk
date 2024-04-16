# 列表

## 列表是什么

**列表（list）** 是由一些列按特定顺序排列的元素组成。

我们可以将任何东西加入列表，其中的元素之间可以没有任何关系，例如创建包含字母表中所有字母、数字 0-9，中文...

列表通常包含多个元素，因此在给列表指定一个表示复数的名称（如 letters、digits 或 names ）是个不错的注意。

在 Python 中，使用方括号 `[]` 表示列表，用英文逗号 `,` 分隔其中的元素。

下面是一个简单的示例：
```python
>>> bycycles = ['trek', 'cannondale', 'redline', 'specialized']
>>> print(bycycles)
['trek', 'cannondale', 'redline', 'specialized']
```

如果使用 `Print()` 将列表打印出来，Python 将打印列表的内部表示，包括方括号：
```python
['trek', 'cannondale', 'redline', 'specialized']
```

显然，这不是我们想要让用户看到的输出。

### 访问列表

列表是有序集合，因此访问列表的任何元素，只需要将该元素的位置（索引）告诉 Python 即可，即首先指出列表的名称，再指出元素的索引，并将后者放在方括号内，如下：

```python
>>> bicycles = ['trek', 'cannondale', 'redline', 'specialized']
>>> print(bicycles[0])
trek
```

这才是我们想让用户看到的结果——整洁、干净的输出。

!!! note

    在 Python 中，第一个列表元素的索引为 0，而不是 1。大多数编程语言是如此规定的，这与列表操作的底层实现有关！！！


### 使用列表中的各个值

我们可以像使用其它变量一样使用列表中的各个值。例如：可以使用 f 字符串根据列表中的值来创建消息。如下：

```python
>>> bicycles = ['trek', 'cannondale', 'redline', 'specialized']
>>> mix = [1, 'hello']
>>> message = f"My first bicycle was a {bicycles[0].title()}"
>>> print(message)
My first bicycle was a Trek
>>> info = f"{mix[1].title()} {mix[0]}"
>>> print(info)
Hello 1
```

## 列表中修改、、删除元素

我们创建的大多数列表是动态的，这意味着列表创建后，将随着程序的运行添加、删除或修改元素。

例如，我们可能创建了一个游戏，要求玩家消灭从天而降的外星人，为此，可在开始时将一些外星人存储在列表中，然后每当有外星人被消灭时，都将其从列表中删除，而每次有新的外星人出现在屏幕上时，都将其添加到列表中。在整个游戏运行期间，外星人列表的长度将不断变化。

### 修改元素

修改列表的元素的语法与访问列表元素的语法相似。

当我们想要修改列表元素时，需要指定列表名和要修改的元素的索引，再指定该元素的新值，假设：

假设有一个摩托车列表，其中第一个元素为 `'honda'`，那么可在创建列表后修改这个元素的值：

```python
>>> motorcycles = ['honda', 'yamaha', 'suzuki']
>>> print(motorcycles)
['honda', 'yamaha', 'suzuki']
>>> motorcycles[0] = 'ducati'    # 将列表第一个元素 'honda' 修改为 'ducati'
>>> print(motorcycles)
['ducati', 'yamaha', 'suzuki']
```

当然我们可以修改任意列表元素的值，而不只是第一个元素的值。

### 添加元素

我们可能会出于很多原因在列表中添加新元素。

比如，我们可能希望游戏中出现新的外星人，添加可视化数据，或者给网站添加新注册的用户。

Python 提供了多种在既有列表中添加数据的方式

1. **在列表末尾添加元素**

    在列表中添加新元素时，最简单的方式是将元素 **追加（append）**到列表末尾。
    
    比如，向上面示例中的列表末尾添加新元素 'ducati'

    ```python
    >>> motorcycles = ['honda', 'yamaha', 'suzuki']
    >>> print(motorcycles)
    ['honda', 'yamaha', 'suzuki']
    >>> motorcycles.append('ducati')   # 使用 append() 方法将元素 'ducati' 添加到列表末尾
    >>> print(motorcycles)
    ['honda', 'yamaha', 'suzuki', 'ducati']
    ```
    
    使用 `append()` 方法将元素 'ducati' 添加到列表末尾，不会影响列表中的其它所有元素。

    `append()` 方法让动态地创建列表易如反掌。例如，你可以先创建一个空列表，再使用一系列函数调用 append() 添加元素。下面来创建一个空列表，再在其中添加元素 'honda'、'yamaha' 和 'suzuki'：

    ```python
    >>> motorcycles = []
    >>> motorcycles.append('honda')
    >>> motorcycles.append('yamaha')
    >>> motorcycles.append('suzuki')
    >>> print(motorcycles)
    ['honda', 'yamaha', 'suzuki']
    ```

    这种创建列表的方式极其常见，因为经常要等程序运行后，我们才知道用户要在程序中存储哪些数据。为了便于管理，可首先创建一个空列表，用于存储用户将要输入的值，然后将用户提供的每个新值追加到列表末尾。
    
2. **在列表中插入元素**

    使用 `insert()` 方法可在列表的任意位置添加新元素。为此，需要指定新元素的索引和值：

    ```java
    >>> motorcycles.insert(0, 'ducati')
    >>> print(motorcycles)
    ['ducati', 'honda', 'yamaha', 'suzuki']
    ```

    在这个示例中，值 'ducati' 被插入到了列表开头，并将值 'ducati' 存储到这个地方。

    **这种操作将列表中的每个既有元素都右移一个位置。**

### 删除元素

我们也经常需要从列表中删除一个或多个元素。例如，玩家将一个外星人消灭后，我们很可能要将其从存活的外星人列表中删除；当用户在我们创建的 Web 应用程序中注销账户时，我们需要将该用户从活动用户列表中删除。你可以根据位置或值来删除列表中的元素。

1. **使用 del 语句删除元素**

    如果知道要删除的元素在列表中的位置，可使用 del 语句：

    ```python title="使用 del 语句删除元素"
    >>> motorcycles = ['honda', 'yamaha', 'suzuki']
    >>> print(motorcycles)
    ['honda', 'yamaha', 'suzuki']
    >>> del motorcycles[0]   # 使用 del 语句删除元素         
    >>> print(motorcycles)
    ['yamaha', 'suzuki']
    ```

    使用 del 可删除任意位置的列表元素，只需要知道其索引即可。

2. **使用 pop() 方法删除元素**

    有时候，我们要将元素从列表中删除，并接着使用它的值。例如，我们可能要获取刚被消灭的外星人的 x 坐标和 y 坐标，以便在相应的位置显示爆炸效果；在 Web 应用程序中，我们可能要将用户从活动成员列表中删除，并将其加入非活动成员列表。

    `pop()` 方法删除列表末尾的元素，并让你能够接着使用它。术语弹出（pop）源自这样的类比：<font color=red>列表就像一个栈，而删除列表末尾的元素相当于弹出栈顶元素。</font>

    下面来从列表 motorcycles 中弹出一款摩托车：

    ```python title="使用 pop() 方法删除元素"
    >>> motorcycles = ['honda', 'yamaha', 'suzuki'] # 首先定义并打印了列表 motorcycles
    >>> print(motorcycles)
    ['honda', 'yamaha', 'suzuki']

    >>> popped_motorcycle = motorcycles.pop() # 从列表中弹出一个值，并将其赋值给变量 popped_motorcycle
    >>> print(motorcycles) # 然后打印这个列表，以核实从中删除了一个值                   
    ['honda', 'yamaha']
    >>> print(popped_motorcycle)  # 最后打印弹出的值，以证明依然能够访问被删除的值
    suzuki # 输出表明，列表末尾的值 'suzuki' 已删除，被赋给了变量 popped_motorcycle
    ```

    方法 `pop()` 有什么用处呢？假设列表中的自行车是按购买时间存储的，就可使用方法 pop() 来打印一条消息，指出最后购买的是哪款自行车：
    
    ```python
    >>> motorcycles = ['honda', 'yamaha', 'suzuki']
    >>> last_owned = motorcycles.pop()
    >>> print(f"The last motorcycle I owned was a {last_owned.title()}.")
    The last motorcycle I owned was a Suzuki.
    ```

3. **删除列表任意位置的元素**

    我们可以使用 `pop()` 删除列表中任意位置的元素，只需要在括号中指定要删除的元素的索引即可。

    ```python
    >>> motorcycles = ['honda', 'yamaha', 'suzuki']
    >>> first_owned = motorcycles.pop(0)
    >>> print(f"The first motorcycle I owned was a {first_owned.title()}.")
    The first motorcycle I owned was a Honda.
    ```

    别忘了，每当你使用 `pop()` 时，被弹出的元素就不再在列表中了。

    !!! notes 
    
        如果不确定该使用 `del` 语句还是 `pop()` 方法，一个简单的判断标准：
        
        - 如果要从列表中删除一个元素，且不再以任何方式使用它，就使用 `del` 语句。
        
        - 如果要在删除元素后继续使用它，就使用 `pop()` 方法。

4. 根据值删除元素

    有时候，我们不知道要从列表中删除的值在哪个位置。如果只知道要删除的元素的值，可使用 `remove()` 方法。

    假设要从列表 motorcycles 中删除值 'ducati', 

    ```python title="根据值删除元素"
    >>> motorcycles.remove('ducati')
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    ValueError: list.remove(x): x not in list
    >>> motorcycles = ['honda', 'yamaha', 'suzuki']
    >>> motorcycles.remove('honda')
    >>> print(motorcycles)
    ['yamaha', 'suzuki']
    ```

    `remove()` 方法让 Python 确定 'ducati' 出现在列表的什么地方，并将该元素删除：

    使用 `remove()` 从列表中删除元素后，也可继续使用它的值。下面删除值 'ducati' 并打印一条消息，指出将其从列表中删除的原因：

    ```python
    >>> motorcycles = ['honda', 'yamaha', 'suzuki', 'ducati']
    >>> too_expensive = 'ducati'
    >>> motorcycles = ['honda', 'yamaha', 'suzuki', 'ducati']
    >>> too_expensive = motorcycles[-1]
    >>> print(too_expensive)
    ducati

    >>> motorcycles.remove(too_expensive)
    >>> print(motorcycles)
    ['honda', 'yamaha', 'suzuki']

    >>> print(f"\n A {too_expensive.title()} is too expensive for me.")

    A Ducati is too expensive for me.
    >>> print(too_expensive)
    ducati
    ```

    !!! notes

        `remove()` 方法只删除第一个指定的值。如果要删除的值可能在列表中出现多次，就需要使用循环，确保将每个值都删除。



## 管理列表

在我们创建的列表中，元素的排列顺序常常是无法预测的，因为我们并非总能控制用户提供数据的顺序。这虽然在大多数情况下是不可避免的，但我们经常需要以特定的顺序呈现信息。我们有时候希望保留列表元素最初的排列顺序，而有时候又需要调整排列顺序。Python 提供了很多管理列表的方式，可根据具体情况选用。

### 使用 sort() 方法对列表进行永久排序

Python 方法 sort() 让你能够较为轻松地对列表进行排序。假设你有一个汽车列表，并要让其中的汽车按字母顺序排列。为了简化这项任务，假设该列表中的所有值都是全小写的。

```Python
>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
>>> cars.sort()
>>> print(cars)
['audi', 'bmw', 'subaru', 'toyota']
```

sort() 方法能永久地修改列表元素的排列顺序。现在，汽车是按字母顺序排列的，再也无法恢复到原来的排列顺序：

还可以按与字母顺序相反的顺序排列列表元素，只需向 sort() 方法传递参数 reverse=True 即可。下面的示例将汽车列表按与字母顺序相反的顺序排列：
 
```python
>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
>>> cars.sort(reverse=True)
>>> print(cars)
['toyota', 'subaru', 'bmw', 'audi'] #
```
同样，对列表元素排列顺序的修改也是永久的。

### 使用 sorted() 函数对列表进行临时排序

要保留列表元素原来的排列顺序，并以特定的顺序呈现它们，可使用 sorted() 函数。sorted() 函数让你能够按特定顺序显示列表元素，同时不影响它们在列表中的排列顺序。

下面尝试对汽车列表调用这个函数。

```python
>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
>>> print(f"Here is the original list:\n - {cars}")
Here is the original list:
 - ['bmw', 'audi', 'toyota', 'subaru']

>>> print(f"Here is the sorted list:\n - {sorted(cars)}")
Here is the sorted list:
 - ['audi', 'bmw', 'subaru', 'toyota']

>>> print(f"Here is the original list again:\n - {cars}")
Here is the original list again:
 - ['bmw', 'audi', 'toyota', 'subaru']
```

!!! notes ""

    - 在调用 sorted() 函数后，列表元素的排列顺序并没有变（见❶）。如果要按与字母顺序相反的顺序显示列表，也可向 sorted() 函数传递参数 reverse=True。

    - 在并非所有的值都是全小写的时，按字母顺序排列列表要复杂一些。在确定排列顺序时，有多种解读大写字母的方式，此时要指定准确的排列顺序，可能会比这里所做的更加复杂。然而，大多数排序方式是以此为基础的。

### 反向打印列表

反转列表元素的排列顺序，可使用 `reverse()` 方法。假设汽车列表是按购买时间排列的，可轻松地按相反的顺序排列其中的汽车：

```python 
>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
>>> print(cars)
['bmw', 'audi', 'toyota', 'subaru']
>>> cars.reverse()
>>> print(cars)
['subaru', 'toyota', 'audi', 'bmw']
```

!!! notes

    - `reverse()` 不是按与字母顺序相反的顺序排列列表元素，只是反转列表元素的排列顺序。

    - `reverse()` 方法会永久地修改列表元素的排列顺序，但可随时恢复到原来的排列顺序，只需对列表再次调用 `reverse()` 即可。

### 确定列表的长度

使用 `len()` 函数可快速获悉列表的长度。在下面的示例中，列表包含 4 个元素，因此其长度为 4：

```python title="确定列表的长度"
>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
>>> len(cars)
4
```
在需要完成如下任务时，`len()` 很有用：明确还有多少个外星人未被消灭，确定需要管理多少项可视化数据，计算网站有多少注册用户，等等。

!!! notes

    Python 在计算列表元素数时从 1 开始，因此你在确定列表长度时应该不会遇到差一错误。

## 使用列表时避免索引错误

我们在刚开始使用列表时，很容易犯一种错误。假设我们有一个包含三个元素的列表，却要求获取第四个元素：

```python
>>> motorcycles = ['honda', 'yamaha', 'suzuki']
print(motorcycles[3])
```

这将导致 **索引错误**：

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

Python 试图提供位于索引 3 处的元素，但当它搜索列表 motorcycles 时，却发现索引 3 处没有元素。鉴于列表索引差一的特征，这种错误很常见。有些人从 1 开始数，因此以为第三个元素的索引为 3。但是在 Python 中，第三个元素的索引为 2，因为索引是从 0 开始的。

索引错误意味着 Python 在指定索引处找不到元素。在程序发生索引错误时，请尝试将指定的索引减 1，然后再次运行程序，看看结果是否正确。

别忘了，每当需要访问最后一个列表元素时，都可以使用索引 -1。这在任何情况下都行之有效，即便在你最后一次访问列表后，其长度发生了变化：

```python
>>> motorcycles = ['honda', 'yamaha', 'suzuki']
>>> print(motorcycles[-1])
ducati
```

索引 -1 总是返回最后一个列表元素，这里为值 'suzuki'。仅当列表为空时，这种访问最后一个元素的方式才会导致错误：

```
>>> print(motorcycles[-1])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

列表 motorcycles 不包含任何元素，因此 Python 返回一条索引错误消息：

!!! notes 

    在发生索引错误却找不到解决办法时，请尝试将列表或其长度打印出来。列表可能与我们以为的截然不同，在程序对其进行了动态处理时尤其如此。查看列表或其包含的元素数，可帮助我们排查这种逻辑错误。