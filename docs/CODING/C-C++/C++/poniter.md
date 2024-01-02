# 指针

我们通过指针，可以简化一些 C++ 编程任务的执行，如动态内存分配，没有指针是无法执行的。所以，掌握好指针是成为一名优秀的 C++ 程序员的石头。

众说周知，每一个变量都有一个内存位置，每一个内存位置都定义了可使用<font color="blue">连字号运算符（&）</font>访问的地址，它表示了在内存中的一个地址。

看下面代码实例，它将输出定义的变量的地址：
```C++
#include <iostream>

using namespace std;

int main{
    int var1;
    char var2[10];

    cout << "var1 变量的地址：";
    cout << &var1 << endl;

    cout << "var2 变量的地址："
    cout << &var2 << endl;

    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
var1 变量的地址：000000B51C1EF5E4
var2 变量的地址：000000B51C1EF608
```

通过这个例子，我们可以了解什么是内存地址以及如何访问它。

<hr/>

## 什么是指针 ？

<font color="red">指针是一个变量，它的值是另一个变量的地址，即内存位置的直接地址</font> 。与其他变量或常量一样，我们必须在使用指针存储其他变量地址之前，对其进行声明。

指针变量声明的一般形式为：
```C++
type *var-name;
```
在这里：

- **`type`** 是指针的*基类型*，它必须是一个有效的 C++ 数据类型。
- <font color="blue">一元运算符 （*）</font> 用来声明指针，与乘法中使用的星号是相同的，不过，在这个语句中，星号是用来指定一个变量是指针。
- **`var-name`** 是指针变量的名称。

下面是有效声明指针的实例：
```C++
int *ip;  /* 一个整型的指针 */
double *dp; /* 一个 double 型的指针 */
float *fp; /* 一个浮点数的指针 */
char *ch; /* 一个字符型的指针 */
```

所有指针的值的实际数据类型，不管是整型、浮点型、字符型，还是其他的数据类型，都是一样的，都是代表内存地址的长的十六进制数。

不同数据类型的指针之间唯一不同的是：<font color="red">指针所指向的变量或常量的数据类型不同</font>。

<hr/>

## 在 C++ 中使用指针

使用指针时会频繁进行以下 3 个操作：

1. 定义一个指针变量。
2. 把变量地址赋值给指针。
3. 访问指针变量中可用地址的值。

这些是通过使用 <font color="blue">一元运算符 （*）</font>来返回位于操作数所指定的变量的值。下面的实例涉及到了这些操作：
```C++
#include <iostream>

using namespace std;


int main() {

	int var = 20; // 实际变量的声明
	int *ip;      // 指针变量的声明

	ip = &var;    // 在指针变量中存储 var 变量的地址

	cout << "Value of var variable: ";
	cout << var << endl;

	// 输出在指针变量中存储的地址
	cout << "Address stored in ip variable: ";
	cout << ip << endl;

	// 访问指针变量中地址的值
	cout << "Value of *ip variable: ";
	cout << *ip << endl;

	return 0;

}
```
当上面代码被编译和执行时，它会产生下列的结果：
```
Value of var variable: 20
Address stored in ip variable: 0000006083DDF8D4
Value of *ip variable: 20
```

## C++ Null 指针

!!! tip

    :boom: C++ 支持空指针。NULL 指针是一个定义在标准库中的值为 0 的常量。


在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 `NULL` 值是一个良好的编程习惯。赋值为 `NULL` 值的指针被称为空指针。如下面代码：

```C++
#include <iostream>

using namespace std;

int main() {

	int *ptr = NULL;

	cout << "ptr 的值是：" << ptr;

	return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：
```
ptr 的值是：0000000000000000
```

!!! Warning

    在大多数的操作系统上，程序不允许访问地址为 0 的内存，因为该内存是操作系统保留的。但是，内存地址 0 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。但是按照惯例，如果指针包含空值（零值），就假定它不执行任何东西。

如果要检查一个空指针，我们可以使用 if 语句，如下所示：
```C++

