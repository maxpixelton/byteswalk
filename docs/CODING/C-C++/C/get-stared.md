# 开始

## 简介

C 语言是一种通用的高级语言，最初是由丹尼斯·里奇在贝尔实验室为开发 UNIX 操作系统而设计的。C 语言最开始是于 1972 年在 DEC PDP-11 计算机上被首次实现。

在 1978 年，布莱恩·柯林汉（Brian Kernighan）和丹尼斯·里奇（Dennis Ritchie）制作了 C 的第一个公开可用的描述，现在被称为 K&R 标准。

UNIX 操作系统，C编译器，和几乎所有的 UNIX 应用程序都是用 C 语言编写的。由于各种原因，C 语言现在已经成为一种广泛使用的专业语言。

- 易于学习。
- 结构化语言。
- 它产生高效率的程序。
- 它可以处理底层的活动。
- 它可以在多种计算机平台上编译。

当前最新的 C 语言标准为 C18 ，在它之前的 C 语言标准有 C17、C11...C99 等。

### 关于 C

- C 语言是为了编写 UNIX 操作系统而被发明的。
- C 语言是以 B 语言为基础的，B 语言大概是在 1970 年被引进的。
- C 语言标准是于 1988 年由美国国家标准协会（ANSI，全称 American National Standard Institute）制定的。
- 截至 1973 年，UNIX 操作系统完全使用 C 语言编写。
- 目前，C 语言是最广泛使用的系统程序设计语言。
- 大多数先进的软件都是使用 C 语言实现的。
- 当今最流行的 Linux 操作系统和 RDBMS（Relational Database Management System：关系数据库管理系统） MySQL 都是使用 C 语言编写的。

### 为什么要使用 C ?

C 语言最初是用于系统开发工作，特别是组成操作系统的程序。由于 C 语言所产生的代码运行速度与汇编语言编写的代码运行速度几乎一样，所以采用 C 语言作为系统开发语言。

下面列是使用 C 的实例：

- 操作系统
- 语言编译器
- 汇编器
- 文本编辑器
- 打印机
- 网络驱动器
- 现代程序
- 数据库
- 语言解释器
- 实体工具

### C 程序

一个 C 语言程序，可以是 3 行，也可以是数百万行，它可以写在一个或多个扩展名为 ".c" 的文本文件中，例如，`hello.c`。我们可以使用 "vi"、"vim" 或任何其他文本编辑器来编写 C 语言程序。

### C11

C11（也被称为C1X）指 ISO 标准 ISO/IEC 9899:2011。在它之前的 C 语言标准为 C99。

#### 新特性

- 对齐处理（Alignment）的标准化（包括 _Alignas 标志符，alignof 运算符，aligned_alloc 函数以及 <stdalign.h> 头文件）。
- _Noreturn 函数标记，类似于 gcc 的 __attribute__((noreturn))。
- _Generic 关键字。
- 多线程（Multithreading）支持，包括：
    -  _Thread_local存储类型标识符，<threads.h>头文件，里面包含了线程的创建和管理函数。
    - _Atomic类型修饰符和<stdatomic.h>头文件。
- 增强的 Unicode 的支持。基于 C Unicode 技术报告 ISO/IEC TR 19769:2004，增强了对 Unicode 的支持。包括为 UTF-16/UTF-32 编码增加了 char16_t 和 char32_t 数据类型，提供了包含 unicode 字符串转换函数的头文件 <uchar.h> 。
- 删除了 gets() 函数，使用一个新的更安全的函数gets_s()替代。
- 增加了边界检查函数接口，定义了新的安全的函数，例如 fopen_s()，strcat_s() 等等。
- 增加了更多浮点处理宏(宏)。
- 匿名结构体/联合体支持。这个在gcc早已存在，C11将其引入标准。
- 静态断言（Static assertions），_Static_assert()，在解释 #if 和 #error 之后被处理。
- 新的 fopen() 模式，("…x")。类似 POSIX 中的 O_CREAT|O_EXCL，在文件锁中比较常用。
- 新增 quick_exit() 函数作为第三种终止程序的方式。当 exit()失败时可以做最少的清理工作。

## C 环境设置

设置 C 语言环境，先确保电脑上有以下两款可用的软件，文本编辑器和 C 编译器。

### 文本编辑器

文本编辑器用来写代码。包括 Windows Notepad、OS Edit command、Brief、Epsilon、EMACS 和 vim/vi。

文本编辑器的名称和版本在不同的操作系统上可能会有所不同。例如，Notepad 通常用于 Windows 操作系统上，vim/vi 可用于 Linux/UNIX 操作系统上。

通过编辑器创建的文件通常称为源文件，源文件包含程序源代码。C 程序的源文件通常使用扩展名 .c。

