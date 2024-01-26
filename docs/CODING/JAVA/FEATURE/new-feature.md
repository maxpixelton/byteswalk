# 新特性


## Java 21 __🏆__

JDK 21 于 2023 年 9 月 19 日发布，这是一个非常重要的版本，里程碑式。

JDK 21 共有 15 个特性，这篇文章挑选其中较为重要的一些新特性详细介绍：

1.  [JEP 430: 字符串模板 (预览)](https://openjdk.org/jeps/430) __🔥__
2.  [JEP 431: 序列号集合](https://openjdk.org/jeps/431) __🔥__
3.  [JEP 439: 分代 ZGC](https://openjdk.org/jeps/439) __🔥__
4.  [JEP 440: 记录模式](https://openjdk.org/jeps/440) __🔥__
5.  [JEP 441: switch 的模式匹配](https://openjdk.org/jeps/441) __🔥__
6.  [JEP 442: 外部函数和内存 API（第三次预览）](https://openjdk.org/jeps/442) __🔥__
7.  [JEP 443: 未命名模式和变量（预览）](https://openjdk.org/jeps/443) __🔥__
8.  [JEP 444: 虚拟线程](https://openjdk.org/jeps/443) __🔥__
9.  [JEP 445: 未命名类和实例 main 方法 (预览)](https://openjdk.org/jeps/445) __🔥__
10. [JEP 446: 范围值 (预览)](https://openjdk.org/jeps/446)
11. [JEP 448: Vector API (第六孵化器)](https://openjdk.org/jeps/448)
12. [JEP 449: 停用 Windows 32 位 x86 端口，以便移除](https://openjdk.org/jeps/449)
13. [JEP 451: 准备禁止动态加载代理点击并应用](https://openjdk.org/jeps/451)
14. [JEP 452: 密钥封装机制 API](https://openjdk.org/jeps/452)
15. [JEP 453: 结构化并发 (预览)](https://openjdk.org/jeps/453)

下面挑选较为重要的一些新特性进行详细介绍。

### JEP 430 字符串模板（预览）

目前，__String Template (字符串模板)__ 依然是 JDK 21 中的一个预览功能。

__String Template (字符串模板)__ 提供了一个更简洁、更直观的方式来动态构建字符串。通过使用占位符 `${}`，我们可以将变量的值直接嵌入到字符串中，而不需要手动处理。在运行时，Java 编译器会将这些占位符替换为实际的变量值。并且，表达式支持局部变量、静态/非静态字段甚至方法，计算结果等特性。

实际上，__String Template (字符串模板)__ 在大多数编程语言中都存在：
```
"Greeting {{ name }}!";   // Angular
"Greeting ${ name }!";    // Typescript
$"Greeting { name }!"     // Visual basic
f"Greeting { name }!"     // Python
```

在没有 __String Template (字符串模板)__ 之前，我们通常使用字符串拼接或格式化方法来构建字符串：
```Java
// concatenation
message = "Greetings " + name + "!";

// String.format()
message = String.format("Greeting %s!", name)

// MessageFormat
message = new Messageformat("Greetings {0}").format(name);

// StringBuilder
message = new StringBuilder().append("Greetings ").append(name).append("!").toString();
```

这些方式或多或少都存在难以阅读、冗长、复杂等缺点。

Java 使用 __String Template (字符串模板)__ 进行字符串拼接，可以直接在字符串中嵌入表达式，无需进行额外处理。

```Java
String message = STR."Greeting \{name}";
```

在上面的模板表达式中：

- __STR__ 是模板处理器。

- `\{name}` 为表达式，运行时，这些表达式将被相应的变量值替换。

目前，Java 目前支持三种模板处理器：

- __STR__: 自动执行字符串插值，即将模板中的每个嵌入式表达式替换为其值（转换为字符串）。

- __FMT__：与 __STR__ 类似，但是它还可以接受格式说明符，这些格式说明符出现在嵌入式表达式的左边，用来控制输出样式。

- __RAW__：不会像 __STR__ 和 __FMT__ 模板处理器自动处理字符串模板，而是返回一个 `StringTemplate` 对象， 这个对象包含了模板中的文本和表达式的信息。

```Java
String name = "Lokeesh";

// STR
String messageSTR = STR."Greetings \{name}.";
System.out.println(messageSTR);

// FMT
String messageFMT = STR. "Greetring %-12s\{ name }." ;
System.out.println(messageFMT);

// RAW
StringTemplate st = RAW."Greetings \{name}.";
String messageRAW = STR.process(st);
System.out.println(messageRAW);
```

除了 JDK 自带的三种模板处理器外，我们还可以通过实现 `StringTemplate.Processor` 接口来创建自己的模板处理器。

我们还可以使用局部变量、静态或非静态字段甚至方法作为嵌入表达式：

```Java
// variable
String name = "张三";
String messageVariable = STR."Greetings \{name}.";
        
// method
String messageMethod = STR."Greetings \{getName()}.";

// field
String messageField = STR."Greetings \{this.age}!";
```

还可以在表达式中执行计算并打印结果：
```Java
int x = 10, y = 20;
// "10 + 20 = 30"
String s = STR. "\{ x } + \{ y } = \{ x + y }" ;
System.out.println(s);
```

为了提高可读性，我们还可以将嵌入的表达式分成多行：
```Java
String time = STR. "The current time is \{
    /* sample comment - current time in HH:mm:ss */
    DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").format(LocalDateTime.now())
    }." ;
System.out.println(time);
```


### JEP 431 序列号集合

JDK 21 引入了一种新的集合类型：__Sequenced Collections（序列号集合，也叫有序集合）__，这是一种具有确定出现顺序（encounter order）的集合（无论遍历这样的集合多少次，元素的出现顺序始终是固定的）。序列化集合提供了处理集合的第一个和最后一个元素以及反向视图（与原始集合相反的顺序）的简单方法。

__Sequenced Collections（序列号集合，也叫有序集合）__ 包括以下三个接口：

- [SequencedCollection](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/SequencedCollection.html)

- [SequencedSet](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/SequencedSet.html)

- [SequencedMap](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/SequencedMap.html)


`SequencedCollection`__ 接口继承了 `Collection` 接口，提供了在集合两端访问、添加或删除元素以及获取集合的反向视图的方法。

```Java
public interface SequencedCollection<E> extends Collection<E> {

    // New Method
    SequencedCollection<E> reversed();

    
    // Promoted methods from Deque<E>

    default void addFirst(E e) {}
    default void addLast(E e) {}


    default E getFirst() {}
    default E getLast() {}

    default E removeFirst() {}
    default E removeLast() {}
}
```

`List` 和 `Deque` 接口实现了 `SequencedCollection` 接口。

下面以 `ArrayList` 为例子，展示下使用效果：
```Java
ArrayList<Integer> arrayList = new ArrayList<>();

arrayList.add(1);                               // List contains: [1]
arrayList.add(3);                               // List contains: [1, 3]

arrayList.addFirst(0);                  // List contains: [0, 1, 3]
arrayList.addLast(2);                   // List contains: [0, 1, 3, 2]

Integer first = arrayList.getFirst();           // 0
Integer last = arrayList.getLast();             // 2
arrayList.forEach(System.out::println);

List<Integer> reversed = arrayList.reversed();
reversed.forEach(System.out::println);          // Prints [2, 3, 1, 0]
```

`SequencedSet` 接口直接继承了 `SequencedCollection` 接口并重写了 `reversed()` 方法：
```Java
public interface SequencedSet<E> extends SequencedCollection<E>, Set<E> {
    /**
     * {@inheritDoc}
     *
     * @return a reverse-ordered view of this collection, as a {@code SequencedSet}
     */
    SequencedSet<E> reversed();
}
```

以 `LinkedHashSet` ，展示下使用效果:
```Java
LinkedHashSet<Integer> linkedHashSet = new LinkedHashSet<>(List.of(1, 2, 3));
StringJoiner joiner1 = new StringJoiner(", ", "[", "]");
for (Integer element : linkedHashSet) {
    joiner1.add(element.toString());
}
System.out.println("first source: " + joiner1.toString());

Integer first = linkedHashSet.getFirst();
System.out.println(STR."the first element of linkedHashSet: \{first}");
Integer last = linkedHashSet.getLast();
System.out.println(STR."the first element of linkedHashSet: \{last}");

linkedHashSet.addFirst(111);
linkedHashSet.addLast(222);
        
StringJoiner joiner2 = new StringJoiner(", ", "[", "]");
for (Integer element : linkedHashSet) {
    joiner2.add(element.toString());
}
System.out.println("end source: " + joiner2.toString());
```

输出结果：
```
first source: [1, 2, 3]
the first element of linkedHashSet: 1
the first element of linkedHashSet: 3
end source: [1, 2, 3, 111, 1, 2, 3, 222]
```

`SequencedMap` 接口继承了 `Map` 接口，提供了在集合两端访问、添加或删除键值对、获取包含 key 的 `SequencedSet`、包含 value 的 `SequencedCollection`、包含 entry（键值对）的 `SequencedSet` 以及获取集合的反向视图的方法。

```Java
public interface SequencedMap<K, V> extends Map<K, V> {

    // New Methods
    SequencedMap<K, V> reversed();


// Promoted Methods from NavigableMap<K, V>
    default Map.Entry<K,V> firstEntry() {}
    default Map.Entry<K,V> lastEntry() {}

    default Map.Entry<K,V> pollFirstEntry() {}
    default Map.Entry<K,V> pollLastEntry() {}

    default V putFirst(K k, V v) {}
    default V putLast(K k, V v) {}

    default SequencedSet<K> sequencedKeySet() {}
    default SequencedCollection<V> sequencedValues() {}
    
    default SequencedSet<Map.Entry<K, V>> sequencedEntrySet() {}
}
```

### JEP 439 分代 ZGC

### JEP 440 记录模式

### JEP 441 switch 的模式匹配

### JEP 442 外部函数和内存 API（第三次预览）

### JEP 443 未命名模式和变量（预览）

### JEP 444 虚拟线程

### JEP 445 未命名类和实例 main 方法 (预览)



## Java 21 __🏆__

## Java 19 __🏆__

