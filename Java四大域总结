# java四大域总结	

web部分，发现有些地方总是单个容易理解，可是把所有的放在一起来大杂烩，总是有那么几个知识点容易混淆。其实网上的资料已经够多了，虽然也不乏辛劳的搬运工。可是最终的目的不就是要我们自身理解吗？自己理解了的东西才正真是我们的。作为额外的奖励，我们先来关注一下JSP的九大隐式对象。

可以看看下图中关于JSP的九大隐式对象:
![alt](https://images2015.cnblogs.com/blog/955396/201607/955396-20160728213716091-1874759388.jpg)
## 作用域
顾名思义，起作用的大小范围也！如果是你自己去学习一个知识点，你要达到哪些目标才算是对一个知识点称得上是懂得，理解，掌握呢？从扁神医的望闻问切，到数据库的增删改查，所有的事物开始也不外乎：是什么？有什么用？怎么用？为什么可以这么用？故而，我觉得咱们应该把握以下问题，带着这些问题去学习，发现自己的不足，才是学习的上策。
- 作用域的实际大小(是什么)
- 作用域的作用(有什么用)
- 怎么使用这些作用域(怎么用)
- 它这样使用实现的原理(为什么可以这么用)

### 进行逐一分析:
#### <font color="red">servletcontext域（application域）

1. <font color = "blue">作用域的实际大小（是什么）			

	`servletcontext`域起作用的范围是：整个web应用程序。
数据产生之后，不仅等会还要用 ，还要给别人用，则请使用`servletcontext`。
它是四个域中范围最大的域。
2. <font color = "blue">作用域的作用（有什么用）

	由于一个web应用中的所有servlet共享同一个`servletcontext`对象，所以多个`servlet`通过`servletcontext`对象实现了数据在不同servlet之间的共享。

3. <font color = "blue">怎么使用这些作用域（怎么用）

a）可以通过编程的方式绑定，也可以作为web应用的全局变量被所有`Servlet`和`JSPs`访问
```java
	   设置Context属性：
        ServletContext application = this.getServletContext();
        application.setAttribute("person1", new Person("Jim"));
        application.setAttribute("person2", new Person("Green"));
       获取Context属性：
        ServletContext application = this.getServletContext();
        Enumberation persons = application.getAttributeNames();
        while (persons.hasMoreElements()) {
            String name = (String) persons.nextElement();
            Person p = (Person) persons.getAttribute(name);
            application.removeAttribute(name);
        }
 ```
b）为整个web应用配置context域：

　　修改web.xml配置文件，加入以下内容
```xml
　　<context -param>
　　　　<param-name>data</param-name>
　　　　<param-value>Hello world !</param-value>
　　</context - param>
```
从Servlet中访问这些初始化参数：
```java
    ServletContext application=this.getServletContext();
    out.println(application.getInitParameter("data"));
```
c）读取资源文件
   　　使用`ServletContext`接口可以直接访问web应用中的静态内容文档结构.包括HTML,GIF和JPEG文件。如以下方法:
            　　`.getResource()`
          　　`  .getResourceAsStream()`
   　　这两个方法的参数都是以`"/"`开头的字符串,表示资源相对于`context`根的相对路径.文档结构可以存在于服务器文件系统,或是war包中,或是在远程服务器上,抑或其他位置。不可以用来获得动态资源,比如`,getResource("/index.jsp")`,这个方法将返回该jsp文件的源码,而不是动态页面.可以用 `"Dispatching Requests"` 获得动态内容.列出web应用中可被访问的资源,可以使用`getResourcePaths(String path)`方法。

                                                   
4. <font color = "blue">它这样使用实现的原理或优缺点（为什么可以这么用）

	`servlet`并不适合做数据输出，故需要将数据转发给JSP文件进行美化再输出给客户端。
	
	JSP中可嵌入java代码，这使得它接收java数据变得可能。同时，由
于`servletcontext`域可使整个web应用共享该数据，因此而带来“线程安全”问题同样影响对转发的数据，故而需要使用request域。
#### <font color="red">Httpsession域（session域）
1. <font color = "blue">作用域的实际大小（是什么）

	`Httpsession`的作用域是：一次会话。

	数据产生之后显示给用户了，等会还要使用的情况下使用`Httpsession`域。

	它是四个域中范围第二大的域。
2. <font color = "blue">作用域的作用（有什么用）

	（会话范围）在第一次调用`request.getSession()`方法时，服务器会检查是否已经有对应的`session`。如果没有，就在内存中创建一个`session`并返回。当一短时间内（默认30分钟）`session`没有被使用，则服务器会销毁该`session`。若服务器非正常关闭，未到期的`session`也会跟着销毁。若调用`session`提供的`invalidate()`方法，可以立即销毁`session`。
3. <font color = "blue">怎么使用这些作用域（怎么用）
```java
	  a) jsp中操作session
　　　　(String)request.getSession().getAttribute("username"); // 获取
　　　　request.getSession().setAttribute("username", "xxx");  // 设置

 　　b) java中操作session
　　　　//servlet中
　　　　request.getSession();
　　　　session.getAttribute("username");
　　　　session.setAttribute("username", "xxx");
　　　　session.setMaxInactiveInterval（30*60）；
　　　　session.invalidate();
 
　　　　//struts中方法1
　　　　ServletActionContext.getRequest().getSession().setAttribute("username", "xxx");
　　　　ServletActionContext.getRequest().getSession().getAttribute("username");
　　　　ServletActionContext.getRequest().getSession().setMaxInactiveInterval(30*60);
　　　　ServletActionContext.getRequest().getSession().invalidate();

　　　　//struts中方法2
　　　　ActionContext.getContext().getSession().put("username", "xxx");
　　　　ActionContext.getContext().getSession().get("username");
　　　　ActionContext.getContext().getSession().clear();

　　　c) web.xml中操作session
　　　　<session-config>  
　　　　    <session-timeout>30</session-timeout>  
　　　　</session-config>

　　　d) tomcat-->conf-->conf/web.xml
　　　　<session-config>
　　　　    <session-timeout>30</session-timeout>
　　　　</session-config>
```
4. <font color = "blue">它这样使用实现的原理（为什么可以这么用）

	`HttpSession`在服务器中，为浏览器创建独一无二的内存空间，在其中保存了会话相关的信息。服务器创建`session`出来后，会把`session`的`id`号，以`cookie`的形式回写给客户机，这样，只要客户机的浏览器不关，再去访问服务器时，都 会带着`session`的`id`号去，服务器发现客户机浏览器带`session id`过来了，就会使用内存中与之对应的`session`为之服务。
#### <font color="red">ServletRequest域（request域）

1. <font color = "blue">作用域的实际大小（是什么）

	`ServletRequset`域是：整个请求链（请求转发也存在）。
	
	数据产生之后，只需要使用一次，这种情况下请使用`ServletRequset`域。

	它是四个域中范围排第三的域。
	
2. <font color = "blue">作用域的作用（有什么用）

	在整个请求链中共享数据。

	最常用到：在`servlet`中处理好的数据交给JSP显示，此时参数就可以放置在`ServletRequset`域中带过去。

3. <font color = "blue">怎么使用这些作用域（怎么用）

	a) 获得客户机信息的方法