if(ptr)   /* 如果 ptr 非空，则完成 */

if(!ptr)  /* 如果 ptr 为空，则完成 */
```

!!! Warning

    因此，如果所有未使用的指针都被赋予空值，同时避免使用空指针，就可以防止误用一个未初始化的指针。大多数情况下，未初始化的变量存有一些垃圾值，导致程序难以调试。


## C++ 指针的算术运算符

!!! tip

    :boom: 指针是一个用数值表示的地址。因此，您可以对指针执行算术运算。可以对指针进行四种算术运算：`++`、`--`、`+`、`-`。

假设 `ptr` 是一个指向地址 1000 的整型指针，是一个 32 位的整数，下面对该指针指向下列的算术运算：
```C++
ptr++;
```

执行 `ptr++` 后，指针 `ptr` 会向前移动 4 个字节，指向下一个整型元素的地址。这是由于指针算术运算符会根据指针的类型和大小来决定移动的距离。在这种情况下，由于是一个 32 为整数指针，每个整数占据 4 个字节，因此 `ptr++` 会将指针 ptr 向前移动 4 个字节，指向下一个整型元素的地址。

如果 `ptr` 指向一个地址为 1000 的字符，执行 `ptr++` 指针 `ptr` 的值会增加，指向下一个字符元素的地址，由于 `ptr` 是一个字符指针，每个字符占据一个字节，因此 `ptr++` 会将 `ptr` 的值增加 1，执行后 `ptr` 执行地址 1001。

!!! tip annotate "指针算术运算符的详细解析"

    1. 🚀 <font color="purple">加法运算符</font>。*可以对指针进行加法运算。当一个指针 `p` 加上一个整数 `n` 时，结果是指向 `p` 向前移动 `n` 个元素的大小。例如，如果 `p` 是一个 `int` 类型的指针，每个 `int` 占 4 个字符，那么 `p + 1` 将指向 `p` 所指向的下一个 `int` 元素。*
    2. 🚀 <font color="purple">减法运算符</font>。*可以对指针进行减法运算。当一个指针 `p` 减去一个整数 `n` 时，结果是指针 `p` 向后移动 `n` 个元素的大小。例如，如果 `p` 是一个 `int` 类型的指针，每个 `int` 占4个字节，那么 `p - 1` 将指向p所指向的前一个 `int` 元素。*
    3. 🚀 <font color="purple">指针与指针之间的减法运算</font>。*可以计算两个指针之间的距离。当从一个指针 `p` 减去另一个指针 `q` 时，结果是两个指针之间的元素个数。例如，如果 `p` 和 `q` 是两个 `int` 类型的指针，每个 `int` 占 4 个字节，那么 `p - q` 将得到两个指针之间的元素个数。*
    4. 🚀 <font color="purple">指针与整数之间的比较运算</font>。*可以将指针与整数进行比较运算。可以使用关系运算符（如 `<` 、`>`、`<=`、`>=`）对指针和整数进行比较。这种比较通常用于判断指针是否指向某个有效的内存位置。*


### 递增一个指针

可以在程序中使用指针代替数组，因为变量指针可以递增，而数组不能递增，因为数组是一个常量指针。

下面的代码递增变量指针，以便访问数组中的每一个元素：
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
	int var[MAX] = {10, 100, 1000};
	int *ptr;
	
	// 指针中的数组地址
	ptr = var;
	
	for(int i = 0; i < MAX; i++)
	{
		cout << "The address of var[" << i << "] =";
		cout << ptr << endl;
		
		cout << "The value of var[" << i << "] =";
		cout << *ptr << endl;
		
		// 移动到下一个位置
		ptr++;
	}
	return 0;
}
```
当上面代码被编译执行，它会输出下列结果：
```
The address of var[0] = 0x71593ffc28
The value of var[0] = 10
The address of var[1] = 0x71593ffc2c
The value of var[1] = 100
The address of var[2] = 0x71593ffc30
The value of var[2] = 1000
```

