# CopyOnWriteArrayList 源码

## 简介

CopyOnWriteArrayList 是 ArrayList 的线程安全版本，内部也是通过数组实现，每次对数组的修改都完全拷贝一份新的数组来修改，修改完了再替换掉老数组，这样保证了只阻塞写操作，不阻塞读操作，实现读写分离。

## 继承体系

![CopyOnWriteArrayList 的继承关系](https://shichuan-hao.github.io/images/static/java/java-copyonwritearraylist-class-diagram.png)

`CopyOnWriteArrayList` 实现了 `List`、`Cloneable`、`RandomAccess`、`java.io.Serializable` 等接口，具备了以下能力：

1. 实现 `List`，提供了基础的添加、删除、遍历等操作。

2. 实现 `Cloneable`，可以被克隆。

3. 实现 `Serializable`，可以被序列化。

4. 实现 `RandomAccess`，提供了随机访问的能力。


## 源码解析

### 属性

```java
/** The lock protecting all mutators */
// 在修改时加锁
final transient ReentrantLock lock = new ReentrantLock();

/** The array, accessed only via getArray/setArray. */
// 真正存储元素的地方，只能通过 getArray()/setArray() 访问
private transient volatile Object[] array;
```

1. ___lock___：用于修改时加锁，使用 transient 修饰表示不自动序列化。

2. ___array___：真正存储元素的地方，使用 transient 修饰表示不自动序列化，使用 volatile 修饰表示一个线程对这个字段的修改另外一个线程立即看见。

???+ tip "为什么没有 size 字段 ？"

    因为每次修改都要拷贝一份正好可以存储目标个数元素的数组，所以不需要 size 属性。数组的长度就是集合的大小，而不像 ArrayList 数组的长度实际是要大于集合的大小的。

    比如，`add(E e)` 操作，先拷贝一份 n + 1 个元素的数组，再把新元素放到新数组的最后一位，这时新数组的长度为 len + 1 ，也就是集合的 size 了。

### 构造方法

#### CopyOnWriteArrayList()


创建空数组

```java

/**
 * Creates an empty list.
 * 创建空列表 list
 */
public CopyOnWriteArrayList() {
    // 所有对 array 的操作都通过 setArray() 和 getArray() 进行
    setArray(new Object[0]);
}

/**
 * Sets the array.
 */
final void setArray(Object[] a) {
    array = a;
}
```


#### CopyOnWriteArrayList(Collection<? extends E> c)

- 如果 c 是 CopyOnWriteArrayList 类型，直接将它的数组赋值给当前 list 的数组（这个是浅拷贝，两个集合共用同一个数组）。

- 如果 c 不是 CopyOnWriteArrayList 类型，则进行拷贝将 c 的元素全部拷贝到当前 list 的数组中。

```java
/**
 * Creates a list containing the elements of the specified
 * collection, in the order they are returned by the collection's
 * iterator.
 *
 * @param c the collection of initially held elements
 * @throws NullPointerException if the specified collection is null
 */
public CopyOnWriteArrayList(Collection<? extends E> c) {
    Object[] elements;
    // 如果 c 是 CopyOnWriteArrayList 类型，就直接将它的数组拿过来使用
    if (c.getClass() == CopyOnWriteArrayList.class)
        elements = ((CopyOnWriteArrayList<?>)c).getArray();
    else {
        // 如果 c 不是 CopyOnWriteArrayList 类型，将集合转化为数组
        elements = c.toArray();
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        // 这里 c.toArray 返回的不一定是 Object[] 类型（原因跟 ArrayList 的一样）
        if (elements.getClass() != Object[].class)
            elements = Arrays.copyOf(elements, elements.length, Object[].class);
    }
    setArray(elements);
}
```

#### CopyOnWriteArrayList(E[] toCopyIn)

将 toCopyIn 的元素拷贝给当前 list 的数组。

```java
/**
 * Creates a list holding a copy of the given array.
 *
 * @param toCopyIn the array (a copy of this array is used as the
 *        internal array)
 * @throws NullPointerException if the specified array is null
 */
public CopyOnWriteArrayList(E[] toCopyIn) {
    setArray(Arrays.copyOf(toCopyIn, toCopyIn.length, Object[].class));
}
```

### 常用方法

#### add(E e)

添加一个元素到 list 的末尾

```java
/**
 * Appends the specified element to the end of this list.
 * 添加指定元素到 list 的末尾
 *
 * @param e element to be appended to this list 要添加到 list 的元素 
 * @return {@code true} (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
        // 获取旧数组
        Object[] elements = getArray();
        int len = elements.length;
        // 将旧数组元素拷贝到新数组中
        // 新数组大小是旧数组大小加 1
        Object[] newElements = Arrays.copyOf(elements, len + 1);
        // 将元素放在最后一位
        newElements[len] = e;
        setArray(newElements);
        return true;
    } finally {
        // 释放锁
        lock.unlock();
    }
}
```

1. 加锁。

2. 获取元素数组。

3. 新建一个数组，大小为原数组长度加 1，并将原数组元素拷贝到新数组。

4. 把新添加的元素放到新数组的末尾。

5. 把新数组赋值给当前对象的 array 属性，覆盖原数组。

6.解锁。

#### add(int index, E element)

添加一个元素在指定索引处。

```java
/**
 * Inserts the specified element at the specified position in this
 * list. Shifts the element currently at that position (if any) and
 * any subsequent elements to the right (adds one to their indices).
 * 
 * 将指定元素插入到列表的指定位置。将当前位于该位置的元素（如果有）和后面的元素
 * 向右移动一位（在其索引中添加一个）
 *
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public void add(int index, E element) {
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
        // 获取旧数组
        Object[] elements = getArray();
        int len = elements.length;
        // 检查是否越界，可以等于 len
        if (index > len || index < 0)
            throw new IndexOutOfBoundsException("Index: " + index + ", Size: " + len);
        Object[] newElements;
        int numMoved = len - index;
        if (numMoved == 0)
            // 如果插入的位置是最后一个，就拷贝 n + 1 的数组，其前 n 个元素与旧数组一致。
            newElements = Arrays.copyOf(elements, len + 1);
        else {
            // 如果插入的位置不是最后一个，就新建一个 n + 1 的数组
            newElements = new Object[len + 1];
            // 拷贝旧数组前 index 的元素到新数组中
            System.arraycopy(elements, 0, newElements, 0, index);
            // 将 index 及其之后的元素往后挪一位拷贝到新数组中
            System.arraycopy(elements, index, newElements, index + 1,
                             numMoved);
        }
        // 将元素放置在 index 处
        newElements[index] = element;
        setArray(newElements);
    } finally {
        // 释放锁
        lock.unlock();
    }
}
```

1. 加锁

2. 检查索引是否合法，如果不合法抛出 `IndexOutOfBoundsException` 异常，注意这里 index 等于 len 是合法的 。

3. 如果索引等于数组长度（即数组最后一位再加 1），就拷贝一个 （len + 1） 的数组。

4. 如果索引不等于数组长度，就新建一个 len + 1 的数组，并按索引位置分成两部分，索引之前（不包含）的部分拷贝到新数组索引之前（不包含）的部分，索引之后（包含）的位置拷贝到新数组索引之后（不包含）的位置，索引所在位置留空。

5. 把索引位置赋值为待添加的元素。

6. 把新数组赋值给当前对象的 array 属性，覆盖原数组。

7. 解锁。

#### addIfAbsent(E e)

添加一个元素如果这个元素不存在于集合中。

```java
/**
 * Appends the element, if not present.
 * 
 *
 * @param e element to be added to this list, if absent
 * @return {@code true} if the element was added
 */
public boolean addIfAbsent(E e) {
    // 获取元素数组，取名为快照
    Object[] snapshot = getArray();
    // 检查元素是否存在，如果不存在直接返回 false
    return indexOf(e, snapshot, 0, snapshot.length) >= 0 ? false : addIfAbsent(e, snapshot);
}

/**
 * static version of indexOf, to allow repeated calls without
 * needing to re-acquire array each time.
 * @param o element to search for
 * @param elements the array
 * @param index first index to search
 * @param fence one past last index to search
 * @return index of element, or -1 if absent
 */
private static int indexOf(Object o, Object[] elements,
                           int index, int fence) {
    if (o == null) {
        for (int i = index; i < fence; i++)
            if (elements[i] == null)
                return i;
    } else {
        for (int i = index; i < fence; i++)
            if (o.equals(elements[i]))
                return i;
    }
    return -1;
}

/**
 * A version of addIfAbsent using the strong hint that given
 * recent snapshot does not contain e.
 */
private boolean addIfAbsent(E e, Object[] snapshot) {
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
        // 重新获取旧数组
        Object[] current = getArray();
        int len = current.length;
        // 如果快照与刚刚获取的数组不一致，说明有修改
        if (snapshot != current) {
            // Optimize for lost race to another addXXX operation
            // 重新检查元素是否在刚获取的数组里
            int common = Math.min(snapshot.length, len);
            
            for (int i = 0; i < common; i++)
                // 到这个方法里面了，说明元素不在快照里面
                if (current[i] != snapshot[i] && eq(e, current[i]))
                    return false;
            if (indexOf(e, current, common, len) >= 0)
                    return false;
        }
        // 拷贝一份 n + 1 的数组
        Object[] newElements = Arrays.copyOf(current, len + 1);
        // 将元素放在最后一位
        newElements[len] = e;
        setArray(newElements);
        return true;
    } finally {
        // 释放锁
        lock.unlock();
    }
}
```

1. 检查这个元素是否存在于数组快照中。

2. 如果存在直接返回 false，如果不存在调用 `addIfAbsent(E e, Object[] snapshot)` 处理。

3. 加锁。

4. 如果当前数组不等于传入的快照，说明有修改，检查待添加的元素是否存在于当前数组中，如果存在直接返回 false。

5. 拷贝一个新数组，长度等于原数组长度加 1，并将原数组元素拷贝到新数组中。

6. 把新元素添加到数组最后一位。

7. 把心数组赋值给当前对象的 array 属性，覆盖原数组。

8. 解锁。

#### get(int index)

获取指定索引的元素，支持随机访问，时间复杂度为 O(1)

```java
/**
 * {@inheritDoc}
 *
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public E get(int index) {
    // 获取元素不需要加锁
    // 直接返回 index 位置的元素
    // 这里没有做越界检查，因为数组本身会做越界检查
    return get(getArray(), index);
}

/**
 * Gets the array.  Non-private so as to also be accessible
 * from CopyOnWriteArraySet class.
 */
final Object[] getArray() {
    return array;
}

@SuppressWarnings("unchecked")
private E get(Object[] a, int index) {
    // 获取元素
    return (E) a[index];
}
```

1. 获取元素数组。

2. 返回数组指定索引位置的元素。

#### remove(int index)

```java

1. 删除指定索引位置的元素。

/**
 * Removes the element at the specified position in this list.
 * Shifts any subsequent elements to the left (subtracts one from their
 * indices).  Returns the element that was removed from the list.
 *
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public E remove(int index) {
    final ReentrantLock lock = this.lock;
    // 加锁
    lock.lock();
    try {
        // 获取旧数组
        Object[] elements = getArray();
        int len = elements.length;
        E oldValue = get(elements, index);
        int numMoved = len - index - 1;
        if (numMoved == 0)
            // 如果移除的是最后一位，就直接拷贝一份 n-1 的新数组，最后一位就自动删除了
            setArray(Arrays.copyOf(elements, len - 1));
        else {
            // 如果移除的不是最后一位，就新建一个 n-1 的新数组
            Object[] newElements = new Object[len - 1];
            // 将前 index 的元素拷贝到新数组中
            System.arraycopy(elements, 0, newElements, 0, index);
            // 将 index 后面（不包含）的元素往前挪一位，这样刚好将 index 位置覆盖掉了，相当于删除了
            System.arraycopy(elements, index + 1, newElements, index, numMoved);
            setArray(newElements);
        }
        return oldValue;
    } finally {
        // 释放锁
        lock.unlock();
    }
}
```

1. 加锁。

2. 获取指定索引位置元素的旧值。

3. 如果移除的是最后一位元素，就把原数组的前 len - 1 个元素拷贝到新数组中，并把新数组赋值给当前对象的数组属性。

4. 如果移除的不是最后一位元素，就新建一个 len - 1 长度的数组，并把原数组除了指定索引位置的元素全部拷贝到新数组中，并把新数组赋值给当前对象的数组属性。

5. 解锁并返回旧值。

#### size()

返回数组的长度

```java
/**
 * Returns the number of elements in this list.
 *
 * @return the number of elements in this list
 */
public int size() {
    // 获取元素个数不需要加锁
    // 直接返回数组的长度
    return getArray().length;
}
```

## 总结

1. `CopyOnWriteArrayList` 使用 ReentrantLock 重入锁加锁，保证线程安全。

2. `CopyOnWriteArrayList` 的写操作都要先拷贝一份新数组，在新数组中做修改，修改完了在用新数组替换老数组，所以空间复杂度是 O(n)，性能比较底下。

3. `CopyOnWriteArrayList` 的读操作支持随机访问，时间复杂度为 O(1)。

4. `CopyOnWriteArrayList` 采用读写分离的思想，读操作不加锁，写操作加锁，且写操作占用较大内存空间，所以适用于读多写少的场合。

5. `CopyOnWriteArrayList` 只保证最终一致性，不保证实时一致性。