　　   ` getRequestURL`方法返回客户端发出请求时的完整URL。
　　    `getRequestURI`方法返回请求行中的资源名部分。
　　    `getQueryString` 方法返回请求行中的参数部分。
　　    `getRemoteAddr`方法返回发出请求的客户机的IP地址
　　    `getRemoteHost`方法返回发出请求的客户机的完整主机名
　　   ` getRemotePort`方法返回客户机所使用的网络端口号
　　    `getLocalAddr`方法返回WEB服务器的IP地址。
　　   ` getLocalName`方法返回WEB服务器的主机名
　　    `getMethod`得到客户机请求方式
　　    
 b) 获得客户机请求头
　　  `  getHeader(string name)`方法
　　   ` getHeaders(String name)`方法
　　    `getHeaderNames`方法
　　    
c) 获得客户机请求参数(客户端提交的数据)
　　    `getParameter(name)`方法
　　    `getParameterValues（String name）`方法
　　    `getParameterNames`方法
　　   ` getParameterMap`方法
4. <font color = "blue">它这样使用实现的原理（为什么可以这么用）

	在`service`方法调用前由服务器创建，传入`service`方法。整个请求结束，`request`生命结束。

#### <font color="red">PageContext域（page域）

1. <font color = "blue">作用域的实际大小（是什么）

	`PageContext`域的作用范围是：整个JSP页面。
它是四个域中范围最小的一个域。
2. <font color = "blue">作用域的作用（有什么用）

	a）它可以获取其它八大隐式对象，可以认为它是一个入口对象。
	
	b） 获取其它所有域中的数据。
	
	c） 跳转到其它资源。其身上提供了`forword`和`sendRedirect`方法，简化了转发和重定向的操作
3. <font color = "blue">怎么使用这些作用域（怎么用）

	以下以一个简单的JSP页面程序为例：
```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"pageEncoding="UTF-8"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html><head><title>pageContext对象_例1</title></head>
<body><br>
<%
//使用pageContext设置属性，该属性默认在page范围内
pageContext.setAttribute("name","地球");
request.setAttribute("name","太阳系");
session.setAttribute("name","银河系");
//session.putValue("name","麦哲伦系");
application.setAttribute("name","宇宙");
%>
page设定的值:<%=pageContext.getAttribute("name")%><br>
request设定的值：<%=pageContext.getRequest().getAttribute("name")%><br>
session设定的值：<%=pageContext.getSession().getAttribute("name")%><br>
application设定的值：<%=pageContext.getServletContext().getAttribute("name")%><br>
范围1内的值：<%=pageContext.getAttribute("name",1)%><br>
范围2内的值：<%=pageContext.getAttribute("name",2)%><br>
范围3内的值：<%=pageContext.getAttribute("name",3)%><br>
范围4内的值：<%=pageContext.getAttribute("name",4)%><br>
<!--从最小的范围page开始，然后是reques、session以及application-->
<%pageContext.removeAttribute("name",3);%>
pageContext修改后的session设定的值：<%=session.getValue("name")%><br>
<%pageContext.setAttribute("name","宇宙",4);%>
pageContext修改后的application设定的值：<%=pageContext.getServletContext().getAttribute("name")%><br>
值的查找：<%=pageContext.findAttribute("name")%><br>
属性name的范围：<%=pageContext.getAttributesScope("name")%><br>
</body></html>
```
显示结果：
>page设定的值:地球
request设定的值：太阳系
session设定的值：银河系
application设定的值：宇宙
范围1内的值：地球
范围2内的值：太阳系
范围3内的值：银河系
范围4内的值：宇宙
pageContext修改后的session设定的值：null
pageContext修改后的application设定的值：宇宙
值的查找：地球
属性name的范围：1
4. <font color = "blue">它这样使用实现的原理（为什么可以这么用）
	
	`pageContext`对象，这个对象代表页面上下文，该对象主要用于访问JSP之间的共享数据。当对JSP的请求时开始，当响应结束时销毁。





	