### 递减一个指针
同样嘞，也可以对指针进行递减运算，<font color="red">即把值减去其数据类型的字节数</font>，如下所示：
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
    int var[MAX] = {10, 10, 1000};
    int *ptr;

    // 指针中最后一个元素的地址
    ptr = &var[MAX - 1];
    for (int i = MAX; i > 0; i--)
    {
        cout << "Address of var[" << i << "] = ";
        cout << ptr << endl;

        cout << "Value of var[" << i << "] = ";
        cout << *ptr << endl;

        // 移动到下一个位置
        ptr--;
    }
    
    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
Address of var[3] = 0x7f863ffcc0
Value of var[3] = 1000
Address of var[2] = 0x7f863ffcbc
Value of var[2] = 10
Address of var[1] = 0x7f863ffcb8
Value of var[1] = 10
```

### 指针的比较

指针也可以用关系运算符进行比较，如 `==`、`<`、`>`。如果 `p1` 和 `p2` 指向两个相关的变量，比如同一个数组中的不同元素，则可对 `p1` 和 `p2` 进行大小比较。

下面代码修改了上面的实例，只要变量指针所指向的地址小于或等于数组的最后一个元素的地址 `&var[MAX - 1]`, 就将变量指针进行递增。
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
    int var[MAX] = {10, 100, 1000};
    int *ptr;

    // 指针中第一个元素的地址
    ptr = var;
    int i = 0;
    while ( ptr <= &var[MAX - 1])
    {
        cout << "Address of var[" << i << "] = ";
        cout << ptr << endl;

        cout << "Value of var[" << i << "] = ";
        cout << *ptr << endl;

        // 指向上一个位置
        ptr++;
        i++;
    }

    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```C++
Address of var[0] = 0x2f11dff6b8
Value of var[0] = 10
Address of var[1] = 0x2f11dff6bc
Value of var[1] = 100
Address of var[2] = 0x2f11dff6c0
Value of var[2] = 1000
```

## C++ 指针 vs 数组

!!! tip

    :boom: 指针和数组之间有着密切的关系。事实上，指针和数组在很多情况下是可以互换的。比如，一个指向数组开头的指针，可以通过使用指针的算术运算或数组索引来访问数组。

如下代码实例：
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
	int var[MAX] = {10, 100, 1000};
	int *ptr;
	
	// 指针中的数组地址
	ptr = var;
	
	for(int i = 0; i < MAX; i++)
	{
		cout << "The address of var[" << i << "] = ";
		cout << ptr << endl;
		
		cout << "The value of var[" << i << "] = ";
		cout << *ptr << endl;
		
		// 移动到下一个位置
		ptr++;
	}
	return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
The address of var[0] = 0xf4419ffdc8
The value of var[0] = 10
The address of var[1] = 0xf4419ffdcc
The value of var[1] = 100
The address of var[2] = 0xf4419ffdd0
The value of var[2] = 1000
```

当然，指针和数组并不是完全互换的，例如，下面代码：
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
    int var[MAX] = {10, 100, 1000};

    for (int i = 0; i < MAX; i++)
    {
        *var = i;      // 正确做法
        var++;         // 错误做法
    }
    return 0;
    
}
```
将 指针运算符 * 应用到 var 上是完全可以的，但修改 var 的值是非法的。这是因为 var 是一个指向数组开头的常量，不能作为左值。

由于一个数组名对应一个指针常量，只要不改变数组的值，仍然可以用指针形式的表达式。例如，下面是一个有效的语句，把 `var[2] 赋值为 500`:
```C++
*(var + 2) = 500;
```
这个语句之所以是有效的，且能成功编译，是因为 var 未改变。




## C++ 指针数组

!!! tip

    :boom: 	可以定义用来存储指针的数组。

在熟悉指针数组的概念之前，先看下面的实例，它用到了一个由 3 个整数组成的数组：
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
    int var[MAX] = {10, 100, 200};


    for (int i = 0; i < MAX; i++) {
        cout << "Value of var[" << i << "] = ";
        cout << var[i] << endl;
    }
    return 0;
}
```
当编译和执行上面代码，它会产生下列结果：
```
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```

