# ArrayList 源码

## 简介

`ArrayList` 是一种以数组实现的 `List`，与数组相比，它具有 __动态扩展__ 的能力，因此也可称为 __动态数组__。

## 继承关系

![ArrayList 的继承关系](https://shichuan-hao.github.io/images/static/java/java-arraylist-class-diagram.png)

由上图可知， `ArrayList` 实现了 `List`、`Serializable`、`RandomAccess`、`Cloneable` 接口，继而具备了以下能力：

1. 实现 `List`，提供了基础的添加、删除、遍历等操作。

2. 实现 `RandomAccess`，提供了随机访问的能力。

3. 实现 `Cloneable`，可以被克隆。

4. 实现 `Seriablizable`，可以被序列化。

## 源码解析

### 属性

#### DEFAULT_CAPACITY

```Java title="源码"
/**
 * Default initial capacity.
 */
private static final int DEFAULT_CAPACITY = 10;
```

默认容量为 10，也就是通过 `new ArrayList()` 创建时的默认容量。

#### EMPTY_ELEMENTDATA

```Java title="源码"
/**
 * Shared empty array instance used for empty instances.
 */
private static final Object[] EMPTY_ELEMENTDATA = {};
```
空数组，当传入的容量为 0 时使用，即通过 `new ArrayList(0)` 创建时用的是这个空数组

#### DEFAULTCAPACITY_EMPTY_ELEMENTDATA

```Java title="源码"
/**
 * Shared empty array instance used for default sized empty instances. We
 * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
 * first element is added.
 */
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
```

同样也是空数组，是通过 `new ArrayList()` 创建时用的是这个空数组，与 `EMPTY_ELEMENTDATA` 的区别是。
在<font color=red>添加第一个元素时使用这个空数组会初始化为 DEFAUT_CAPACITY=10 个元素</font>


#### elementData

```Java title="源码"
/**
 * The array buffer into which the elements of the ArrayList are stored.
 * The capacity of the ArrayList is the length of this array buffer. Any
 * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
 * will be expanded to DEFAULT_CAPACITY when the first element is added.
 */
transient Object[] elementData; // non-private to simplify nested class access
```
真正存放元素的地方，使用 `transient` 是为了不序列化这个字段。

没有用 `private` 修饰，后面注释写的是 “为了简化嵌套类的访问”，但是实际上加了 `private` 嵌套类一样可以访问。

!!! info

    private 表示类私有的属性，只要是在这个类内部都可以访问，嵌套类或者内部类也是在类内部，所以也可以访问类的私有属性。

??? tip "elementData 设置成了 `transient`，`ArrayList` 是怎么把元素序列化呢 ？"

    ```java
    /**
     * Save the state of the <tt>ArrayList</tt> instance to a stream (that
     * is, serialize it).
     * 将 ArrayList 实例的状态保存到流中（即序列化）
     *
     * @serialData The length of the array backing the <tt>ArrayList</tt>
     *             instance is emitted (int), followed by all of its elements
     *             (each an <tt>Object</tt>) in the proper order.
     */
    private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException{
        // Write out element count, and any hidden stuff
        // 防止序列化期间有修改
        int expectedModCount = modCount;
        // 写出非 transient 和非 static 属性（会写出 size 属性）
        s.defaultWriteObject();

        // Write out size as capacity for behavioural compatibility with clone()
        // 写出元素个数
        s.writeInt(size);

        // Write out all elements in the proper order.
        // 依次写出元素
        for (int i=0; i<size; i++) {
            s.writeObject(elementData[i]);
        }

        // 如果有修改，抛出异常
        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }

    /**
     * Reconstitute the <tt>ArrayList</tt> instance from a stream (that is,
     * deserialize it).
     * 从流中重建 ArrayList（即反序列化）
     */
    private void readObject(java.io.ObjectInputStream s)
        throws java.io.IOException, ClassNotFoundException {

        // 声明为空数组
        elementData = EMPTY_ELEMENTDATA;

        // Read in size, and any hidden stuff
        // 读入非 transient 和非 static 属性（会读取 size 属性）
        s.defaultReadObject();

        // Read in capacity
        // 读取元素个数，没什么用，只是因为写出的时候写了 size 属性，读的时候也要按照顺序读
        s.readInt(); // ignored

        if (size > 0) {
            // be like clone(), allocate array based upon size not capacity
            // 计算容量
            int capacity = calculateCapacity(elementData, size);
            SharedSecrets.getJavaOISAccess().checkArray(s, Object[].class, capacity);
            // 检查是否需要扩容
            ensureCapacityInternal(size);

            Object[] a = elementData;
            // Read in all elements in the proper order.
            // 依次读取元素到数组中。
            for (int i=0; i<size; i++) {
                a[i] = s.readObject();
            }
        }
    }
    ```
    
    查看 `writeObject()` 方法可知，先调用 `s.defaultWriteObject();` 方法，再将 `size` 写入到流中，再把元素一个一个的写入流中。

    一般情况下，只要实现了 `Serializable` 接口即可自动序列化，`writeObject()` 和 `readObject()` 是为了自己控制序列化方式，这两个方法必须声明为 `private`，在 `java.io.ObjectOutputStreamClass#getPrivateMethod()` 方法中通过反射获取到 `writeObject()` 方法。

    在 `ArrayList` 的 `writeObject()` 方法中先调用了 `s.defaultWriteObject();` 方法，这个方法是写入非 `transient` 的属性，在 `ArrayList` 中也就是 `size` 属性。同样嘞，在 `readObject()` 方法中先调用了 `s.defaultReadObject();` 方法解析除了 `size` 属性。

    <font color=red> `elementData` 定义为 `transient` 的优势是 __根据 `size` 序列化真实的元素，而不是根据数组的长度序列化元素，减少了空间占用__ 。</font>

#### size

```Java title="源码"
/**
 * The size of the ArrayList (the number of elements it contains).
 * ArrayList 包含的元素个数
 * @serial
 */
private int size;
```

真正存储元素的个数，而不是 `elementData` 数组的长度。

??? tip "为什么用 size 存储元素的而是，而不是 elementData 数组的长度 ？"

    在 Java 中，ArrayList 是一个动态数组实现的容器，它允许在运行时动态地增加或减少容器的大小。
    
    ArrayList 内部使用一个数组 elementData 来存储元素，而 size 则是用来记录 ArrayList 中实际存储的元素个数。

    选择使用 size 来存储元素的数量而不是直接使用 elementData 数组的长度，主要是为了更好地支持动态扩展和收缩。以下是一些原因：
    
    1. __动态大小__： ArrayList 允许在运行时添加或删除元素，而不需要提前定义数组的大小。使用 size 来记录实际元素数量可以更方便地追踪和管理容器的大小变化。
    
    2. __避免浪费空间__： 如果直接使用数组的长度来表示元素数量，可能会导致数组的实际长度比元素数量多很多，造成空间的浪费。而使用 size 可以精确地表示当前存储的元素数量，避免浪费空间。
    
    3. __方便抽象__： size 提供了一个抽象的接口，用于获取容器中的元素数量。这种抽象使得对容器大小的操作更加直观和方便。
    
    4. __支持可变大小__： ArrayList 支持动态地增加或减少容器的大小，而不需要手动调整数组的大小。使用 size 可以更方便地进行大小的调整。

    总的来说，使用 size 来存储元素数量是为了提供更灵活、更高效的动态数组实现。这使得 ArrayList 能够在运行时自动管理内部数组的大小，而不需要用户手动处理数组大小的问题。

### 构造方法

#### ArrayList(int initialCapacity)

```Java title="源码"
/**
 * Constructs an empty list with the specified initial capacity.
 * 用指定的初始容量构建一个空列表
 *
 * @param  initialCapacity  the initial capacity of the list（列表的初始容量）
 * @throws IllegalArgumentException if the specified initial capacity
 *         is negative 如果指定的初始容量是负数就抛出异常
 */
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        // 如果传入的初始容量大于 0，就新建一个数组存储元素
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        // 如果传入的初始容量等于 0，就使用空数组 EMPTY_ELEMENTDATA
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        // 如果传入的初始容量小于 0，就抛出异常
        throw new IllegalArgumentException("Illegal Capacity: "+ initialCapacity);
    }
}
```

传入初始容量：

- 如果大于 0，就初始化 elementData 为对应的大小。

- 如果等于 0，就使用 EMPTY_ELEMENT 空数组。

- 如果小于 0，抛出异常。

#### ArrayList()

```Java title="源码"
/**
 * Constructs an empty list with an initial capacity of ten.
 */
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

不传入初始容量，初始化为 DEFAULTCAPACITY_EMPTY_ELEMENTDATA 空数组。<font color=red>在添加第一个元素的时候扩容为默认的大小，即 DEFAULT_CAPACITY=10</font>

#### ArrayList(Collection<? extends E> c)

```Java title="源码"
/**
 * Constructs a list containing the elements of the specified
 * collection, in the order they are returned by the collection's
 * iterator.
 * 构建包含指定集合元素的列表 list，按照集合迭代器返回顺序
 *
 * @param c the collection whose elements are to be placed into this list 将元素放入 list 的集合
 * @throws NullPointerException if the specified collection is null 如果集合是空则抛出空指针异常
 */
public ArrayList(Collection<? extends E> c) {
    // 集合转数组
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        // 检查 c.toArray() 返回的是不是 Object[] 类型，如果不是，重新拷贝成 Object[].class 类型
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // replace with empty array.
        // 如果指定集合 c 为空集合，就初始化为 EMPTY_ELEMENTDATA
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

传入集合并初始化 elementData，这里会使用 __拷贝__ 将传入集合的元素 __拷贝__ 到 elementData 数组中，如果元素个数为 0 ，就初始化为 EMPTY_ELEMENTDATA 空数组。

??? tip "为什么 `c.toArray();` 返回的有可能不是 Object[] 类型 ？"

    ```java
    public class App {
        public static void main(String[] args) {
            Father father = new Son();
            // 打印结果为 class cn.byteswalk.yuanma.Son
            System.out.println(father.getClass());

            MyList strings = new MyList();
            // 打印结果为 class [Ljava.lang.String;
            System.out.println(strings.toArray().getClass());
        }
    }

    public class Father {}

    public class Son extends Father{}

    public class MyList extends ArrayList<String> {
        
        /**
         * 子类重写父类的方法，返回值可能不一样
         * 但这里只能用数组类型，换成 Object 就不行
         * 应该是 java 本身的 bug
         */
        @Override
        public String[] toArray() {
            return new String[]{"1", "2", "3"};
        }
    }
    ```

### 常用方法

#### add(E e)

```java title="源码"
/**
 * Appends the specified element to the end of this list.
 *
 * @param e element to be appended to this list
 * @return <tt>true</tt> (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

private void ensureExplicitCapacity(int minCapacity) {
    modCount++;
    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

/**
 * Increases the capacity to ensure that it can hold at least the
 * number of elements specified by the minimum capacity argument.
 *
 * @param minCapacity the desired minimum capacity
 */
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}
```

#### add(int index, E element)

```Java title="源码"
    /**
     * Inserts the specified element at the specified position in this
     * list. Shifts the element currently at that position (if any) and
     * any subsequent elements to the right (adds one to their indices).
     *
     * @param index index at which the specified element is to be inserted
     * @param element element to be inserted
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }

        /**
     * A version of rangeCheck used by add and addAll.
     */
    private void rangeCheckForAdd(int index) {
        if (index > size || index < 0)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
```

#### addAll(Collection<? extends E> c)

```Java title="源码"
    /**
     * Appends all of the elements in the specified collection to the end of
     * this list, in the order that they are returned by the
     * specified collection's Iterator.  The behavior of this operation is
     * undefined if the specified collection is modified while the operation
     * is in progress.  (This implies that the behavior of this call is
     * undefined if the specified collection is this list, and this
     * list is nonempty.)
     *
     * @param c collection containing elements to be added to this list
     * @return <tt>true</tt> if this list changed as a result of the call
     * @throws NullPointerException if the specified collection is null
     */
    public boolean addAll(Collection<? extends E> c) {
        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // Increments modCount
        System.arraycopy(a, 0, elementData, size, numNew);
        size += numNew;
        return numNew != 0;
    }
```

#### get(int index)

```Java title="源码"
    /**
     * Returns the element at the specified position in this list.
     *
     * @param  index index of the element to return
     * @return the element at the specified position in this list
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E get(int index) {
        rangeCheck(index);

        return elementData(index);
    }

        /**
     * Checks if the given index is in range.  If not, throws an appropriate
     * runtime exception.  This method does *not* check if the index is
     * negative: It is always used immediately prior to an array access,
     * which throws an ArrayIndexOutOfBoundsException if index is negative.
     */
    private void rangeCheck(int index) {
        if (index >= size)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }

        @SuppressWarnings("unchecked")
    E elementData(int index) {
        return (E) elementData[index];
    }

```

#### remove(int index)


```Java title="源码"
        /**
     * Removes the element at the specified position in this list.
     * Shifts any subsequent elements to the left (subtracts one from their
     * indices).
     *
     * @param index the index of the element to be removed
     * @return the element that was removed from the list
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
```

#### remove(Object o)

```Java title="源码"
    /**
     * Removes the first occurrence of the specified element from this list,
     * if it is present.  If the list does not contain the element, it is
     * unchanged.  More formally, removes the element with the lowest index
     * <tt>i</tt> such that
     * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>
     * (if such an element exists).  Returns <tt>true</tt> if this list
     * contained the specified element (or equivalently, if this list
     * changed as a result of the call).
     *
     * @param o element to be removed from this list, if present
     * @return <tt>true</tt> if this list contained the specified element
     */
    public boolean remove(Object o) {
        if (o == null) {
            for (int index = 0; index < size; index++)
                if (elementData[index] == null) {
                    fastRemove(index);
                    return true;
                }
        } else {
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }

        /*
     * Private remove method that skips bounds checking and does not
     * return the value removed.
     */
    private void fastRemove(int index) {
        modCount++;
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work
    }
```

#### retainAll(Collection<?> c)

```Java title="源码"
    /**
     * Retains only the elements in this list that are contained in the
     * specified collection.  In other words, removes from this list all
     * of its elements that are not contained in the specified collection.
     *
     * @param c collection containing elements to be retained in this list
     * @return {@code true} if this list changed as a result of the call
     * @throws ClassCastException if the class of an element of this list
     *         is incompatible with the specified collection
     * (<a href="Collection.html#optional-restrictions">optional</a>)
     * @throws NullPointerException if this list contains a null element and the
     *         specified collection does not permit null elements
     * (<a href="Collection.html#optional-restrictions">optional</a>),
     *         or if the specified collection is null
     * @see Collection#contains(Object)
     */
    public boolean retainAll(Collection<?> c) {
        Objects.requireNonNull(c);
        return batchRemove(c, true);
    }

        private boolean batchRemove(Collection<?> c, boolean complement) {
        final Object[] elementData = this.elementData;
        int r = 0, w = 0;
        boolean modified = false;
        try {
            for (; r < size; r++)
                if (c.contains(elementData[r]) == complement)
                    elementData[w++] = elementData[r];
        } finally {
            // Preserve behavioral compatibility with AbstractCollection,
            // even if c.contains() throws.
            if (r != size) {
                System.arraycopy(elementData, r,
                                 elementData, w,
                                 size - r);
                w += size - r;
            }
            if (w != size) {
                // clear to let GC do its work
                for (int i = w; i < size; i++)
                    elementData[i] = null;
                modCount += size - w;
                size = w;
                modified = true;
            }
        }
        return modified;
    }
```

#### removeAll(Collection<?> c)


```Java title="源码"
    /**
     * Removes from this list all of its elements that are contained in the
     * specified collection.
     *
     * @param c collection containing elements to be removed from this list
     * @return {@code true} if this list changed as a result of the call
     * @throws ClassCastException if the class of an element of this list
     *         is incompatible with the specified collection
     * (<a href="Collection.html#optional-restrictions">optional</a>)
     * @throws NullPointerException if this list contains a null element and the
     *         specified collection does not permit null elements
     * (<a href="Collection.html#optional-restrictions">optional</a>),
     *         or if the specified collection is null
     * @see Collection#contains(Object)
     */
    public boolean removeAll(Collection<?> c) {
        Objects.requireNonNull(c);
        return batchRemove(c, false);
    }

```

## 总结