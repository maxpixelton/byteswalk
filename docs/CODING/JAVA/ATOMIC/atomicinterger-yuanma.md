# AtomicInteger 源码

!!! Question

    1. 什么是原子操作 ？

    2. 原子操作与数据库的 ACID 有啥关系 ？

    3. AtomicInteger 是怎么实现原子操作 ？

    4. AtomicInteger 有什么缺点 ?


AtomicInteger 是 Java 并发包下面提供的原子类，主要操作的是 int 类型的整型，通过调用底层 Unsafe 的 CAS 等方法实现原子操作。

关于 Unsafe，点击 <a href="./unsafe.md" target="_blank">__:material-book: 传送门__</a>


## 原子操作

<font color=red>__原子操作__ 是指不会被线程调度机制打断的操作，这种操作一旦开始，就一直运行到结束，中间不会有任何线程上下文切换。</font>

原子操作可以是一个步骤，也可以是多个操作步骤，但是其顺序不可以被打乱，也不可以被切割而只执行其中的一部分，将整个操作视为一个整体是原子性的核心特征。

!!! note

    这里的 <font color=red>__原子操作__</font> 与 <font color=#871F78>__数据库 ACID 中的原子性__</font>，最大的区别在于：数据库中的原子性主要运用在事务中，一个事务之内的所有更新操作要么都成功，要么都失败，事务是有回滚机制的，而这里的 <font color=red>__原子操作__</font> 是没有回滚的

## 源码分析

### 主要属性


```java
// setup to use Unsafe.compareAndSwapInt for updates
// 获取 unsafe 实例
private static final Unsafe unsafe = Unsafe.getUnsafe();
// 标识 value 字段的偏移量
private static final long valueOffset;

// 静态代码块，通过 unsafe 获取 value 的偏移量
static {
    try {
        valueOffset = unsafe.objectFieldOffset
            (AtomicInteger.class.getDeclaredField("value"));
    } catch (Exception ex) { throw new Error(ex); }
}

// 存储 int 类型值得地方，使用 volatile
private volatile int value;
```

1. 使用 int 类型的 value 存储值，且使用 volatile 修饰。<font color=red>volatile 主要是保证可见性,即一个线程修改对另一个线程立即可见，主要的实现原理是 __内存屏障__</font>

2. 调用 Unsafe 的 objectFieldOffset() 方法获取 value 字段在类中的偏移量，用于后面的 CAS 操作时使用。

## 主要方法


```java
/**
 * Atomically sets the value to the given updated value
 * if the current value {@code ==} the expected value.
 * 如果当前值 {@code ==} 是期望值，就原子将值设置为给定的更新值。
 *
 * @param expect the expected value (期望值)
 * @param update the new value （新值）
 * @return {@code true} if successful. False return indicates that
 * the actual value was not equal to the expected value. 
 * 成功返回 true，返回 false 表示实际值不等于预期值
 */
public final boolean compareAndSet(int expect, int update) {
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
}

// Unsafe 中的方法
public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);
```

调用 Unsafe.compareAndSwapInt() 方法实现，这个方法有四个参数：

1. `Object var1`：操作的对象。

2. `long var2`：对象中字段的偏移量

3. `int var4`：原来的值，即期望的值。

4. `int var5`：要修改的值。

可以看到，这是一个 native 方法，底层是使用 C/C++ 写的，主要是调用 CPU 的 CAS 指令来实现。它能够保证只有当对应偏移量处的字段值是期望值时才更新，即类似下面这样的两步操作：

```java
if(value == expect) {
    value = newValue;
}
```

通过 CPU 的 CAS 指令可以保证这两步操作是一个整体，从而不会出现多线程环境中可能比较的时候 value 值是 a，而到真正赋值的时候 value 值可能变成 b 了的问题。

## getAndIncrement() 方法

```java
/**
 * Atomically increments by one the current value.
 * 将当前值原子递增 1
 *
 * @return the previous value
 */
public final int getAndIncrement() {
    return unsafe.getAndAddInt(this, valueOffset, 1);
}

/**
 * Unsafe 中的方法
 */
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    
    return var5;
}
```
`getAndIncrement()` 方法底层调用的是 Unsafe 的 getAndAddInt() 方法，这个方法有三个参数：

1. `Object var1`: 操作的对象。

2. `long var2`：对象中字段的偏移量。

3. `int var4`：要增加的值。

通过查看 Unsafe 的 getAndAddInt() 方法的源码，可以看到它是先获取当前的值，然后再调用 compareAndSwapInt() 方法尝试更新对应偏移量处的值，如果成功了就跳出循环，如果不成功就再重新尝试，直到成功为止，这就是（CAS + 自旋）的乐观锁机制！！！

AtomicInteger 中的其他方法几乎都是类似的，最终会调用 Unsafe 的 compareAndSwapInt 来保证对 value 值更新的原子性。

总结

1. AtomicInteger 中维护了一个使用 volatile 修饰的变量 value，保证可见性。

2. AtomicInteger 中的主要方法最终几乎都会调用到 Unsafe 的 compareAndSwapInt() 方法保证对变量修改的原子性。

??? Question "为什么需要 AtomicInteger ?"

    看下面代码：
    ```java
    public class AtomicIntegerTest {
        private static int count = 0;

        public static void increment() {
            count++;
        }

        public static void main(String[] args) {
            IntStream.range(0, 100)
                    .forEach(i->
                            new Thread(()->IntStream.range(0, 1000)
                                    .forEach(j->increment())).start());

            // 这里使用2或者1看自己的机器
            // 我这里是用run跑大于2才会退出循环
            // 但是用 debug 跑大于1就会退出循环了
            while (Thread.activeCount() > 1) {
                // 让出CPU
                Thread.yield();
            }

            System.out.println(count);
        }
    }
    ```

    这里起来 100 个线程，每个线程对 count 自增 1000 次，可以发现每次运行的结果都不一样，但它们有个共同点就是都不到 100,000 次，所以直接使用 int 是有问题的。

    <font color=red>使用 volatile 能解决这个问题吗 ？</font>

    ```java
    private static volatile int count = 0 ;

    public static void increment() {
        count++;
    }
    ```

    答案很遗憾，volatile 无法解决这个问题，因为 volatile 仅有两个作用：
    
    - 保证可见性，即一个线程对变量的修改另一个线程立即可见。

    - 禁止指令重排序。

    这里有一个隐藏且重要的问题，实际上 count++ 是两步操作：

    1. 获取 count 的值。

    2. 对 count 的值加 1。

    使用 volatile 无法保证这两步不被其他线程调度打断的，所以无法保证原子性。

    从而就有了 AtomicInteger，它的自增调用的是 Unsafe 的 CAS 并使用自旋保证一定会成功，它可以保证两步操作的原子性。

    ```java
    public class AtomicIntegerTest {
        private static AtomicInteger count = new AtomicInteger(0);

        public static void increment() {
            count.incrementAndGet();
        }

        public static void main(String[] args) {
            IntStream.range(0, 100)
                    .forEach(i ->
                            new Thread(() -> IntStream.range(0, 1000)
                                    .forEach(j -> increment()))
                                    .start());

            // 这是使用 2 或 1 需要看自己的及其
            // 这里是用 run 跑大于 2 才会退出循环
            // 但是用 debug 跑大于 1 就会退出循环
            while (Thread.activeCount() > 2) {
                // 让出 CPU
                Thread.yield();
            }

            System.out.println("count = " + count);
        }
    }
    ```

    这里总会打印出 100000。

??? Question "AtomicInteger 的缺点 ？"

    著名的 ABA 问题 ！！！