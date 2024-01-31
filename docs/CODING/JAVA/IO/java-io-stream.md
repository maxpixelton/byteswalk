# Java IO 流

## 简介

## 字节流

### InputStream（字节输入流）

`InputStream` 用于从源头（通常是文件）读取数据（字节信息）到内存中。

`java.io.InputStream` 抽象类是所有字节输入流的父类。

`InputStream` 常用的方法如下：

```java

/**
 * 从输入数据流中读取下一个字节的数据。返回值在 0 到 255 范围内。
 * 如果由于到达数据流的末端而未读取到任何字节返回值是 -1，表示数据流结束。
 * 此方法会阻塞，直到输入数据可用、检测到数据流结束或出现异常为止。
 */
public abstract int read() throws IOException

/**
 *
 * 从输入流中读取一些字节存储到数组 b 中。
 * 
 * 如果数组 b 的长度为零，则不读取并返回 0。
 * 如果没有可用字节读取，返回 -1。
 * 如果有可用字节读取，则最多读取的字节数最多等于 b.length，返回读取的字节数。
 * 这个方法等价于 read(b, 0, b.length)。
 */
public int read(byte[] b) throws IOException

/**
 * 在read(byte b[ ]) 方法的基础上增加了 off 参数（偏移量）
 * 和 len 参数（要读取的最大字节数）
 */
public int read(byte[] b, int off, int len) throws IOException


/**
 * 忽略输入流中的 n 个字节, 返回实际忽略的字节数。
 */
public long skip(long n) throws IOException

/**
 * 返回输入流中可以读取的字节数。
 */
public int available() throws IOException

 /**
  * 关闭输入流释放相关的系统资源。
  */
public void close() throws IOException

/**
 * 将所有字节从一个输入流传递到一个输出流。
 */
public long transferTo(OutputStream out) throws IOException

/**
 * 读取输入流中的所有字节，返回字节数组。
 *
 * @since 9
 */
public byte[] readAllBytes() throws IOException

/**
 * 阻塞直到读取 len 个字节。
 */
public int readNBytes(byte[] b, int off, int len) throws IOException
```


#### FileInputStream（）

`FileInputStream` 是一个较为常用的字节输入流对象，可以直接指定文件路径、可以直接读取单字节数据、也可以读取到字节数组中如：



```java
try (FileInputStream fis = new FileInputStream("input.txt")) {
    System.out.println(STR."Number of remaining bytes: \{ fis.available() }");
    int content = 0;
    long skip = fis.skip(4);
    System.out.println(STR."The actual number of bytes skipped: \{ skip }");
    System.out.print("The content read from file: ");
    while ((content = fis.read()) != -1) {
        System.out.print((char)content);
    }
    System.out.println();
} catch (IOException e) {
    throw new RuntimeException(e);
}
```

#### BufferedInputStream (字节输入流)

大多数情况下，我们不会直接单独使用 `FileInputStream`，通常会配合 `BufferedInputStream`（字节缓冲输入流）



