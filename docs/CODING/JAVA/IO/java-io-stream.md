# Java IO 基础知识总结

IO 即 `Input/Output`，输入（数据输入到计算机内存的过程）和输出（数据输出到外部存储，如数据库、文件、远程主机...的过程）。数据传输过程类似于水流，因此称为 IO 流。

IO 流在 Java 中根据数据流向分为<strong>输入流</strong>和<strong>输出流</strong>，根据数据的处理方式分为 <strong>字节流</strong>和 <strong>字符流</strong>。

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来：

- `InputStream/Reader`：所有的输入流的基类，前者是字节输入流，后者是字符输入流。

- `OutputStream/Writer`：所有输出流的基类，前者是字节输出流，后者是字符输出流。

## 字节流

### InputStream（字节输入流）

抽象类 `java.io.InputStream` 用来从源头（通常是文件）读取数据（字节信息）到内存中，是所有字节输入流的父类。

常用方法：

- `read()`：返回输入流中下一个字节的数据。返回的值介于 0 到 255 之间。如果未读取任何字节，则代码返回 -1 ，表示文件结束。

- `read(byte b[ ])`：从输入流中读取一些字节存储到数组 b 中。如果数组 b 的长度为零，则不读取。如果没有可用字节读取，返回 -1。如果有可用字节读取，则最多读取的字节数最多等于 b.length ， 返回读取的字节数。这个方法等价于 read(b, 0, b.length)。

