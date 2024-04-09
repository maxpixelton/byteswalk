# 控制流

## if 语句

用来有条件的执行：

```Python
if_stmt ::=  "if" assignment_expression ":" suite
             ("elif" assignment_expression ":" suite)*
             ["else" ":" suite]
```

它通过对表达式逐个求值直至找到一个真值（请参阅 布尔运算 了解真值与假值的定义）在子句体中选择唯一匹配的一个；然后执行该子句体（而且 if 语句的其他部分不会被执行或求值）。 如果所有表达式均为假值，则如果 else 子句体如果存在就会被执行。

```Python title='if 语句示例'
>>> x = int(input("Please enter an integer: ")) 
Please enter an integer: 42
>>> if x < 0:
...     x = 0
...     print('Negative changed to zero') 
... elif x == 0:
...     print('Zero')
... elif x == 1:
...     print('Single')
... else:
...     print('More') 
...
More
```

可有零个或多个 elif 部分，else 部分也是可选的。关键字 'elif' 是 'else if' 的缩写，用于避免过多的缩进。

if ... elif ... elif ... 序列可以当作其它语言中 switch 或 case 语句的替代品。

如果是把一个值与多个常量进行比较，或者检查特定类型或属性，match 语句更有用。详见 match 语句。

## for 语句

用于对序列（例如字符串、元组或列表）或其他可迭代对象中的元素进行迭代:



## while 语句

用于在表达式保持为真的情况下重复地执行:

```Python
while_stmt ::=  "while" assignment_expression ":" suite
                ["else" ":" suite]
```

这将重复地检验表达式，并且如果其值为真就执行第一个子句体；如果表达式值为假（这可能在第一次检验时就发生）则如果 else 子句体存在就会被执行并终止循环。

第一个子句体中的 break 语句在执行时将终止循环且不执行 else 子句体。 第一个子句体中的 continue 语句在执行时将跳过子句体中的剩余部分并返回检验表达式。