在开始编程之前，请确保您有一个文本编辑器，且有足够的经验来编写一个计算机程序，然后把它保存在一个文件中，编译并执行它。

### C 编译器

写在源文件中的源代码是人类可读的源。它需要"编译"，转为机器语言，这样 CPU 可以按给定指令执行程序。C 语言编译器用来把源代码编译成最终的可执行程序。

最常用的免费可用的编译器是 GNU 的 C/C++ 编译器，如果是 HP 或 Solaris，则可以使用各自操作系统上的编译器。

以下部分是如何在不同的操作系统上安装 GNU 的 C/C++ 编译器。这里同时提到 C/C++，主要是因为 GNU 的 gcc 编译器适合于 C 和 C++ 编程语言。

#### UNIX/Linux 上的安装

对于 Linux 或 UNIX，请在命令行使用下面的命令来检查您的系统上是否安装了 GCC：
```Shell
$ gcc -v
```
如果已经安装了 GNU 编译器，则会显示如下消息：
```
Using built-in specs.
Target: i386-redhat-linux
Configured with: ../configure --prefix=/usr .......
Thread model: posix
gcc version 4.1.2 20080704 (Red Hat 4.1.2-46)
```

如果未安装 GCC，参见 http://gcc.gnu.org/install/ 上的详细说明安装 GCC。

#### Mac OS 上的安装

对于 Mac OS X，最快捷的获取 GCC 的方法是从苹果的网站上下载 Xcode 开发环境，并按照安装说明进行安装。一旦安装上 Xcode，您就能使用 GNU 编译器。

Xcode 目前可从 http://developer.apple.com/technologies/tools/ 上下载。

#### Windows 上的安装