下面代码是 `FileInputStream` 配合 `BufferedInputStream` 使用：
```Java
public static void main(String[] args) {
    try {
        // 新建一个 BufferedInputStream 对象
        FileInputStream fileInputStream = new FileInputStream("input.txt");
        BufferedInputStream bufferedInputStream = new BufferedInputStream(fileInputStream);
        // 读取文件得内容并复制到 String 对象中
        String result = new String(bufferedInputStream.readAllBytes());
        System.out.println(result);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

#### DataInputStream

`DataInputStream` 用来读取指定类型数据，不能单独使用，比如集合其他流，如 `FileInputStream`。


```Java
public static void main(String[] args) {
    // 必须将 InputStream 作为构造参数才能使用
    try (DataInputStream dataInputStream = new DataInputStream(new FileInputStream("input.txt"))){
        // 可以读取任意具体的类型数据
        System.out.println(STR."Boolean type of data：\{dataInputStream.readBoolean()}");
        System.out.println(STR."Boolean type of data：\{dataInputStream.readInt()}");
        System.out.println(STR."Boolean type of data：\{dataInputStream.readUTF()}");
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

#### ObjectInputStream

`ObjectInputStream` 用于从输入流中读取 Java 对象（反序列化）， `ObjectOutputStream` 用于将对象写入到输出流(序列化)。

```Java
public static void read() {
    // 1. 创建对象流对象
    try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("object.data"))) {
        // 2. 读取对象
        System.out.println(ois.readInt());
        System.out.println(ois.readUTF());
        System.out.println((Date) ois.readObject());
        System.out.println((Person) ois.readObject());
    } catch (IOException | ClassNotFoundException e) {
        throw new RuntimeException(e);
    }
}
```

!!! tip

    __只有实现了 `Serializable` 接口的类的对象才能被序列化__。
    
    __如果对象的属性是对象，属性对应的类也必须实现 `Serializable` 接口__

    `Serialzable` 接口是一个空接口，只起到标记作用。

    如果对象中有属性不想被序列化，使用 `transient` 修饰


### OutputStream（字节输出流）


`OutputStream` 用来将数据（字节信息）写入到目的地（通常是文件）。

`java.io.OutputStream` 抽象类是所有字节输出流的父类。

 常用的方法有：

 - `write(int b)`: 将特定字节写入输出流。
 - `write(byte[] b)`：将数组 b 写入到输出流，等价于 `write(b, 0, b.length)`。
 - `write(byte[] b, int off, int len)`: 在 `write(byte[] b)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。
 - `flush()`: 刷新此输出流并强制写出所有缓冲的输出字节。
 - `close()`: 关闭输出流释放相关的系统资源。

#### FileOutputStream

 FileOutputStream 是最常用的字节输出流对象，同样可以直接指定文件路径、输出单字节数据、输出指定的字节数组。

 示例如下：

```Java
public static void main(String[] args) {
    try (FileOutputStream output = new FileOutputStream("output.txt")) {
        byte[] array = ("It's easy take me to your heart").getBytes();
        output.write(array);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```
运行结果：

![Image title]{https://shichuan-hao.github.io/images/static/java/java-io-fileoutputstream-restult.png}

#### BufferOutputStream

与 FileInputStream 一样，FileOutputStream 也是配合 BufferedOutputStream（字节缓冲输出流）来使用。

```Java
FileOutputStream fileOutputStream = new FileOutputStream("output.txt");
BufferedOutputStream bos = new BufferedOutputStream(fileOutputStream)
```

#### DataOutputStream

DataOutputStream 用来写入指定类型数据，不能单独使用，必须结合其他流，如 `FileOutputStream`：

```Java
// 输出流
FileOutputStream fileOutputStream = new FileOutputStream("out.txt");
DataOutputStream dataOutputStream = new DataOutputStream(fileOutputStream);
// 输出任意数据类型
dataOutputStream.writeBoolean(true);
dataOutputStream.writeByte(1);
```

#### ObjectOutputStream

`OjectInputStream` 将对象写入到输出流（`OjectInputStream`，序列化）。

```Java
/**
 * 使用对象输出流将数据写入文件
 */
public static void write() {
    // 1. 创建对象流对象
    try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("objectdata"))) {
        // 2. 写入对象，与写入顺序一致
        oos.writeInt(97);
        oos.writeUTF("李四");
        oos.writeObject(new Date());
        oos.writeObject(new Person("Tom", 23));
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

!!! tip

    1. 对象流不仅可以读写对象，还可以读写基本数据类型。

    2. 使用对象流读写对象时，该对象必须序列化与反序列化。

    3. 系统提供的类(如Date等)已经实现了序列化接口，自定义类必须手动实现序列化接口。

## 字符流

不管是文件读写还是网络发生接收，信息（数据）的最小存储单元都是字节，基于下面两个原因，I/O 流将操作分为 __字节流操作__ 和 __字符流操作__

1. 字符流是由 Java 虚拟机将字节转换得到的，这个过程比较耗时。
2. 在编码类型未知的情况下，容易出现乱码问题（乱码问题很容易复现，只需改成中文就可以了）：

因此，为方便对字符进行流操作，I/O 流提供了直接操作字符的接口。

字符流默认采用 `Unicode` 编码，也可以通过构造方法自定义编码，常用字符编码如 GBK、utf8 所在的字节数：

- __utf8__：英文占 1 个字节，中文占 3 个字节。
- __unicode__: 任何字符都占 2 个字节。
- __gbk__: 英文占 1 字节，中文占 2 字节。

### Reader（字符输入流）

#### InputStreamReader

### Writer（字符输出流）

## 字节缓冲流

### 字节缓冲输入流

### 字节缓冲输出流

## 字符传冲流

## 打印流

## 随机访问流

