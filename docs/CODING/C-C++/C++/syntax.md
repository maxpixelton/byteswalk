# 语法

## 基本语法


## 注释

程序的注释是解释性语句，我们可以在 C++ 代码中包含注释，这将提高源代码的可读性。

C++ 支持单行注释和多行注释（注释中的所有字符都会被 C++ 编译器忽略）

- `//` - 单行注释
- `/* ... */` - 多行注释

!!! tip

    :boom: 在 `/*` 和 `*/` 注释内部，`//` 字符没有特殊的含义。在 `//` 注释内，`/*` 和 `*/` 字符也没有特殊的含义。因此，我们可以在一种注释内嵌套另一种注释。

## 数据类型

我们在写代码时，需要用到各种变量存储各种信息。变量保留的是它所存储的值的内存位置。这意味着，当我们创建一个变量时，就会在内存中保留一些空间。

我们可能需要存储各种数据类型（如字符型、宽字符型、整型、浮点型、双浮点型、布尔型等）的信息，操作系统会根据变量的数据类型，来分配内存和决定在保留内存总存储什么。

### 基本的数据类型

C++ 提供了 7 中基本的数据类型。

类型           | 关键字  |
------------  | ------------- |
布尔型         | bool         | 
字符型         | char         | 
整型           | int          | 
浮点型         | float        | 
双浮点型       | double       | 
无类型         | void         | 
宽字符型       | wchar_t      |

!!! tip

    `wchar_t` 是这样来的:
    ```C++
    typedef short int wchar_t;
    ```


## 变量类型

## 变量作用域

## 常量

## 修饰符类型

## 存储类

## 运算符

## 循环

## 判断

## 函数

## 数字

## 数组

C++ 支持**数组**数据结构，可以存储一个固定大小的相同类型元素的顺序集合。

数组是用来存储一系列数据，但它往往被认为是一系列相同类型的变量。

数组的声明不是一个个单独的变量，如 nummber0、number1、...、number99，而是声明一个数组变量，比如 numbers，然后使用 numbers[0]、
numbers[1]、...、numbers[99] 来代表一个个单独的变量。数组中的特定元素可以通过索引访问。

所有数组都是由连续的内存位置组成，最低的地址对应第一个元素，最高的地址对应最后一个元素。

### 声明数组

在 C++ 中要声明一个数组，需要指定元素的类型和元素的数量，如下所示：
```
type arrayName [ arraySize ]; 
```

这叫做一维数组。arraySize 必须是一个大于零的整数常量，type 可以是任意有效的 C++ 数据类型。例如，要声明一个类型为 double 的包含 10 个元素的数组 balance，声明语句如下：

```C++
double balance[10]; // 现在 balance 是一个可用的数组，可以容纳 10 个类型为 double 的数字。
```

大括号 {} 之间的值的数目不能大于我们在数组声明时在方括号 [] 中指定的元素数目。

如果省略了数组的大小，数组的大小则为初始化时元素的个数。因此，如果：
```C++
double balance[] 
```

### 初始化数组

## 字符串

C++ 提供了两种类型的字符串表示形式：

- C 风格字符串

- C++ 引入的 string 类类型

### C 风格字符串

C 风格的字符串源于 C 语言，并在 C++ 中继续得到支持：使用 **null** 字符 `\0` 终止的一维字符数组，即一个以 null 结尾的字符串，包含了组成字符串的字符。

下面的声明和初始化创建了一个 **Byteswalk** 字符串。由于在数组的末尾存储了空字符，因此字符数组的大小比单词 **Byteswalk** 的字符数多一个。

```C++
char site[10] = {'B', 'y', 't', 'e', 's', 'w', 'a', 'l', 'k', '\0'};
```

根据数组初始化规则，可以将上面的语句写成以下语句：

```C++
char site[] = "Byteswalk";
```

下面是 C/C++ 中定义的字符串的内存表示：


其实，我们不需要将 null 字符放在字符串常量的末尾。C++ 编译器会在初始化数组时，自动将 `\0` 放在字符串的末尾，

下面尝试输出上面的字符串：
```c++
#include <iostream>
#include <Windows.h>

using namespace std;

int main()
{
    SetConsoleOutputCP(CP_UTF8);

    char site[11] = {'B', 'y', 't', 'e', 's', 'w', 'a', 'l', 'k', 's', '\0'};

    cout << "菜鸟教程：";
    cout << site << endl;

    return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：
```
```

C++ 提供大量的函数用来操作以 null 字符串结尾的字符串：
序号 | 函数  目的 
:----------- |-------------:
1 | strcpy(s1, s2);       
2 | strcat(s1, s2); 
3 | strlen(s1);


序号 | 函数（方法）     | 目的
:--- |:---------------:| -----------:
1    | strcpy(s1, s2); | 复制字符串 s2 到字符串 s1。
2    | strcat(s1, s2); | 连接字符串 s2 到字符串 s1 的末尾。连接字符串也可以用 `+` 号。
3    | strlen(s1);     | 返回字符串 s1 的长度
4    | strcmp(s1, s2); | 如果 s1 和 s2 是相同的，则返回 0；如果 s1 < s2 就返回值小于 0；如果 s1 > s2 则返回值大于 0。
5    | strchr(s1, ch); | 返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。
6    | strstr(s1, s2); | 返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置。

下面的例子使用上述的一些函数：
```c++
#include <iostream>
#include <cstring>
#include <windows.h>

using namespace std;

int main()
{
    char str1[13] = "runoob";
    char str2[13] = "google";
    char str3[13];



    SetConsoleOutputCP(CP_UTF8);

    // 复制 str1 到 str3
    strcpy(str3, str1);
    cout << "strcpy(str3, str1): " << str3 << endl;


    // 连接前，str1 的总长度
    cout << "使用 strlen(str1) 方法前，str1 的总长度：" << strlen(str1) << endl;

    // 连接 str1 和 str2
    strcat(str1, str2);
    cout << "strcat(str1, str2): " << str1 << endl;

    // 连接后，str1 的总长度
    cout << "使用 strlen(str1) 方法后，str1 的总长度：" << strlen(str1) << endl;

    return 0;
    
}
```

当上面的代码被编译和执行时，会产生下列结果：
```
strcpy(str3, str1): Byteswalk
使用 strlen(str1) 方法前，str1 的总长度：9
strcat(str1, str2): Byteswalkgoogle
使用 strlen(str1) 方法后，str1 的总长度：15
```

### C++ 中的 String 类

C++ 标准库提供了 string 类类型，支持上诉所有操作，此外还增加了其它更多的功能。

```C++
#include <iostream>
#include <string>

using namespace std;

int main() 
{
    string str1 = "runoob";
    string str2 = "google";
    string str3;
    int len;

    // 复制 str1 到 str3
    str3 = str1;
    cout << "str3 : " << str3 << endl;

    // 连接 str1 和 str2
    str3 = str1 + str2;
    cout << "str1 + str2 : " << str3 << endl;

    // 连接后，str3 的总长度
    len = str3.size();
    cout << "str3.size() : " << len << endl;

    return 0;
}
```