可能还有一种情况，我们想要让数组存储指向 int 或 char 或其他数据类型的指针。如下面是一个指向整数的指针数组的声明：
```C++
int *ptr[MAX];
```

我们也可以用一个指向字符的指针数组来存储一个字符串列表，如下：
```C++
int *ptr[MAX];
```

在这里，将 ptr 声明为一个数组，由 MAX 个整数指针组成。因此，ptr 中的每个元素，都是一个指向 int 值的指针。下面实例用到了三个整数，它们将存储在一个指针数组中，如下：
```C++
#include <iostream>

using namespace std;

const int MAX = 3;

int main()
{
    int var[MAX] = {10, 100, 200};
    int *ptr[MAX];

    for(int i = 0; i < MAX; i++)
    {
        ptr[i] = &var[i];           // 赋值为整数的地址
    }

    for(int i = 0; i < MAX; i++)
    {

        cout << "Use 'var[i]' : value of var[" << i << "] = ";
        cout << var[i] << endl;

        cout << "\n";
        
        cout << "Use '&var[i]' : address of var[" << i << "] = ";
        cout << &var[i] << endl;
        
        cout << "\n";

        cout << "Use '*ptr[i]' : value of var[" << i << "] = ";
        cout << *ptr[i] << endl;
        
        cout << "\n";
        
        cout << "Use '*ptr[i]' address of var[" << i << "] = ";
        cout << ptr[i] << endl;

        cout << "<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<" << endl;

    }

    return 0;
}
```
编译和执行上面代码，它输出了下列结果：
```
Use 'var[i]' : value of var[0] = 10

Use '&var[i]' : address of var[0] = 0xf51abffa5c

Use '*ptr[i]' : value of var[0] = 10

Use '*ptr[i]' address of var[0] = 0xf51abffa5c
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
Use 'var[i]' : value of var[1] = 100

Use '&var[i]' : address of var[1] = 0xf51abffa60

Use '*ptr[i]' : value of var[1] = 100

Use '*ptr[i]' address of var[1] = 0xf51abffa60
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
Use 'var[i]' : value of var[2] = 200

Use '&var[i]' : address of var[2] = 0xf51abffa64

Use '*ptr[i]' : value of var[2] = 200

Use '*ptr[i]' address of var[2] = 0xf51abffa64
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
```

同理，我们也可以用一个指向字符的指针数组来存储一个字符串列表，如下：
```C++
#include <iostream>

using namespace std;

const int MAX = 4;

int main()
{
    const char *names[MAX] = {
        "Zara Ali",
        "Hina Ali",
        "Nuha Ali",
        "Sara Ali"
    };

    for (int i = 0; i < MAX; i++)
    {
        cout << "Value of names[" << i << "] = ";
        cout << names[i] << endl;

        cout << "Address of names[" << i << "] = ";
        cout << *names[i] << endl;
    }
    
    return 0;
}
```
上代码输出的结果是：
```
Value of names[0] = Zara Ali
Address of names[0] = Z
Value of names[1] = Hina Ali
Address of names[1] = H
Value of names[2] = Nuha Ali
Address of names[2] = N
Value of names[3] = Sara Ali
Address of names[3] = S
```


## C++ 指向指针的指针（多级间接寻址）

指向指针的指针是一种多级间接寻址的形式，或者说是一个指针链。

指针的指针是将指针的地址存放在另一个指针里面。

通常情况下，一个指针包含一个变量的地址。当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置。


