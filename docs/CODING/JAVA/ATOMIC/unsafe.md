# Unsafe

!!! Question

    1. Unsafe 是什么 ？

    2. Unsafe 只有 CAS 的功能吗 ？

    3. Unsafe 为什么是不安全的 ？

    4. 怎么使用 Unsafe ?

## 简介

Unsafe 提供了访问底层的机制，这种机制仅供 Java 核心类库使用，而不应该被普通用户使用。

为了更好了解 Java 的生态体系，学习并了解它是非常有必要的，不求深入到底层的 C/C++ 代码，但求了解它的基本功能。

## 获取 Unsafe 的实例

通过查看 Unsafe 的源码，我们可以发现它提供了一个 `getUnsafe()` 的静态方法。

```java
@CallerSensitive
public static Unsafe getUnsafe() {
    Class var0 = Reflection.getCallerClass();
    if (!VM.isSystemDomainLoader(var0.getClassLoader())) {
        throw new SecurityException("Unsafe");
    } else {
        return theUnsafe;
    }
}
```

如果直接调用这个方法会抛出一个 SecurityException 异常，这是因为 Unsafe 仅供 Java 内部类使用，外部类不应该使用它。

但是，通过 <font color=red size=5>反射</font> 可以拿到它。！因为它有一个属性叫做 <font color=red size=5>theUnsafe</font>，如下代码：

```Java
public class UnsafeTest {
    public static void main(String[] args)
            throws NoSuchFieldException, IllegalAccessException {
        Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
        theUnsafe.setAccessible(true);
        Unsafe unsafe = (Unsafe) theUnsafe.get(null);
    }
}
```

如果通过构造方法实例化这个类，age 属性将会返回 10。
```Java
User user = new User();
// 打印 10
System.out.println(user.age);
```

如果通过调用 Unsafe 实例化呢 ？

```Java
User user1 = (User) unsafe.allocateInstance(User.class);
// 打印 0
System.out.println(user1.age);
```

age 将返回 0， <font color=red> 因为 Unsafe.allocateInstance() 只会给对象分配内存，并不会调用构造方法，所以这里只会返回 int 类型的默认值 0</font>

