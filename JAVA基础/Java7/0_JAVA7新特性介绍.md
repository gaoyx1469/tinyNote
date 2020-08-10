#JAVA7

JAVA7LTS发布于2011年7月

1. 增加0B和0b前缀表示二进制数

2. 增加下划线_分割数字字面量，编译时自动去除下划线

3. 增加泛型

4. switch的case变量可以使用字符串了

5. 带资源的try语句，在try语句块退出或异常时自动调用资源的close方法，多个资源可用分号分隔，作用似乎跟finally里自己写close差不多。

6. catch中可同时捕获多个异常，异常类型用|分隔

7. 新增Path接口、Paths类、Files类、DirectoryStream接口、WatchService接口等文件操作处理工具类
    ```
        Path file = Paths.get("路径，绝对路径相对路径都行","文件名.扩展名");//获取路径
   
        byte[] bytes = Files.readAllBytes(file);//获取文件字节数组
        List<String> lines = Files.readAllLines(file);//按行读取文件至字符串集合
        Files.write(file,字节数组,StandardOption.APPEND);//文件追加
        Files.move(fromPath,toPath,StandardOption.REPLACE_EXISTING,StandardOption.COPY_ATTRIBUTES)//文件复制
   
        Path path = Paths.get("");
        DirectoryStream<Path> stream = Files.newDirectoryStream(path, "*.*")
        for (Path entry: stream) {
           //使用entry
           System.out.println(entry);
        }
        stream.close;

   ```
   
8. 增强泛型推断

9. NIO2.0  
    ByteBuffer、FileChannel类的使用
    
10. JSR292支持在JVM上运行动态类型语言，在字节码层面支持了InvokeDynamic  

11. fork/join计算框架  
    Java7提供的一个用于并行执行任务的框架，是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到大任务结果的框架。
    该框架为Java8的并行流打下了坚实的基础【Java8对其进行了大的改进，便于使用，这部分在Java8中介绍】