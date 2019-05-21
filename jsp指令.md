[TOC]

#	JSP指令标识

##	页面指令page

page指令基本格式

```jsp
<%@ page属性1="属性值1" 属性2="属性值2" …… %>

```

常见属性

- language 设置`语言`
- contenType 设置JSP页面`MIME类型和字符编码`
- pageEncoding 设置JSP页面的`编码格式`
- import 导包
- buffer 设置out对象缓冲区大小，默认8kb
- autoFlush 指定当缓冲区已满时自动将缓冲区中的内容输出到客户端，默认值为true。如果设置为false，当缓冲区已满时将抛出JSP Buffer overflow异常
- isErrorPage 将当前JSP页面设置为错误处理页面，以处理另一个JSP页面的错误，即异常处理。只有在错误处理页面中将isErrorPage属性设置为true时，才可以调用exception对象输出错误信息。
- errorPage 指定当前页面出现异常时调用的另一个页面（即错误处理页面），在错误处理页面中必须将isErrorPage属性值设置为true
- session 指定当前JSP页面是否支持session，默认值为true，即支持session
- isELIgnored 指定是否禁用EL表达式，如果为true，该页面将忽略EL表达式；否则将执行EL表达式
- isThreadSafe 指定JSP页面是否是线程安全的，如果为true，则表明JSP页面在同一时间可以被多个线程访问；否则不可

`注意：如果将buffer属性的值设置为none，则autoFlush属性不能设置为false。`

####	MIME

> 多用途互联网邮件扩展类型，MIME 消息能包含文本、图像、音频、视频以及其他应用程序专用的数据

- 超文本标记语言文本 .html,.html text/html
- 普通文本 .txt text/plain 
- RTF文本 .rtf application/rtf 
- GIF图形 .gif image/gif 
- JPEG图形 .ipeg,.jpg image/jpeg 
- au声音文件 .au audio/basic 
- MIDI音乐文件 mid,.midi audio/midi,audio/x-midi 
- RealAudio音乐文件 .ra, .ram audio/x-pn-realaudio
- MPEG文件 .mpg,.mpeg video/mpeg 
-  AVI文件 .avi video/x-msvideo 
- GZIP文件 .gz application/x-gzip
- TAR文件 .tar application/x-tar

## 文件包含指令include

```jsp
<%@ include file="path"%> 这个是静态包含。不过是几个前段页面的拼接
```

##	引用标签库指令taglib

```jsp
<%@ taglib prefix="tagPrefix" uri="tagURI" %>
（1）taglib属性：声明指令为taglib指令。
（2）prefix属性：指定标签库的前缀。一般为c，c标签用的多
（3）uri属性：指定标签库文件的位置。
```

