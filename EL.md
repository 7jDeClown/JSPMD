[TOC]



#	EL表达式

## 基本语法

```jsp
${expression}
   
```

​      `由于EL表达式的语法以“${”开头，所以如果在JSP网页中要显示“${”字符串，必须在前面加上\符号。即\${，或者写成${'${'}，就是用表达式来输出“${”符号串。`

## 特点

1. 可以与JSTL及JavaScript语句结合使用
2. 自动执行类型转换。如果想通过EL输入两个字符串型数值（如number1和number2）的和，可以通过+号连接（如`${number1+number2}`）
3. 不仅可以访问一般变量，而且还可以访问JavaBean中的属性，以及嵌套属性和集合对象
4. 可以执行算术运算、逻辑运算、关系运算和条件运算等
5. 可以获得命名空间（PageContext对象，它是页面中所有其他内置对象的最大范围的集成对象，通过它可以访问其他内置对象）
6. 在执行除法运算时，如果0作为除数，则返回无穷大Infinity，而不返回错误
7. 可以访问JSP的作用域（request、session、application及page）
8. 扩展函数可以与Java类的静态方法执行映射

## 禁用EL

### 具体方法

1.使用斜杠“\”符号

```jsp
\${expression}
感觉这个就是转义一下，页面输出$就是这种方式
说明：该方法仅适合禁用页面中一个或多个EL表达式的情况。
```

2.使用page指令

```jsp
<%@ page isELIgnored="布尔值" %> true为忽略
```

3.在web.xml文件中配置<el-ignored>元素

```xml
<jsp-config>
	<jsp-property-group>
		<url-pattern>*.jsp</url-pattern>
		<el-ignored>true</el-ignored><!--将此处的值设置为false，表示使用EL-->
	</jsp-property-group>
</jsp-config>
说明：
该方法适用于禁用Web应用中所有JSP页面中的EL
```

## 保留关键字

| 保留关键字 |       |       |      |
| ---------- | ----- | ----- | ---- |
| and        | eq    | gt    | true |
| instanceof | div   | or    | ne   |
| le         | false | empty | mod  |
| not        | lt    | ge    | null |

## EL运算符

### 访问数据

EL提供的 [] 和 . 运算符可以访问数据

通常情况下二者是等价的，可以相互代替

访问JavaBean对象userInfo的id属性

```jsp
 ${userInfo.id}
 ${userInfo[id]}
  


        当对象的属性名中包括一些特殊符号（-或.）时，只能使用[]运算符来访问对象的属性。${userInfo[user-id]}是正确的，而${userInfo.user-name}是错误的,建议使用[]
         el表达式并不能直接访问页面写的jsp中变量或者函数

```

### 算术运算

| 运  算  符           | 功  能                                             | 示    例             | 结    果 |
| -------------------- | -------------------------------------------------- | -------------------- | -------- |
| +                    | 加                                                 | ${19+1}              | 20       |
| -                    | 减                                                 | ${66-30}             | 36       |
| *                    | 乘                                                 | ${52.1*10}           | 521.0    |
| /或div               | 除                                                 | ${5/2}或${5 div 2}   | 2.5      |
| ${9/0}或${9 div 0}   | Infinity                                           |                      |          |
| %或mod               | 求余                                               | ${17%3}或${17 mod 3} | 2        |
| ${15%0}或${15 mod 0} | 将抛出异常java.lang.ArithmeticException: / by zero |                      |          |

### 逻辑关系

| 运  算  符 | 功    能 | 示    例                                          |
| ---------- | -------- | ------------------------------------------------- |
| ==或eq     | 等于     | ${10==10}或${10 eq 10}                            |
| !=或ne     | 不等于   | ${10!=10}或${10 ne 10} ${"A"!="A"}或${"A" ne "A"} |
| <或lt      | 小于     | ${7<6}或${7 lt 6} ${"A"<"B"}或${"A" lt "B"}       |
| >或gt      | 大于     | ${7>6}或${7 gt 6} ${"A">"B"}或${"A" gt "B"}       |
| <=或le     | 小于等于 | ${7<=6}或${7 le 6} ${"A"<="A"}或${"A" le "A"}     |
| >=或ge     | 大于等于 | ${7>=6}或${7 ge 6} ${"A">="B"}或${"A" ge "B"}     |

| 运  算  符 | 功    能 | 示    例                                                     |
| ---------- | -------- | ------------------------------------------------------------ |
| && 或 and  | 与       | ${true && false}或${true and false} ${"true" && "true"}或${"true" and "true"} |
| \|\| 或 or | 或       | ${true || false}或${true or false} ${false || false}或${false or false} |
| ! 或 not   | 非       | ${! true}或${not true} ${!false}或${not false}               |

### 条件关系

${条件表达式 ? 表达式1 : 表达式2}

## EL隐含对象(jsp对象)

### pageContext

​		用于访问JSP内置对象（如request、response、out、session、exception和page等，但不能用于获取`application、config和pageContext对象`）和servletContext。在获取这些内置对象后，即可获取其属性值。这些属性与对象的Getter方法相对应，在使用时去掉方法名中的get，并将首字母改为小写即可