在 Windows 上安装 GCC，需要先安装 MinGW。安装 MinGW，在 [mingw-w64.org](https://www.mingw-w64.org/downloads/#mingw-builds)，下载最新版本的 MinGW 安装程序，命名格式为 MinGW-<version>.exe。

当安装 MinGW 时，您至少要安装 `gcc-core`、`gcc-g++`、`binutils` 和 `MinGW runtime`，但是一般情况下都会安装更多其他的项。

而后添加将 MinGW 添加到 PATH 环境变量中，这样就可以在命令行中通过简单的名称来指定这些工具。

当完成安装时，您可以从 Windows 命令行上运行 `gcc`、`g++`、`ar`、`ranlib`、`dlltool` 和其他一些 GNU 工具。


## C 程序结构

C 程序主要包括以下部分：

- 预处理器指令
- 函数
- 变量
- 语句 & 表达式
- 注释

下面是用 C 写的经典的 `Hello, World` 代码实例：
```C
#include <stdio.h>
 
int main()
{
   /* 我的第一个 C 程序 */
   printf("Hello, World! \n");
   
   return 0;
}
```
对于这段程序：

1. 第一行 `#include <stdio.h>` 是**预处理器指令**，告诉 C 编译器在实际编译之前要包含 `stdio.h` 文件。
2. 下一行 `int main()` 是主函数，程序从这里开始执行。
3. 下一行 `/*...*/` 将会被编译器忽略，这里放置程序的注释内容。它们被称为程序的注释。
4. 下一行 `printf(...)` 是 C 中另一个可用的函数，会在屏幕上显示消息 "Hello, World!"。
5. 下一行 `return 0`; 终止 `main()` 函数，并返回值 "0"。

键入 & 编译 & 执行 C 程序

将源代码保存在一个文件中，一级如何编译并执行（运行）它，步骤如下（以上面 "Hello, World!" 代码为例子）：

1. 打开一个文本编辑器，添加上述代码。
2. 保存文件为 `hello.c`。
3. 打开命令提示符，进入到保存文件所在的目录。
4. 键入 `gcc hello.c`，输入回车，编译代码。
5. 如果代码中没有错误，命令提示符会跳到下一行，并生成 a.out（Windows 生成 a.exe） 可执行文件。
6. 现在，键入 a.out 来执行程序。
7. 我们可以看到屏幕上显示 "Hello World"。

```Shell
$ gcc hello.c
$ ./a.out
Hello, World!
```

需要确保电脑中已包含 gcc 编译器，并确保在包含源文件 hello.c 的目录中运行它。

如果是多个 c 代码的源码文件，编译方法如下：
```Shell
$ gcc test1.c test2.c -o main.out
$ ./main.out
```
`test1.c` 与 `test2.c` 是两个源代码文件。


## C 基本语法

### C 的令牌（Token）

C 程序由各种令牌组成，令牌可以是关键字、标识符、常量、字符串值，或者是一个符号。例如，下面的 C 语句包括五个令牌：

```Shell
printf("Hello, World! \n");
```

这五个令牌分别是：

1. `printf`
2. `(`
3. `)`
4. `;`

### 分号；

在 C 程序中，分号是语句结束符。也就是说，每个语句必须以分号结束。它表明一个逻辑实体的结束。

例如，下面是两个不同的语句：

```C++
printf("Hello, World! \n");
return 0;
```

### 注释

C 语言有两种注释方式：
- 单行注释 `//`
- 多行注释：`/* ... */`

!!! Warning

    不能在注释内嵌套注释，也不能出现在字符串或字符值中 ！！！

### 标识符

C 标识符是用来标识变量、函数，或任何其他用户自定义项目的名称。一个标识符以字母 A-Z 或 a-z 或下划线 _ 开始，后跟零个或多个字母、下划线和数字（0-9）。

C 标识符内不允许出现标点字符，比如 `@`、`$` 和 `%`。C 是**区分大小写**的编程语言。因此，在 C 中，Manpower 和 manpower 是两个不同的标识符。下面列出几个有效的标识符：

```Shell
mohd      zara   abc move_name a_123
myname50  _temp  j   a23b9     retVal
```

### 关键字

下表是 C 中的保留字。这些保留字不能作为常量名、变量名或其他标识符名称。

|    关键字     | 	说明                                                     |
| ------------ | --------------------------------------------------------- | 
| auto         | 声明自动变量。                                              | 
| break        | 跳出当前循环。                                              |
| case         | 开关语句分支。                                               |
| char         | 声明字符型变量或函数返回值类型。                               | 
| const        | 定义常量，如果一个变量被 const 修饰，那么它的值就不能再被改变。  |
| continue     | 结束当前循环，开始下一轮循环。                                |
| default      | 开关语句中的"其它"分支。                                     |
| do           | 循环语句的循环体。                                           |
| double       | 声明双精度浮点型变量或函数返回值类型。                         |
| else         | 条件语句否定分支（与 if 连用）。                              |
| enum         | 声明枚举类型。                                               |
| extern       | 声明变量或函数是在其它文件或本文件的其他位置定义。              |
| float        | 声明浮点型变量或函数返回值类型。                               |
| for          | 一种循环语句。                                                |
| goto         | 无条件跳转语句。                                              |
| if           | 条件语句。                                                   |
| int          | 声明整型变量或函数。                                          |
| long         | 声明长整型变量或函数返回值类型。                               |
| register     | 声明寄存器变量。                                              |
| return       | 子程序返回语句（可以带参数，也可不带参数）。                     |
| short        | 声明短整型变量或函数。                                         |
| signed       | 声明有符号类型变量或函数。                                     |
| sizeof       | 计算数据类型或变量长度（即所占字节数）。                        |
| static       | 声明静态变量。                                               |
| struct       | 声明结构体类型。                                             |
| switch       | 用于开关语句。                                               |
| typedef      | 用以给数据类型取别名。                                        |
| unsigned     | 声明无符号类型变量或函数。                                     |
| union        | 声明共用体类型。                                               |
| void         | 声明函数无返回值或无参数，声明无类型指针。                        |
| volatile     | 说明变量在程序执行中可被隐含地改变。                              |
| while        | 循环语句的循环条件。                                             |

#### C99 新增关键字

<table>
    <tr>
        <td>_Bool</td>
        <td>_Complex</td>
        <td>_Imaginary</td>
        <td>inline</td>
        <td>restrict</td>
    </tr>
</table>

#### C11 新增关键字

<table>
    <tr>
        <td>_Alignas</td>
        <td>_Alignof</td>
        <td>_Atomic</td>
        <td>_Generic</td>
        <td>_Noreturn</td>
    </tr>
    <tr>
        <td>_Static_assert</td>
        <td>_Thread_local</td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>

### C 中的空格

只包含空格的行，被称为空白行，可能带有注释，C 编译器会完全忽略它。

在 C 中，空格用于描述空白符、制表符、换行符和注释。空格分隔语句的各个部分，让编译器能识别语句中的某个元素（比如 int）在哪里结束，下一个元素在哪里开始。因此，在下面的语句中：

```C
int age;
```

在这里，int 和 age 之间必须至少有一个空格字符（通常是一个空白符），这样编译器才能够区分它们。另一方面，在下面的语句中：

```C
fruit = apples + oranges;   // 获取水果的总数
```

fruit 和 =，或者 = 和 apples 之间的空格字符不是必需的，但是为了增强可读性，您可以根据需要适当增加一些空格。

## C 数据类型

在 C 语言中，数据类型指的是用于声明不同类型的变量或函数的一个广泛的系统。变量的类型决定了变量存储占用的空间，以及如何解释存储的位模式。
