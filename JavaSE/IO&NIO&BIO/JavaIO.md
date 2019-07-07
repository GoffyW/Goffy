### JavaIO/NIO

### IO分类

在Java.io包下主要包括两种流输入、输出两种IO流，每种输入、输出流又分为字节流和字符流两大类。其中字节流以字节单位来处理输入、输出操作。而字符流则以字符处理输入、输出操作。

- 字符流
  - Reader
    - BufferedReader
    - InputStreamReader
    - StringReader
    - CharArrayReader
    - FileReader
    - ...
  - Writer
    - BufferedWriter
    -  OutputStreamWriter
    - StringWriter
    - CharArrayWriter
    - FileWriter
    - ...
  
- 字节流
  - InputStream
    - FileInputStream
    - StringBufferInputStream
    - ByteArrayInputStream
    - ...
  - OutputStream
    - FileOutputStream
    - StringBufferOutputStream
    - ByteArrayOutputStream
    - ...
  
  ### NIO
  
  Non-blocking I/O
  
  一种同步非阻塞的I/O模型，也是I/O多路复用的基础，已经被越来越多地应用到大型应用服务器，成为解决高并发与大量连接、I/O处理问题的有效方式
  
  核心对象：
  
  - Channel （通道）
  - Buffer （缓冲）
  
  