```jsp
${pageContext.request} \\获取request对象
${pageContext.request.serverPort }\\通过request对象获得端口注意：
      不可通过pageContext对象获取保存在request范围内的变量。
${pageContext.response}
${pageContext.response.contentType }\\获取响应类型
${pageContext.out}
${pageContext.out.bufferSize }
${pageContext.session}
${pageContext.session.maxInactiveInterval} \\返回session有序时间
${pageContext.exception}
${pageContext.exception.message}
       在使用该对象时，也需要在可能出现错误的页面中指定错误处理页。并且在其中指定page指令的isErrorPage属性值为true，然后使用上面的EL输出异常信息
${pageContext.page}
${pageContext.page.class}
${pageContext.servletContext}
${pageContext.servletContext.contextPath}//获取服务器上路径，
```

### 访问作用域隐含对象

#### pageScope

1. pageScope隐含对象用于返回包含page（页面）范围内的属性值的集合，返回值为java.util.Map对象。

```jsp
${pageScope.user.name}  \\bean的ID为user，user对象中有name属性
```

2.  requestScope隐含对象用于返回包含request（请求）范围内的属性值的集合，返回值为java.util.Map对象

```jsp
<%request.setAttribute("userName","mr"); 		//定义request范围内的变量userName%>
${requestScope.userName}

```

3. sessionScope隐含对象用于返回包含session（会话）范围内的属性值的集合，返回值为java.util.Map对象

```jsp
<%session.setAttribute("manager","mr"); 		//定义session范围内的变量manager%>
${sessionScope.manager}
```

4. applicationScope隐含对象用于返回包含application（应用）范围内的属性值的集合，返回值为java.util.Map对象

```jsp
<%
//定义application范围内的变量message
application.setAttribute("message","欢迎光临无语阁聊天室！");
%>
${applicationScope.message}
```

### 访问环境信息对象

#### param对象

​		param对象用于获取请求参数的值，应用在参数值只有一个的情况。在应用param对象时，返回的结果为字符串。

```jsp
<input name="name" type="text">
${param.name}  //注意乱码
```

#### paramValues对象

```jsp
<input name="affect" type="checkbox" id="affect" value="登山">登山 
<input name="affect" type="checkbox" id="affect" value="游泳">游泳 
<input name="affect" type="checkbox" id="affect" value="慢走">慢走
<input name="affect" type="checkbox" id="affect" value="晨跑">晨跑
${paramValues.affect[0]}${paramValues.affect[1]}${paramValues.affect[2]}${paramValues.affect[3]}  //只会打印选择中的
```

#### header和headerValues对象

​		header对象用于获取HTTP请求的一个具体的header的值，但是在有些情况下，可能存在同一个header拥有多个不同的值，这时就必须使用headerValues对象。

```jsp
${header.connection}或${header["connection"]}
${header["user-agent"]}
${headerValues["Accept-Encoding"][0]}
```

#### initParam对象

​		initParam对象用于获取Web应用初始化参数的值。例如，在Web应用的web.xml文件中设置一个初始化参数author，用于指定作者

```xml
<context-param>
   <param-name>author</param-name>
   <param-value>mr</param-value>
</context-param>
```

####  cookie对象

​		el表达式中只能获取，不能设置

```jsp
<%Cookie cookie=new Cookie("user","mrbccd");
  response.addCookie(cookie);
%>
${cookie.user.value}
```

## EL函数

定义和使用函数包括以下3个步骤：

> （1）编写一个Java类，并在其中编写公用的静态方法，用于实现自定义EL函数的具体功能。
>
> （2）编写标签库描述文件声明函数，该文件的扩展名为“.tld”，保存在Web应用的WEB-INF文件夹下。
>
> （3）在JSP页面中引用标签库，并调用定义的EL函数实现相应的功能。

将标准库描述文件保存到WEB-INF文件夹下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<taglib xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee
	web-jsptaglibrary_2_0.xsd"
	version="2.0">
	<tlib-version>1.0</tlib-version>
	<uri>/xxx</uri>
	<function>
		<name>xxx</name>
		<function-class>完整包类名</function-class>
		<function-signature>返回类型 函数名(参数类型)
		</function-signature>
	</function>
</taglib>
   


   <uri>标记：用于指定tld文件的映射路径，在应用EL函数时需要使用该标记指定的内容。
   <name>标记：用于指定EL函数所对应方法的方法名，通常与Java文件中的方法名相同。
   <function-class>标记：用于指定EL函数所对应的Java文件，其中包括包名和类名。
   <function-signature>标记：用于指定EL函数所对应的静态方法，其中包括返回值和入口参数的类型，在指定这些类型时需要使用完整的类型名。例如，在上面的代码中不能指定该标记的内容为xxxx funcName(xxx)。
     
     <%@ taglib uri="/xxxx" prefix="xxx"%> //必须与自己写的相对应，有点想servlet
       ${xxx:funcName(xxxx)}  //
```