- `read(byte b[], int off, int len)`：在 `read(byte b[])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。

- `skip(long n)`：忽略输入流中的 n 个字节 ,返回实际忽略的字节数。

- `available()`：返回输入流中可以读取的字节数。

- `close()`：关闭输入流释放相关的系统资源。

从 Java 9 开始，`InputStream` 新增加了多个实用的方法：

- `readAllBytes()`：读取输入流中的所有字节，返回字节数组。

- `readNBytes(byte[] b, int off, int len)`：阻塞直到读取 len 个字节。

- `transferTo(OutputStream out)`：将所有字节从一个输入流传递到一个输出流。

#### FileInputStream

FileInputStream 是一个经常使用的字节输入流对象，可以直接指定文件路径，也可以直接读取单字节数据，还可以读取字节输出，示例：

```java
try (FileInputStream fis = new FileInputStream("input.txt")) {
    System.out.println("Number of remaining bytes：" + fis.available());

    int content = 0;
    long skip = fis.skip(2);
    System.out.println("The actual number of bytes skipped：" + skip);
    System.out.print("The content read from file：");
    while ((content = fis.read()) != -1) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

输出结果：

```
Number of remaining bytes：33
The actual number of bytes skipped：2
The content read from file：teswalk, Walking in the Bytes !
Process finished with exit code 0
```

不过，一般我们是不会直接单独使用 `FileInputStream` ，通常会配合 `BufferedInputStream`（字节缓冲输入流）来使用。比如下面代码是在项目中比较常见的，通过 `readAllBytes()`
读取输入流所有字节并将其直接赋值给一个 `String` 对象。

#### DataInputStream

用于读取指定类型数据，不能单独使用，必须结合其它流，比如 `FileInputStream` 。

```java
// 必须将fileInputStream作为构造参数才能使用
try (FileInputStream fis = new FileInputStream("input.txt");
    DataInputStream dis = new DataInputStream(fis)) {
    // 可以读取任意具体的类型数据
    // dis.readUTF();
    System.out.println(dis.readBoolean());
    System.out.println(dis.readInt());
    System.out.println(dis.readUTF());
} catch (IOException e) {
    if (e instanceof EOFException) {
        System.out.println("读取完毕");
    }
}
```

#### ObjectInputStream

用于从输入流中读取 Java 对象（反序列化），ObjectOutputStream 用于将对象写入到输出流(序列化)。

```java
ObjectInputStream input = new ObjectInputStream(new FileInputStream("object.data"));
MyClass object = (MyClass) input.readObject();
input.close();
```

另外，用于序列化和反序列化的类必须实现 `Serializable` 接口，对象中如果有属性不想被序列化，使用 `transient` 修饰。

### Outputstream（字节输出流）

`OutputStream` 用于将数据（字节信息）写入到目的地（通常是文件），`java.io.OutputStream` 抽象类是所有字节输出流的父类。

常用方法：

OutputStream 常用方法：

- `write(int b)`：将特定字节写入输出流。

- `write(byte b[ ])` : 将数组 `b` 写入到输出流，等价于 `write(b, 0, b.length)` 。

- `write(byte[] b, int off, int len)` : 在 `write(byte b[ ])` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字节数）。

- `flush()`：刷新此输出流并强制写出所有缓冲的输出字节。

- `close()`：关闭输出流释放相关的系统资源。

#### FileOutputStream

最常用的字节输出流对象，可直接指定文件路径，可以直接输出单字节数据，也可以输出指定的字节数组。

`FileOutputStream` 代码示例：

```java
try (FileOutputStream fos = new FileOutputStream("output.txt")) {
    byte[] array = "Byteswalk, Walking in the Bytes".getBytes();
    fos.write(array);
} catch (IOException e) {
    throw new RuntimeException(e);
}
```

与 FileInputStream 一样，FileOutputStream 通常也会配合 BufferedOutputStream（字节缓冲输出流）来使用。

```java
FileOutputStream fileOutputStream = new FileOutputStream("output.txt");
BufferedOutputStream bos = new BufferedOutputStream(fileOutputStream)
```


#### DataOutputStream

用于写入指定类型数据，不能单独使用，必须结合其它流，比如 FileOutputStream 。

```java
// 输出流
FileOutputStream fileOutputStream = new FileOutputStream("out.txt");
DataOutputStream dataOutputStream = new DataOutputStream(fileOutputStream);
// 输出任意数据类型
dataOutputStream.writeBoolean(true);
dataOutputStream.writeByte(1);
```

#### ObjectOutputStream

ObjectInputStream 用于从输入流中读取 Java 对象（ObjectInputStream,反序列化），ObjectOutputStream将对象写入到输出流(ObjectOutputStream，序列化)。

```java
ObjectOutputStream output = new ObjectOutputStream(new FileOutputStream("file.txt")
Person person = new Person("张三", "发大水发大水发生");
output.writeObject(person);
```

## 字符流

无论是文件读写还是网络发送接收，信息的最小存储单元都是字节，但是 I/O 流操作依然分为字节流操作和字符流操作，原因大体如下：

- 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时。

- 在不知道编码类型就很容易出现乱码问题。

乱码问题很容易就可以复现，只需要将上面的 FileInputStream 代码示例中的 input.txt 文件内容改为中文即可，原代码不需要改动。

可以很明显地看到读取出来的内容已经变成了乱码。因此，I/O 流就干脆提供了一个直接操作字符的接口，方便平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。字符流默认采用的是 Unicode 编码，我们可以通过构造方法自定义编码。顺便分享一下之前遇到的笔试题：常用字符编码所占字节数？`utf8` :英文占 1 字节，中文占 3 字节，`unicode`：任何字符都占 2 个字节，`gbk`：英文占 1 字节，中文占 2 字节。

### Reader（字符输入流）

用于从源头（通常是文件）读取数据（字符信息）到内存中，java.io.Reader抽象类是所有字符输入流的父类。

Reader 用于读取文本， InputStream 用于读取原始字节。

常用方法：

- `read()` : 从输入流读取一个字符。

- `read(char[] cbuf)` : 从输入流中读取一些字符，并将它们存储到字符数组 `cbuf` 中，等价于 `read(cbuf, 0, cbuf.length)` 。

- `read(char[] cbuf, int off, int len)`：在 `read(char[] cbuf)` 方法的基础上增加了 `off `参数（偏移量）和 `len` 参数（要读取的最大字符数）。

- `skip(long n)`：忽略输入流中的 n 个字符 ,返回实际忽略的字符数。

- `close()` : 关闭输入流并释放相关的系统资源。

#### InputStreamReader

该类是字节流转换为字符流的桥梁，其子类 FileReader 是基于该基础上的封装，可以直接操作字符。

```java
// 字节流转换为字符流的桥梁
public class InputStreamReader extends Reader {}

// 用于读取字符文件
public class FileReader extends InputStreamReader {}
```

FileReader 代码示例：

```java
try (FileReader fr = new FileReader("input.txt")) {
    int content;
    long skip = fr.skip(3);
    System.out.println(("The actual number of bytes skipped: " + skip));
    System.out.print("The content read from file:");
    while (((content = fr.read()) != -1)) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    throw new RuntimeException(e);
}
```

### Writer（字符输出流）

`Writer` 用于将数据（字符信息）写入到目的地（通常是文件），java.io.Writer 抽象类是所有字符输出流的父类。

常用方法：

- `write(int c)` : 写入单个字符。

- `write(char[] cbuf)`：写入字符数组 `cbuf`，等价于 `write(cbuf, 0, cbuf.length)`。

- `write(char[] cbuf, int off, int len)`：在 `write(char[] cbuf)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字符数）。

- `write(String str)`：写入字符串，等价于 `write(str, 0, str.length())` 。

- `write(String str, int off, int len)`：在 `write(String str)` 方法的基础上增加了 `off` 参数（偏移量）和 `len` 参数（要读取的最大字符数）。

- `append(CharSequence csq)`：将指定的字符序列附加到指定的 `Writer` 对象并返回该 `Writer` 对象。

- `append(char c)`：将指定的字符附加到指定的 `Writer` 对象并返回该 `Writer` 对象。

- `flush()`：刷新此输出流并强制写出所有缓冲的输出字符。

- `close()`: 关闭输出流释放相关的系统资源。

#### OutputStreamWriter

`OutputStreamWriter` 是字符流转换为字节流的桥梁，其子类 `FileWriter` 是基于该基础上的封装，可以直接将字符写入到文件。

```java
// 字符流转换为字节流的桥梁
public class OutputStreamWriter extends Writer {}

// 用于写入字符到文件
public class FileWriter extends OutputStreamWriter {}
```

`FileWriter` 代码示例：

```java
```

