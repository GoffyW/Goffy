##### 相对路径

1. 资源的位置不能随意更改
2. 需要使用../进行文件夹的跳出，使用比较麻烦

##### 绝对路径(开发常用)

​		 /虚拟项目名/项目资源路径

​		注意：在jsp中资源的第一个/表示的是服务器目录，相当于：localhost:8080

##### 使用jsp中的全局路径声明

```Java
<%
	String path = request.getContextPath();
	String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
```

```javascript
<base href="<%=basePath%>">
```

