### servlet

#### 处理中文乱码问题

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

    

#### forward 与 sendRedirect

1. ​	重定向（sendRedirect）

   ```Java
resp.sendRedirect("path");
   ```

   ​	解决了表单重复提交的问题，以及当前servlet无法提交的问题
   
   ​	特点：
   
   1. ​	两次请求    两个request对象
   2. ​    浏览器地址栏信息改变
   
2. 请求转发（forward） 

   ```Java
   req.getRequestDispatcher("path").forward(req, resp);
   ```

   ​	地址栏信息不改变，会造成频繁请求地址。
   
3. 使用场景：

   1. 如果请求中有表单数据，而数据又比较重要，不能重复提交，建议使用重定向
   2. 如果请求被servlet接收后，无法进行处理，建议使用重定向定位到可以处理资源的servlet上