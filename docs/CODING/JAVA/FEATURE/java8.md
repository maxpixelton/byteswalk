# Java 8 新特性

Oracle 在 2014 年发布了 Java8(jdk1.8)，诸多原因使它成为目前市场上使用最多的 jdk 版本。

## Interface

Interface 的设计初衷是面向对象，提高扩展性。这也留有一点遗憾，Interface 修改的时候， 实现它的类也必须跟着改。

为解决接口修改与现有的实现不兼容的问题，新的 Interface 的方法可以用 `default` 或 `static` 修饰，这样就可以有方法体，实现类也不必重写此方法。

一个 interface 中可以有多个方法被它们修饰，这 2 个修饰符的区别主要也是普通方法和静态方法的区别。

1. `default` 修饰的方法，是普通实例方法，可以用 `this` 调用，可以被子类继承、重写。

2. `static` 修饰的方法，使用上和一般静态类方法一样。但它不能被子类继承，只能用 `Interface` 调用。

看下面的例子：

```java title="InterfaceNew"
public interface InterfaceNew {

    static void sm() {
        System.out.println("interface 提供的方法实现");
    }

    static void sm2() {
        System.out.println("interface 提供的方法实现");
    }

    default void def() {
        System.out.println("interface default 方法");
    }

    default void def2() {
        System.out.println("interface default2 方法");
    }

    // 必须实现类重写
    void f();
    
}
```

```java title="InterfaceNew1"
public interface InterfaceNew1 {
    default void def() {
        System.out.println("InterfaceNew1 default方法");
    }
}
```

如果一个类既实现了 `InterfaceNew` 接口又实现了 `InterfaceNew1` 接口，它们两个都有 `def()`，并且 `InterfaceNew` 接口和 `InterfaceNew1` 接口没有继承关系的话，这时就必须重写 `def()`，否则，编译的时候就会报错。

```java title="InterfaceNewImpl"
public class InterfaceNewImpl implements InterfaceNew , InterfaceNew1{
    public static void main(String[] args) {
        InterfaceNewImpl interfaceNew = new InterfaceNewImpl();
        interfaceNew.def();
    }

    @Override
    public void def() {
        InterfaceNew1.super.def();
    }

    @Override
    public void f() {
    }
}
```

!!! question "在 Java 8 中，接口和抽象类有什么区别 ？"

    这里有个疑惑：“既然 interface 也可以有自己的方法实现，似乎和 abstract class 没有太大区别了 ？”

    其实它们还是有区别的：

    1. interface 和 class 的区别（有点像废话），主要有：

        - 接口是多实现，类是单继承。

        - 接口的方法是 public abstract 修饰，变量是 public static final 修饰。 abstract class 可以用其他修饰符。

    2. interface 的方法更像是一个扩展插件，而 abstract class 的方法是要继承的。

    interface 新增 `default` 和 `static` 修饰的方法，是为了解决接口的修改与现有的实现不兼容的问题，而不是为了要替代 `abstract class`。在使用上，该用 `abstract class` 的地方还是要用 abstract class，不要因为 interface 的新特性而将之替换。

<p style="font-size:30px;color:red"><strong><em>记住：接口永远和类不一样！</em> </strong></p>


## functional interface 函数式接口

!!! notes "定义"

    <p style="color:red"><strong>Single Abstract Method interface，又称 SAM 接口，有且只有一个抽象方法，但可以有多个非抽象方法的接口。</strong></p>


在 Java 8 中，专门有一个包放函数式接口 java.util.function，该包下的所有接口都有 `@FunctionInterface` 注解，提供函数式编程。

在其它包中也有函数式接口，其中一些没有 `@FunctionalInterface` 注解，但是只要符合函数式接口的定义就是函数式接口，与是否有 `@FunctionalInterface` 注解无关，注解只是在编译时起到强制规范定义的作用，其在 Lambda 表达式中有广泛的应用。

## Lambda 表达式

Lambda 表达式是推动 Java 8 发布的最重要的特性，是继泛型（Generics）和注解（Annotation）以来最大的变化。

使用 Lambda 表达式可以让代码变的更加简介紧凑，让 Java 也能支持简单的<em>函数式编程。</em>

> Lambda 表达式是一个匿名函数，java 8 允许把函数作为参数传递进方法中。

```java "Lambda 表达式语法格式"
(parameters) -> expression 或
(parameters) -> { statements;}
```

下面实例可以来感受 Lambda 带来的便利。

### 替代匿名内部类

过去给方法传递动态参数的唯一方法是使用内部类，比如：


```java title="Runnable 接口"
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("The runable now is using!");
    }
}).start();

// 用lambda
new Thread(() -> System.out.println("It's a lambda function!")).start();
```

```java title="Comparator 接口"
List<Integer> strings = Arrays.asList(1, 2, 3);

strings.sort(new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o1 - o2;
    }
});

// 用 lambda
strings.sort((Integer o1, Integer o2) -> o1 - o2);
// 分解开
Comparator<Integer> comparator = (Integer o1, Integer o2) -> o1 - o2;
strings.sort(comparator);
```

```java title="Listener 接口"
// Listener 接口
JButton jButton = new JButton();
jButton.addItemListener(new ItemListener() {
    @Override
    public void itemStateChanged(ItemEvent e) {
        e.getItem();
    }
});
// lambda
jButton.addItemListener(ItemEvent::getItem);
```

上面 3 个例子是在开发中最常见的，从中可以体会到 Lambda 带来的便捷和清爽。它只保留实际用到的代码，将无用代码全部省略。

当然它对接口是有要求的，可以从这些匿名内部类发现，他们只重写了接口的一个方法，当然也只有一个方法须要重写，这就是上面说的 "函数式接口"，也就是说只要方法的参数是函数式接口都可以用 Lambda 接口。



### 集合迭代

### 方法的引用

Java 8 允许使用 `::` 关键字来传递方法或构造函数引用，无论如何，表达式返回的类型必须是 functional-interface。

```java

```

### 访问变量

```java
public static void main(String[] args) {
    int value = 100;
    VariableInterface1 v = () -> {
        int num = value - 90;
        System.out.println(num);
    };
    System.out.println(value);
}
```

lambda 表达式可以引用外边变量，但是该变量默认拥有 final 属性，不能被修改，如果修改，编译时就报错。

## Stream

