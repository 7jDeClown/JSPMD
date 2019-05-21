[TOC]



#	jsp九大内置对象

## request作用



1. 获取请求参数
2. 解决中文乱码问题
3. 获取客户端数据
4. 应用request对象域

### request一般方法

```jsp
request.getParameter("xxx")
request.setCharacterEncoding("xxx")一般UTF-8

```

|          方法           |                             说明                             |
| :---------------------: | :----------------------------------------------------------: |
| getHeader(String name)  |                 获得HTTP协议定义的文件头信息                 |
| getHeaders(String name) | 返回指定名称的request Header的所有值，结果是一个枚举型的实例 |
|    getHeadersNames()    |     返回所有request Header的名称，结果是一个枚举型的实例     |
|       getMethod()       | 获得客户端向服务器端传送数据的方法，如get、post、header和trace等 |
|      getProtocol()      |         获得客户端向服务器端传送数据所依据的协议名称         |
|     getRequestURI()     |       获得发出请求字符串的客户端地址，不包括请求的参数       |
|     getRequestURL()     |                获取发出请求字符串的客户端地址                |
|      getRealPath()      |                  返回当前请求文件的绝对路径                  |
|     getRemoteAddr()     |                      获取客户端的IP地址                      |
|     getRemoteHost()     |                      获取客户端的主机名                      |
|     getServerName()     |                       获取服务器的名字                       |
|     getServerPath()     |             获取客户端所请求的脚本文件的文件路径             |
|     getServerPort()     |                      获取服务器的端口号                      |
|  request.getCookies()   |                  获取客户端保存的Cookie数据                  |

```jsp
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";

out.println("basePath:"+basePath);
out.println("<br/>");
out.println("getContextPath:"+request.getContextPath());
out.println("<br/>");
out.println("getServletPath:"+request.getServletPath());
out.println("<br/>");
out.println("getRequestURI:"+request.getRequestURI());
out.println("<br/>");
out.println("getRequestURL:"+request.getRequestURL());
out.println("<br/>");
out.println("getRealPath:"+request.getRealPath("/"));
out.println("<br/>");
out.println("getServletContext().getRealPath:"+getServletContext().getRealPath("/"));
out.println("<br/>");
out.println("getQueryString:"+request.getQueryString());

%>
结果
basePath:http://localhost:8080/test/

getContextPath:/test 
getServletPath:/test.jsp 
getRequestURI:/test/test.jsp 
getRequestURL:http://localhost:8080/test/test.jsp 
getRealPath:D:\Tomcat 6.0\webapps\test\ 
getServletContext().getRealPath:D:\Tomcat 6.0\webapps\test\ 
getQueryString:p=fuck
类似于postman查看的所有参数
 在Windows 7操作系统平台下，由于IP地址采用IPV6，所以当客户端与服务器为同一计算机时获取的IP地址并不是IPV4的形式
```

### request对象域

> 问题：每一次请求都是一个新的request，那么request域的作用范围是什么，在开发中什么时候使用?

请求转发----在服务器内部进行跳转。
这时我们这几个servlet就共享同一个request对象，这就是request域作用范围。

我们使用请求转发可以让多个servlet之间共享同一个request,那么我们如果想要在多个servlet之间进行信息传递，
可以使用setAttribute().

> 在开发中什么时候使用请求转发?
> 如果我们在request域中存储了信息，到其它的页面时，需要得到这个信息，这时我们就需要进行请求转发.


请求转发的代码

`request.getRequestDispatcher("路径").forward(request,response);`

> 请求转发与重定向的区别?

1.请求转发是服务器内部跳转，所有地址栏上的路径不会改变.
 重定向是浏览器在次发送请求，地址栏上的路径会发生改变.

2.请求转发只发送一次请求。
                          重定向会发送两次请求. 

3.请求转发只能在当前应用内部跳转.
 重定向可以在内部跳转也可以跳出当前应用.

4.请求转发时，因为是内部跳转。它的路径写法是   /资源路径。
                           重定向，它的路径需要写   /工程名/资源路径.  


5.请求转发，可以共享reqeust。
                          重定向不可能，因为每一次都是一个新的request。 

6.请求转换是通过reqeust发起  `request.getRequestDispatcher().forward();`

## reponse作用

1. 操作HTTP头信息
2. 设置MIME类型
3. 实现页面重定向

### 操作HTTP头信息

1. 禁用缓存

   ```jsp
<%
	response.setHeader("Cache-Control","no-store");
	response.setDateHeader("Expires",0);
%>
   ```

2. 设置页面自动刷新

```jsp
<%
	response.setHeader("refresh","10");
%>
```

3. 定时跳转网页

```jsp
<%
	response.setHeader("refresh","5;URL=login.jsp");
%>
```

### 设置MIME类型

一个JSP页面采用的内容类型是text/html，即HTML或文本数据。此值并不是固定的，可以根据开发需要动态更改响应类型，参考postman软件中的可选项

```jsp
<%response.setContentType(String type);
%>
type用于指定响应的内容类型，可选值为text/html、text/plain、application/x_msexcel和application/msword等

```

`说明：不同的Web容器定义MIME类型可能存在差异，其中Tomcat容器对MIME类型的声明定义在Tomcat根目录下的“conf\web.xml”文件中，通过<mime-mapping>标记声明。`

### 页面重定向