![](https://shichuan-hao.github.io/images/static/java/java-unsafe-1.png){width="373" hight="324"}

## 修改私有字段的值

使用 Unsafe 的 `putXXX()`，可以修改任意私有字段的值。

```java
public class UnsafeTest {
    public static void main(String[] args)
            throws NoSuchFieldException, IllegalAccessException, InstantiationException {
        Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
        theUnsafe.setAccessible(true);
        Unsafe unsafe = (Unsafe) theUnsafe.get(null);

//        User user = new User();
//        System.out.println(user.age);

        User user = new User();
        System.out.println(user.getAge());
        Field age = user.getClass().getDeclaredField("age");
        unsafe.putInt(user, unsafe.objectFieldOffset(age), 20);
        // 打印 20
        System.out.println(user.getAge());
    }
}
```

一旦我们通过反射调用得到字段 age，我们就可以使用 Unsafe 将其值更改为任何其他 int 值（当然，这里也可以通过反射直接修改）。

## 抛出 checked 异常

众所周知，如果代码抛出了 checked 异常，要不就使用 `try...catch` 捕获它，要不就在方法签名上定义这个异常。但是，通过 Unsafe 我们就可以抛出一个 checked 异常，同时不用捕获或在方法签名上定义它。

```java
// 使用正常方式抛出IOException需要定义在方法签名上往外抛
public static void readFile()
        throws IOException {
    throw new IOException();
}
// 使用Unsafe抛出异常不需要定义在方法签名上往外抛
public static void readFileUnsafe()
        throws NoSuchFieldException, IllegalAccessException {
    Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
    theUnsafe.setAccessible(true);
    Unsafe unsafe = (Unsafe) theUnsafe.get(null);
    unsafe.throwException(new IOException());
}
```

## 使用堆外内存

如果进程在运行过程中 JVM 上的内存不足了，就会导致频繁的进行 GC。理想情况下，我们可以考虑使用<font color=red>堆外内存</font>，这是一块不受 JVM 管理的内存。

<font color=red> 使用 Unsafe 的 allocateMemory() 我们可以直接在堆外分配内存，这可能非常有用，但是要记住，这个内存不受 JVM 管理，因此我们要调用 freeMemory() 方法手动释放它。</font>

假设我们要在堆外创建一个巨大的 int 数组，我们可以使用 allocateMemory() 方法来实现：

```java
public class OffHeapArray {

    // 一个 int 等于 4 个字节
    private static final int INT = 4;
    private long size;
    private long address;

    private static Unsafe unsafe;

    static {
        try {
            Field f = Unsafe.class.getDeclaredField("theUnsafe");
            f.setAccessible(true);
            unsafe = (Unsafe) f.get(null);
        } catch (NoSuchFieldException | IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 构造方法，分配内存
     *
     * @param size 字节数
     */
    public OffHeapArray(long size) {
        this.size = size;
        // 参数字节数
        address = unsafe.allocateMemory(size * INT);
    }

    /**
     * 获取指定索引处的元素
     *
     * @param i 指定索引
     * @return 返回指定索引处的元素
     */
    public int get(long i) {
        return unsafe.getInt(address + i * INT);
    }

    /**
     * 设置指定索引处的元素
     * @param i 指定索引
     * @param value 元素
     */
    public void set(long i, int value) {
        unsafe.putInt(address + i * INT, value);
    }

    /**
     * 元素个数
     * @return 返回元素个数
     */
    public long size() {
        return size;
    }

    /**
     * 释放堆外内存
     */
    public void freeMemory() {
        unsafe.freeMemory(address);
    }
}
```

上面代码，在构造方法中调用 allocateMemory() 分配内存，在使用完成后调用 freeMemory() 释放内存。

使用方式如下：

```java
public static void main(String[] args) {
    OffHeapArray offHeapArray = new OffHeapArray(4);
    offHeapArray.set(0, 1);
    offHeapArray.set(1, 2);
    offHeapArray.set(2, 3);
    offHeapArray.set(3, 4);
    // 在索引 2 的位置重复放入元素
    offHeapArray.set(2, 5);
    // offHeapArray.set(4, 6);

    int sum = 0;
    for (int i = 0; i < offHeapArray.size ; i++) {
        sum += offHeapArray.get(i);
    }
    //
    System.out.println(sum);
    // 一定要记得调用 freeMemory() 将内存释放回操作系统
    offHeapArray.freeMemory();
}
```


## CompareAndSwap 操作

JUC 下面大量使用了 CAS 操作，它们的底层是调用 Unsafe 的 CompareAndSwapXXX() 方法。<font color=red>这种方式广泛运用在无锁算法，与 Java 中标准的悲观锁机制相比，它可以利用 CAS 处理器指令提供极大的加速。</font>

比如，我们可以基于 Unsafe 的 `compareAndSwapInt()` 方法构建线程安全的计数器。

```java
public class Counter {
    private volatile int count = 0;
    
    private static long offset;
    
    private static Unsafe unsafe;
    
    static {
        try {
            Field f = Unsafe.class.getDeclaredField("theUnsafe");
            f.setAccessible(true);
            unsafe = (Unsafe) f.get(null);
            offset = unsafe.objectFieldOffset(Counter.class.getDeclaredField("count"));
        } catch (NoSuchFieldException | IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }
    
    public void increment() {
        int before = count;
        // 失败了就重试直到成功为止
        while (!unsafe.compareAndSwapInt(this, offset, before, before + 1)) {
            before = count;
        }
    }
    
    public int getCount() {
        return count;
    }
}
```

在上面代码中，定义了一个 volatile 的字段 count，以便它的修改对所有线程都可见，并在类加载的时候获取 count 在类中的偏移地址。

在 increment() 方法中，通过调用 Unsafe 的 compareAndSwapInt() 方法来尝试更新之前获取的 count 的值，如果它没有被其它线程更新过，就更新成功，否则不断重试直到成功为止。

下面通过使用多个线程来测试上面的代码：

```java
public static void main(String[] args) throws InterruptedException {
    Counter counter = new Counter();
    ExecutorService threadPool = Executors.newFixedThreadPool(100);

    // 起 100 个线程，每个线程自增 1000 次
    IntStream.range(0, 100)
            .forEach(i -> threadPool.submit(() -> IntStream.range(0, 10000)
                    .forEach(j -> counter.increment())));

    threadPool.shutdown();

    Thread.sleep(2000);

    // 打印 1000000
    System.out.println(counter.getCount());

}
```

输入结果如下图所示：
![](https://shichuan-hao.github.io/images/static/java/java-unsafe-counter.png){width="373" hight="324"}

## park/unpark

JVM 在上下文切换的时候使用了 Unsafe 中的两个非常牛逼的方法 `park()` 和 `unpark()` 。

- 当一个线程正在等待某个操作时，JVM 调用 Unsafe 的 park() 方法来阻塞此线程。

- 当阻塞中的线程需要再次运行时，JVM 调用 Unsafe 的 unpark() 方法来唤醒此线程。

可以在 Java 集合的源码中看到大量的 LockSupport.park() 和 LockSupport.unpark()，它们底层都是调用的 Unsafe 的这两个方法

## 总结

使用 Unsafe 几乎可以操作一切

1. 实例化一个类。

2. 修改私有字段的值。

3. 抛出 checked 异常。

4. 使用堆外内存。

5. CAS 操作。

6. 阻塞/唤醒线程。


??? Question "实例化一个类的方式"

    1. 通过构造方法实例化一个类。
    
    2. 通过 Class 实例化一个类。
    
    3. 通过反射实例化一个类；
    
    4. 通过克隆实例化一个类；
    
    5. 通过反序列化实例化一个类；
    
    6. 通过 Unsafe 实例化一个类；

    ```java
    public class InstantiatedTest {

        private static final Unsafe unsafe;

        static {
            try {
                Field f = Unsafe.class.getDeclaredField("theUnsafe");
                f.setAccessible(true);
                unsafe = (Unsafe) f.get(null);
            } catch (NoSuchFieldException | IllegalAccessException e) {
                throw new RuntimeException(e);
            }
        }

        public static void main(String[] args) throws
                Exception {
            // 1. 构造方法
            User user1 = new User();

            // 2. Class，里面实际上也是反射
            User user2 = User.class.newInstance();

            // 3. 反射
            User user3 = User.class.getConstructor().newInstance();

            // 4. 克隆
            User user4 = user1.clone();

            // 5. 反序列化
            User user5 = deserialize(user1);

            // 6. Unsafe
            User user6 = (User) unsafe.allocateInstance(User.class);

            System.out.println("user1: " + user1.getAge());
            System.out.println("user2: " + user2.getAge());
            System.out.println("user3: " + user3.getAge());
            System.out.println("user4: " + user4.getAge());
            System.out.println("user5: " + user5.getAge());
            System.out.println("user6: " + user6.getAge());
        }

        public static User deserialize(User user) 
            throws IOException, ClassNotFoundException {
            Path path = Paths.get("object.txt");
            ObjectOutputStream oos = new ObjectOutputStream(Files.newOutputStream(path));
            oos.writeObject(user);
            oos.close();

            ObjectInputStream ois = new ObjectInputStream(Files.newInputStream(path));
            // 反序列化
            User user5 = (User) ois.readObject();
            ois.close();
            return user5;
        }
    }
    ```

    输出结果如下：

    ![InstantiatedTest 的输出结果](https://shichuan-hao.github.io/images/static/java/java-instantiatedclass-output.png)

    <font color=red>最后一种通过 Unsafe 实例化的类，里面的 age 的值是 0，而不是 10！这是因为调用 Unsafe 的 allocateInstance() 方法只会给对象分配内存，并不会初始化对象中的属性，所以 int 类型的默认值就是 0.</font>