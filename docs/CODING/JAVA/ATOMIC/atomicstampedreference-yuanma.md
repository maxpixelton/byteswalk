# AtomicStampedReference 源码（ABA 问题）

!!! Question

    1. 什么是 ABA ？

    2. ABA 的危害 ？

    3. ABA 的解决方法 ？

    4. AtomicStampedReference 是什么 ？

    5. AtomicStampedReference 是怎么解决 ABA 的 ？

AtomicStampedReference 是 java 并发包下提供的一个原子类，它能解决其它原子类无法解决的 ABA 问题。


## ABA

<font color=red>__ABA 问题__ 发生在多线程环境中，当某线程连续读取同一块内存地址两次，两次得到的值是一样的，它简单地认为 “此内存地址的值并没有被修改过”，然而，同时可能存在另一个线程在这两次读取之间把这个内存地址的值从 A 修改成了 B 有修改回了 A，这时还简单地认为 “没有修改过” 是显然错误的。</font>

比如，两个线程按下面的顺序执行：

1. 线程 1 读取内存位置 X 的值为 A。

2. 线程 1 阻塞了。

3. 线程 2 读取内存位置 X 的值为 A。

4. 线程 2 修改了内存位置 X 的值为 B。

5. 线程 2 又修改了内存位置 X 的值为 A。

6. 线程 1 恢复，继续执行，比较发现还是 A 把内存位置 X 的值设置为 C。

![ABA]()

显然，针对线程 1 来说，第一次的 A 和第二次的 A 实际上并不是同一个 A。<font color=red>ABA 通常发生在无锁结构中</font>，用代码表示上面的过程大概是这样的：

```java
public class ABATest {
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(1);

        new Thread(()->{
            int value = atomicInteger.get();
            System.out.println("thread 1 read value: " + value);

            // 阻塞1s
            LockSupport.parkNanos(1000000000L);

            if (atomicInteger.compareAndSet(value, 3)) {
                System.out.println("thread 1 update from " + value + " to 3");
            } else {
                System.out.println("thread 1 update fail!");
            }
        }).start();

        new Thread(()->{
            int value = atomicInteger.get();
            System.out.println("thread 2 read value: " + value);
            if (atomicInteger.compareAndSet(value, 2)) {
                System.out.println("thread 2 update from " + value + " to 2");

                // do sth
                value = atomicInteger.get();
                System.out.println("thread 2 read value: " + value);
                if (atomicInteger.compareAndSet(value, 1)) {
                    System.out.println("thread 2 update from " + value + " to 1");
                }
            }
        }).start();
    }
}
```
打印结果如下：

![ABATest 的输出结果](https://shichuan-hao.github.io/images/static/java/java-apatest-output.png)

## ABA 的危害

假设有一个无锁的栈结构，看下面代码：

```java
public class ABATest2 {

    static class Stack {
        // 将 top 放在原子类中，
        private final AtomicReference<Node> top = new AtomicReference<>();

        // 栈中节点信息
        static class Node {
            int value;
            Node next;
            public Node(int value) {
                this.value = value;
            }
        }

        // 出栈操作
        public Node pop() {
            for (;;) {
                // 获取栈顶节点
                Node t = top.get();
                if (t == null) {
                    return null;
                }
                // 栈顶下一个节点
                Node next = t.next;
                // CSA 更新 top 指向其 next 节点
                if (top.compareAndSet(t, next)) {
                    // 把栈顶元素弹出，应该把 next 清空防止外面直接操作栈
                    t.next = null;
                    return t;
                }
            }
        }

        // 入栈操作
        public void push(Node node) {
            for (;;) {
                // 获取栈顶节点
                Node next = top.get();
                // 设置栈顶节点为新节点的 next 节点
                node.next = next;
                // CAS 更新 top 指向新节点
                if (top.compareAndSet(next, node)) {
                    return;
                }
            }
        }
    }
}
```
咋一看，上面代码没有什么问题，然而想一下下面情形：

假如，初始化栈结构为 top->1->2->3，而后两个线程分别做如下操作：

1. 线程 1 执行 pop() 出栈操作，但是执行到 `if (top.compareAndSet(t, next)) {` 这行之前暂停了，所以此时节点 1 并未出栈；

2. 线程 2 执行 pop() 出栈操作弹出节点 1，此时栈变为 top -> 2 -> 3。

3. 线程 2 执行 pop() 出栈操作弹出节点 2，此时栈变为 top -> 3。

4. 线程 2 执行 push() 入栈操作添加节点 1，此时栈变为 top -> 1 -> 3。

5. 线程 1 恢复执行，比较节点 1 的引用并没有改变，执行 CAS 成功，此时栈变为 top -> 2。

看看，点解变成 top -> 2 了 ？不是应该变成 top -> 3 吗 ？这是因为线程 1 在第一步保存的 next 是节点 2，所以它执行 CAS 成功后 top 节点就指向了节点 2 了。

测试代码如下：

```java
private static void testStack() {
    // 初始化栈为 top -> 1 -> 2 -> 3
    Stack stack = new Stack();
    stack.push(new Stack.Node(3));
    stack.push(new Stack.Node(2));
    stack.push(new Stack.Node(1));

    // 线程 1 出栈一个元素
    new Thread(stack::pop)
            .start();

    new Thread(() -> {
        // 线程 2 出栈两个元素
        Stack.Node A = stack.pop();
        Stack.Node B = stack.pop();
        // 线程 2 又将 A 入栈
        stack.push(A);
    }).start();
}

public static void main(String[] args) {
    testStack();
}
```

在 Stack 的 pop() 方法的 `if (top.compareAndSet(next, node)) {` 处打个断点，线程 1 运行到这里时阻塞它的执行，让线程 2 执行，再执行线程 1 这句，这句执行完可以看到栈的 top 对象中只有 2 这个节点了。

!!! tips

    记得打断点的时候一定要打 Thread 断点，在 IDEA 中是右击选择 Suspend 为 Thread。如下图：

    ![IDEA Thread 断点](https://shichuan-hao.github.io/images/static/java/java-debug-threa-assert.png)


## ABA 的解决办法

大概有以下三种：

1. __版本号__。比如上面的栈结构增加一个版本号用来控制，每次 CAS 的同时检查版本号有没有变过。还有一些数据结构喜欢使用最高位存储一个__邮戳（时间搓）__来保证 CAS 的安全。

2. __不重复使用节点的引用__。比如，上面的栈结构在线程 2 执行 push() 入栈操作的时候，新建一个节点传入，而不是重复节点 1 的引用。

3. __直接操作元素而不是节点__。比如，上面的栈结构 push() 方法不应该传入一个节点（Node），而是传入元素值（int 的 value）。

## 源码分析

接下来，从 Java 中的 AtomicStampedReference 总结些经验，看看是怎么解决 ABA 问题的。

### 内部类

### 属性

### 构造方法

### 栗子

## Java 中其它可以解决 ABA 问题的类 ！

## 总结

1. 在多线程环境下使用无锁结构要注意 ABA 问题。

2. ABA 的解决一般是使用版本号来控制，并保证数据结构使用元素值来传递，且每次添加元素都新建节点承载元素值。

3. AtomicStampeReference 内部使用 Pair 来存储元素值以及版本号。
