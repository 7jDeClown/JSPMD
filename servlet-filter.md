[TOC]

# Filter

## Filter接口

| 方 法 声 明                                                  | 说    明                                     |
| ------------------------------------------------------------ | -------------------------------------------- |
| public void init(FilterConfig filterConfig) throws ServletException | 过滤器初始化方法，此方法在初始化过滤器时调用 |
| public void doFilter ( ServletRequest request, ServletResponse response, FilterChain chain ) throws IOException, ServletException | 对请求进行过滤处理                           |
| public void destroy()                                        | 销毁方法以释放资源                           |

## FilterConfig接口

| 方 法 声 明                                 | 说    明                   |
| ------------------------------------------- | -------------------------- |
| public String getFilterName()               | 用于获取过滤器名           |
| public ServletContext getServletContext()   | 获取Servlet上下文          |
| public String getInitParameter(String name) | 获取过滤器的初始化参数值   |
| public Enumeration getInitParameterNames()  | 获取过滤器的所有初始化参数 |

## FilterChain接口

| 方法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public void doFilter ( ServletRequest request, ServletResponse response ) throws IOException, ServletException | 此方法用于将过滤后的请求传递给下一个过滤器，如果此过滤器是过滤器链中的最后一个过滤器，那么请求将传送给目标资源 |



```xml
 <filter>
  	<filter-name>myFilter</filter-name>
  	<filter-class>com.wxkj.login.action.ServletFilterAction</filter-class>
  	<init-param>
  		<param-name>userCount</param-name>
  		<param-value>0</param-value>
  	</init-param>
  </filter>
  <filter-mapping>
  	<filter-name>myFilter</filter-name>
  	<url-pattern>/login</url-pattern>
  </filter-mapping>
```

`说明：使用过滤器并不一定要将请求向下传递到下一过滤器或目标资源，如果业务逻辑需要，也可以在处理后直接回应客户端。`