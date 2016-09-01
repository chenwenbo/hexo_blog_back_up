title: Java一行一行读取文件
date: 2016-05-10 18:23:40
tags:
---
java 的I/O中有很多类，我们使用的时候经常容易混淆，下面有两种方法一行一行的读文件。

### 方法1：
```
private static void readFile1(File fin) throws IOException {
        FileInputStream fis = new FileInputStream(fin);
 
        //Construct BufferedReader from InputStreamReader
        BufferedReader br = new BufferedReader(new InputStreamReader(fis));
 
        String line = null;
        while ((line = br.readLine()) != null) {
                System.out.println(line);
        }
 
        br.close();}
```

### 方法2：
```
private static void readFile2(File fin) throws IOException {
        // Construct BufferedReader from FileReader
        BufferedReader br = new BufferedReader(new FileReader(fin));

        String line = null;
        while ((line = br.readLine()) != null) {
                System.out.println(line);
        }

        br.close();}
```

用下列代码：
//用.可以获取当前的路径
```
File dir = new File(".");File fin = new File(dir.getCanonicalPath() + File.separator + "in.txt");
readFile1(fin);
readFile2(fin);
```

两种方法都能一行一行读取文件，
两种方法的不同之处是使用的 BufferedReader的构造方法，方法1使用了 InputStreamReader  方法2使用了
FileReader，这两个类有什么不同之处呢？

在java文档中描述， InputStreamReader 是 字节流到字符流的一个过渡。
他能读字节和用特殊字符编码的字符数组， InputStreamReader 能够处理其他输入流文件，比如网络连接，资源类路径，zip文件等

FileReader  对于读取字符文件来说是一个很便利的类，这个类的构造方法包含 默认字符编码 和 默认的字节缓冲。 FileReader  不允许超过平台之外的编码，所以，将这个程序运行在其他平台编码不是一个好主意。

总结来说，相对于 FileReader来说， 使用InputStreamReader更好。
对于文件路径之间的连接，使用  File.separator 比 / \ 更好，因为  File.separator跨平台的，可以适应多种平台。

> 延伸阅读： [java io 类层级结构](http://www.programcreek.com/2012/05/java-io-class-hierarchy-diagram/)
