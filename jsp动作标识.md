[TOC]

#	动作标识

##	<jsp:include>

```jsp
<jsp:include page="url" flush="false|true" />  //好像已经淘汰，电脑已运行就500错误，要我写name和value值
或：
<jsp:include page="url" flush="false|true" >
	子动作标识<jsp:param>
  //<jsp:param  name="" value="" />类似于跳转时url后面拼接了两个值
</jsp:include>
  这个会生成多个servelet实例
```

- page :指定被包含文件的相对路径。例如，指定属性值为“top.jsp”，则表示将与当前JSP文件相同文件夹中的top.jsp文件包含到当前JSP页面中
- flush：可选，设置是否刷新缓冲区，默认为false。如果设置为true，在当前页面输出使用缓冲区的情况下首先刷新缓冲区，然后执行包含操作

## jsp:forward

```jsp
<jsp:forward page="url"/>
或：
<jsp:forward page="url">
	子动作标识<jsp:param>
  // <jsp:param name="data" value="XXX"></jsp:param>
</jsp:forward>page
```

这种跳转URL地址栏`不变`

##	jsp:param

```jsp
<jsp:param name="参数名" value="参数值" />
（1）name属性：指定参数名称。
（2）value属性：设置对应的参数值。
说明：通过<jsp:param>动作标识指定的参数将以“参数名=值”形式添加到请求中，其功能与在文件名后面直接加“?参数名=参数值”相同
```

## jsp:useBean

```jsp
<jsp:useBean id = "name" class = "classname" scope = "page|request|ssesion|application" />
<jsp:setProperty name="变量名" property="*"/>
或者
<jsp:useBean id="变量名" scope="page|request|session|application" >
  <jsp:setProperty name="变量名" property="*"/>
</jsp:useBean>
```

- page : 该JavaBean实例只在该页面有效
- request: 该JavaBean实例只在本次请求有效
- session: 该JavaBean实例只在本次Session有效
- application  :该JavaBean实例在本次应用有效

## jsp:setProperty

```jsp
<jsp:setProperty name = "BeanName" property = "propertyName" value = "value" />
```

- name 属性确定需要设定的JavaBean的实例名
- property 属性确定需要设定的属性名 
- value 属性确定设定需要设置的属性值

## jsp:getProperty

```jsp
<jsp:getProperty name = "BeanName" property = "propertyName" />
```