response对象提供的sendRedirect()方法可以将网页重定向到另一个页面，重定向操作支持将地址重定向到不同的主机上。在客户端浏览器上将会得到跳转的地址，并重新发送请求链接，`用户可以从浏览器的地址栏中看到跳转后的地址`。执行重定向操作后request中的属性全部失效，并且开始一个`新的request`对象

   

## out

### print和println方法

```jsp
<%
	out.print("xxxx");
%>
<%="xxx" %>
<pre>  //加此标签才会换行
<%
	out.println("xxx");
	out.println("xxx");
%>
</pre>

```

### 管理缓冲区

|    方    法     | 说    明                                     |
| :-------------: | -------------------------------------------- |
|     clear()     | 清除缓冲区中的内容                           |
|  clearBuffer()  | 清除当前缓冲区中的内容                       |
|     flush()     | 刷新流                                       |
|  isAutoFlush()  | 检测当前缓冲区已满时是自动清空，还是抛出异常 |
| getBufferSize() | 获取缓冲区的大小                             |

## session

### 生命周期

session作用于同一浏览器中，在各个页面中共享数据。`无论当前浏览器是否在多个页面间执行了跳转操作，整个用户会话一直存在。`直到关闭浏览器。如果在一个会话中客户端长时间不向服务器发出请求，session对象就会自动消失，这个时间取决于服务器。例如，Tomcat服务器默认为`30`分钟，这个时间可以通过编写程序修改.。

### 会话的基本操作

#### 创建

```jsp
session.setAttribute(String name,Object obj)
   name：指定作用域在session范围内的变量名。
   obj：保存在session范围内的对象
```

#### 获取

```jsp
getAtttibute(String name)  返回结果是Object
name指定保存在session范围内的关键字
```

#### 设置会话有效时间

```
设置session有效时间的方法
session.setMaxInactiveInterval(int time);time为秒
getLastAccessedTime()方法可以返回客户端最后一次与会话相关联的请求时间；
getMaxInactiveInterval()方法返回一个会话内两个请求最大时间间隔，以秒为单位。
手动销毁session ，invalidate()方法
 session对象被销毁后，将不可使用。如果在销毁后调用其任何方法，则抛出”Session already invalidated”异常
```

## aplication

### 生命周期

application对象在`服务器启动时自动创建，在服务器停止时销毁`。当application对象没有被销毁时，所有用户都可以共享它，生命最顽强.

### 操作application中数据

```jsp
application.setAttribute(String name,Object obj);
   name：指定应用程序环境属性的名称。
   obj：属性值，可以是任何Java数据类型。
application.getAttributeNames();
    getAttributeNames()方法用于获得所有application对象使用的属性名
application.getAttribute(String name);
    根据键获取值
removeAttribute(String name);
		移除指定名称的属性
```

### 配置Web应用初始化参数

```xml
<context-param> web.xml中配置初始化参数
	<param-name>home</param-name>
	<param-value>http://www.xxx.xxx</param-value>
</context-param>
```

```jsp
application.getInitParameter(String name);
		用于返回一下已命名的参数值
application.getInitParameter("xxxx");
		获取xxxx的值
application.getAttributeNames();
		返回所有已定义的应用程序初始化参数名的枚举
```

## pageContext对象

### 常用方法，加粗的方法非常重要

| 方    法                                  | 说    明                                                     |
| ----------------------------------------- | ------------------------------------------------------------ |
| forward(java.lang.String relativeUtlpath) | 把页面转发到另一个页面                                       |
| getAttribute(String name)                 | 获取参数值                                                   |
| getAttributeNamesInScope(int scope)       | 获取某范围的参数名称的集合，返回值为java.util.Enumeration对象 |
| **getException()**                        | 返回exception对象                                            |
| **getRequest()**                          | 返回request对象                                              |
| **getResponse()**                         | 返回response对象                                             |
| **getSession()**                          | 返回session对象                                              |
| **getOut()**                              | 返回out对象                                                  |
| **getApplication**                        | 返回application对象                                          |
| setAttribute()                            | 为指定范围内的属性设置属性值                                 |
| removeAttribute()                         | 删除指定范围内的指定属性                                     |

pageContext.request.contextPath

### 获取web.xml配置信息

config对象主要用于取得服务器的配置信息，通过pageContext对象的getServletConfig()方法可以获取一个config对象

| 方    法                | 说    明                                                     |
| ----------------------- | ------------------------------------------------------------ |
| getServletContext()     | 获取Servlet上下文                                            |
| getServletName()        | 获取Servlet服务器名                                          |
| getInitParameter()      | 获取服务器所有初始参数名称，返回值为java.util.Enumeration对象 |
| getInitParameterNames() | 获取服务器中name参数的初始值                                 |

## 其他对象

### page

| 方    法         | 说    明                       |
| ---------------- | ------------------------------ |
| getClass()       | 返回当前Object的类             |
| hashCode()       | 返回该Object的哈希代码         |
| toString()       | 把该Object类转换为字符串       |
| equals(Object o) | 比较该对象和指定的对象是否相等 |

### exception

exception对象用来处理JSP文件执行时发生的所有错误和异常，只有在page指令中设置为isErrorPage属性值为`true`的页面中才可以被使用，在一般的JSP页面中使用该对象将无法编译JSP文件

| 方    法              | 说    明                          |
| --------------------- | --------------------------------- |
| getMessage()          | 返回exception对象的异常信息字符串 |
| getLocalizedmessage() | 返回本地化的异常错误              |
| toString()            | 返回关于异常错误的简单信息描述    |
| fillInStackTrace()    | 重写异常错误的栈执行轨迹          |

