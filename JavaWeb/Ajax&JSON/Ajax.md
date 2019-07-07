## Ajax+json

### Ajax

#### 作用：

​	在用户与服务器之间引入一个中间媒介，从而消除了网络交互过程中的处理—等待—处理—等待缺点

#### 缺陷：

1. 需要测试浏览器的支持
2. 网页的后退功能是失效的，需要明显的提示用户“数据已更新”
3. 手机设备，支持不是很友好

#### 对象（重要）

1. ##### XMLHttpRequest

   提供客户端同HTTP服务器通讯的协议

   ```js
   //IE
   var xmlHttpReq = new ActiveXObject("MSXML2.XMLHTTP.3.0");
   xmlHttpReq.open("GET", "http://localhost/books.xml", false);
   xmlHttpReq.send();
   alert(xmlHttpReq.responseText);
   //非IE
   var xmlHttpReq = new XMLHttpRequest();
   xmlHttpReq.open("GET", "http://localhost/books.xml", false);
   xmlHttpReq.send();
   alert(xmlHttpReq.responseText);
   ```

2. ##### onreadystatechange

   指定当readyState属性改变时的事件处理句柄

   ```JS
   var xmlhttp=null;
   function PostOrder(xmldoc)
   {
     var xmlhttp = new ActiveXObject("Msxml2.XMLHTTP.5.0");
     xmlhttp.Open("POST", "http://myserver/orders/processorder.asp", false); 
     xmlhttp.onreadystatechange= HandleStateChange;
     xmlhttp.Send(xmldoc);
     myButton.disabled = true;
   }
   function HandleStateChange()
   {
     if (xmlhttp.readyState == 4)
     {
       myButton.disabled = false;
       alert("Result = " + xmlhttp.responseXML.xml);
     }
   }
   ```

3. ##### status

   返回当前请求的http状态码

   ```js
   var xmlhttp = new ActiveXObject("Msxml2.XMLHTTP.3.0");
   xmlhttp.open("GET", "http://localhost/books.xml", false);
   xmlhttp.send();
   alert(xmlhttp.status);
   ```

   常用状态码以及含义：

   1. 404  没找到页面(not found)
   2. 403  禁止访问(forbidden)
   3. 500  内部服务器出错(internal service error)
   4. 200  一切正常(ok)
   5. 304 没有被修改(not modified)(服务器返回304状态，表示源文件没有被修改 )

4. ##### readyState

   返回XMLHTTP请求的当前状态

   ```js
   var XmlHttp;
   XmlHttp = new ActiveXObject("Msxml2.XMLHTTP.3.0");
   
   function send() {
      XmlHttp.onreadystatechange = doHttpReadyStateChange;
      XmlHttp.open("GET", "http://localhost/sample.xml", true);
      XmlHttp.send();
   }
   
   function doHttpReadyStateChange() {
      if (XmlHttp.readyState == 4) {
         alert("Done");
      }
   }
   ```

   备注：

   | 0 (未初始化)   |        对象已建立，但是尚未初始化（尚未调用open方法）        |
   | -------------- | :----------------------------------------------------------: |
   | 1 (初始化)     |                 对象已建立，尚未调用send方法                 |
   | 2 (发送数据)   |          send方法已调用，但是当前的状态及http头未知          |
   | 3 (数据传送中) | 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误 |
   | 4 (完成)       | 数据接收完毕,此时可以通过通过responseBody和responseText获取完整的回应数据 |

5. ##### open

   创建一个新的http请求，并指定此请求的方法、URL以及验证信息

   ```js
   var xmlhttp = new ActiveXObject("Msxml2.XMLHTTP.3.0");
   xmlhttp.open("GET","http://localhost/books.xml", false);
   xmlhttp.send();
   var book = xmlhttp.responseXML.selectSingleNode("//book[@id='bk101']");
   alert(book.xml);
   ```

   open()语法：

   ```js
   oXMLHttpRequest.open(bstrMethod, bstrUrl, varAsync, bstrUser, bstrPassword);
   ```

   1、bstrMethod
   	http方法，例如：POST、GET、PUT及PROPFIND。大小写不敏感。

   ​	 **如果用 POST 请求向服务器发送数据，需要将“Content-type” 的首部设置为“application/x-www-form-urlencoded”.它会告知服务器正在发送数据，并且数据已经 符合URL编码了**

   ```js
   ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
   ```

   2、bstrUrl
   	请求的URL地址，可以为绝对地址也可以为相对地址。 

   3、varAsync[可选]
   	布尔型，指定此请求是否为异步方式，默认为true。如果为真，当状态改变时会调用onreadystatechange属性指定的回调函数。 

   4、bstrUser[可选]
   	如果服务器需要验证，此处指定用户名，如果未指定，当服务器需要验证时，会弹出验证窗口。 

   5、bstrPassword[可选]
   	验证信息中的密码部分，如果用户名为空，则此值将被忽略。 

6. ##### send

   发送请求到http服务器并接收回应

   ```js
   xmlhttp = new ActiveXObject("Msxml2.XMLHTTP.3.0");
   xmlhttp.open("GET", "http://localhost/sample.xml", false);
   xmlhttp.send();
   alert(xmlhttp.responseXML.xml);
   ```

   1、open 方法定义了 Ajax 请求的一些细节。send 方法可为已经待命的请求发送指令

   2、data：将要传递给服务器的字符串。

   3、若选用的是 GET 请求，则不会发送任何数据， 给 send 方法传递 null 即可：request.send(null);

   4、当向send()方法提供参数时，要确保open()中指定的方法是POST，如果没有数据作为请求体的一部分发送，则使用null

### json

#### 方式1（利用js原生）

```js
 $.ajax({
        type:"post",//请求方式
        url:"data.jsp",//请求地址
        data:params,//传输的数据，参数
        success:function(data){//服务器返回的话（数据）
        var jdata = data.trim();//data.trim()是去除返回数据的空格
        jdata = eval("("+ jdata +")");
        }
 });
```

#### 方式2（利用jQuery）

```js
var object = $.parseJSON(data)
```









