### servlet

### 处理中文乱码问题

##### 请求中文乱码解决

1. 使用String进行数据重新编码

```Java
uname = new String(uname.getBytes("iso8859-1"),"utf-8");
```

 2. 使用公共配置

    - get方法：

      步骤一：

      ```Java
      request.setCharacterEncoding("utf-8");
      ```

      步骤二：

      在Tomcat的目录下的conf目录中的修改server.xml文件，在Connector标签中增加useBodyEncodingForURI="true"

    - post方法：

      ```Java
      resp.setContentType("text/html; charset=UTF-8");
      ```


### forward 与 sendRedirect

|                                  | 重定向   | 请求转发 |
| -------------------------------- | -------- | -------- |
| 第二次请求是谁请求的？           | 浏览器   | 服务器   |
| 浏览器发生了几次请求？           | 2        | 1        |
| servlet可以共享request吗？       | 不可以   | 可以     |
| 地址栏是否发生改变？             | 是       | 否       |
| 浏览器显示的是那一次的访问地址？ | 最后一次 | 第一次   |
| 可以跳转到什么资源               | 任意资源 | 项目内部 |
| 第二次请求路径是？               | 绝对路径 | 内部路径 |

1. ​	重定向（sendRedirect）

   ```Java
resp.sendRedirect("path");
   ```

   1. ​	两次请求    两个request对象，数据不共享
   2. ​    浏览器地址栏信息改变
   
2. 请求转发（forward） 

   ```Java
   req.getRequestDispatcher("path").forward(req, resp);
   ```

   1. 一次请求
   2. 浏览器地址不发生改变
   
3. 使用场景：

   1. 如果请求中有表单数据，而数据又比较重要，不能重复提交，建议使用重定向
   2. 如果请求被servlet接收后，无法进行处理，建议使用重定向定位到可以处理资源的servlet上
   
4. 选择

   1. 重定向的速度比转发慢，因为浏览器还得发出一个新的请求，如果在使用转发和重定向都无所谓的时候建议使用转发。
   2. 因为转发只能访问当前WEB的应用程序，所以不同WEB应用程序之间的访问，特别是要访问到另外一个WEB站点上的资源的情况，这个时候就只能使用重定向了

   [^注]: 重定向丢失数据，但在spring3.1之后，可以使用Flash解决重定向时传值丢失的问题

   