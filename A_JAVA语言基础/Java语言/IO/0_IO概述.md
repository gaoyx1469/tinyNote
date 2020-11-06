# IO

### 一、IO是个啥

IO用于在设备间进行数据传输的操作



### 二、字节流

* `InputStream`字节输入流抽象类
  * `FileInputStream`文件输入流，从文件中读取字节流
  * `BufferedInputStream`缓冲输入流
* `OutputStream`字节输出流抽象类
  * `FileOutputStream`文件输出流，将字节流写入文件
  * `BufferedOutputStream`缓存输出流



### 三、字符流

* `Reader`字符输入流抽象类
  * `InputStreamReader`字符输入流
    * `FileReader`文件输入流，从文件中读取字符流
  * `BufferedReader`字符缓冲输入流
* `Writer`字符输出流抽象类
  * `OutputStreamWriter`字符输出流
    * `FileWriter`文件输出流，将字符流写入文件
  * `BufferedWriter`字符缓冲输出流



### 四、常见包装流

```
基本数据操作流：
DataInputStream：数据输入流允许应用程序以与机器无关方式从底层输入流中读取基本 Java 数据类型。包装InputStream。
DataOutputStream：数据输出流允许应用程序以适当方式将基本 Java 数据类型写入输出流中。包装OutputStream。
   
字节数组操作流：
ByteArrayInputStream：包含一个内部缓冲区，该缓冲区包含从流中读取的字节。构造参数为数组。
ByteArrayOutputStream：此类实现了一个输出流，其中的数据被写入一个 byte 数组。缓冲区会随着数据的不断写入而自动增长。可使用 toByteArray和 toString 获取数据。
   
字符数组操作流：
CharArrayReader：实现一个可用作字符输入流的字符缓冲区
CharArrayWrite：实现一个可用作 Writer 的字符缓冲区
   
字符串操作流程：
StringReader：一个字符流，可以用其回收在字符串缓冲区中的输出来构造字符串。关闭 StringWriter 无效。此类中的方法在关闭该流后仍可被调用，而不会产生任何 IOException。
StringWriter：源为一个字符串的字符流
   
标准输入输出流：
System.in的类型是InputStream
System.out的类型是PrintStream，是OutputStream的孙子
   
打印流：只操作目的地,不操作数据源。可启用自动刷新，在调用println()方法的时候，能够换行并刷新
PrintWriter

随机访问流：
RandomAccessFile：融合了InputStream和OutputStream的功能。支持对随机访问文件的读取和写入。

合并流：
SequenceInputStream：可以将多个输入流串流在一起，合并为一个输入流

序列化流：
ObjectOutputStream：对象输出流，将对象输出到内存
ObjectInputStream：从内存获取到对象数据，从而生成一个对象
```



