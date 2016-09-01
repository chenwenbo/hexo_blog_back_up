title: Java怎样将文件内容转换为字符串
date: 2016-05-10 18:27:15
tags:
---
怎么样将文件内容转换为字符串？


下面的代码做了这件事情，要是其运行的话，可能需要修改文件路径。
```
public static String readFileToString() throws IOException {
                File dirs = new File(".");
                String filePath = dirs.getCanonicalPath() + File.separator+"src"+File.separator+"TestRead.java";
 
                StringBuilder fileData = new StringBuilder(1000);//Constructs a string buffer with no characters in it and the specified initial capacity
                BufferedReader reader = new BufferedReader(new FileReader(filePath));
 
                char[] buf = new char[1024];
                int numRead = 0;
                while ((numRead = reader.read(buf)) != -1) {
                        String readData = String.valueOf(buf, 0, numRead);
                        fileData.append(readData);
                        buf = new char[1024];
                }
 
                reader.close();
 
                String returnStr = fileData.toString();
                System.out.println(returnStr);
                return returnStr;
        }
```

说是用String,好像也不是完全正确，准确的说，实际上是调用了java中的Stringbuffer .这里有一篇文章介绍了java String的不可变性，

Stringbuffer 和 Stringbuilder 都是可变的Strings.但是Stringbuffer在多线程环境中是线程安全的，文中使用Stringbuilder代替了Stringbuffer是因为在非多线程的环境下，StringBuilder更加快。