![指向指针的指针](https://shichuan-hao.github.io/images/assert/CPLUSPLUS/pointer_to_pointer.jpg)

一个指向指针的指针变量必须声明为在变量名前放置两个星号，比如，声明一个指向 int 类型指针的指针：
```C++
#include <iostream>

using namespace std;

int main()
{
    int var;
    int *ptr;
    int **pptr;

    var = 3000;

    // 获取 var 的地址
    ptr = &var;

    // 使用运算符 & 获取 ptr 的地址
    pptr = &ptr;

    // 使用 pptr 获取值
    cout << "The value of var: " << var << endl;
    cout << "The value of *ptr: " << *ptr << endl;
    cout << "The value of **ptr: " << **pptr << endl;
    // cout << "var 的值：" << var << endl;
    // cout << "*ptr 的值：" << *ptr << endl;
    // cout << "**ptr 的值：" << **pptr << endl;

    return 0;
}
```
当上面代码被编译和执行时，它会产生下列结果：
```
The value of var: 3000
The value of *ptr: 3000
The value of **ptr: 3000
```
!!! Bug

    在上面代码中，使用下面输出时，输出的结果乱码！不知道啥原因！留待以后解决！！！
    ```C++
    cout << "var 的值：" << var << endl;
    cout << "*ptr 的值：" << *ptr << endl;
    cout << "**ptr 的值：" << **pptr << endl;
    ```

## C++ 传递指针给函数

C++ 允许我们将指针传递函数，只需要简单地声明函数参数为指针类型即可。

下面代码中，我们传递一个无符号的 long 型指针给函数，并在函数内改变这个值。
```C++
#include <iostream>
#include <ctime>

using namespace std;

// 在些函数时应该习惯性的先声明函数，然后再定义函数
void getSeconds(unsigned long *par);

void getSeconds(unsigned long *par)
{
    // 获取当前的秒数
    *par = time( NULL );
    return;
}

int main()
{
    unsigned long sec;

    getSeconds( &sec );

    // 输出实际值
    cout << "Number of seconds: " << sec << endl;

    return 0;
}
```
编译和执行上面代码时，输出下面结果：
```
Number of seconds :1294450468
```

能接受指针作为参数的函数，也能接受数组作为参数，如下所示：
```C++
```
当上面的代码被编译和执行时，它会产生下列结果：
```
```



!!! tip

    :boom: 通过引用或地址传递参数，使传递的参数在调用函数中被改变。

## C++ 从函数返回指针

!!! tip

    :boom: C++ 允许函数返回指针到局部变量、静态变量和动态内存分配。

C++ 不仅可以从函数返回数组，类似地、C++ 也可以从函数返回指针。为了做到这点，我们必须声明一个返回指针的函数，如下所示：
```C++
int * myFunction()
{
    ....
    // <...code...>
}
```
!!! tip

    :boom: C++ 不支持在函数外返回局部变量的地址，除非定义局部变量为 static 变量。

现在，看下面代码，它会随机生成 10 个随机数，并使用表示指针的数组名（即第一个数组元素的地址）来返回它们，具体如下：
```C++
#include <iostream>
#include <ctime>
#include <cstdlib>

using namespace std;

// 要生成和返回随机数的函数
int * getRadnom()
{
    static int r[10];

    // 设置种子  ？？ 什么是种子 ？？
    srand( (unsigned)time( NULL) );
    for (int i = 0; i < 10; ++i)
    {
        r[i] = rand();
        cout << r[i] << endl;
    }

    return r;
}

// 要调用上面定义函数的主函数
int main()
{
    // 一个指向整数的指针
    int *p;

    p = getRadnom();

    cout << "###################################" << endl;

    for ( int i = 0; i < 10; i++)
    {
        /* code */
        cout << "*(p + " << i << ") :";
        cout << *(p + i) << endl;
    }
    return 0;
}
```
编译和执行上面代码，会产生下列结果：
```
605
25177
31577
12525
19494
19853
31572
6776
18512
20207
###################################
*(p + 0) :605
*(p + 1) :25177
*(p + 2) :31577
*(p + 3) :12525
*(p + 4) :19494
*(p + 5) :19853
*(p + 6) :31572
*(p + 7) :6776
*(p + 8) :18512
*(p + 9) :20207
```