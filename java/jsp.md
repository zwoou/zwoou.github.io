---
title: jsp
tags: jsp
---

## 1. 路径 ##
    ${pageContext.request.contextPath} 获得绝对路径
## 2. 9大内置对象 ##
    JSP共有以下9个内置的对象：
	request 用户端请求，此请求会包含来自GET/POST请求的参数
	response 网页传回用户端的回应
	pageContext 网页的属性是在这里管理
	session 与请求有关的会话期
	application servlet 正在执行的内容
	out 用来传送回应的输出
	config servlet的构架部件
	page JSP网页本身
	exception 针对错误网页，未捕捉的例外
<!--more-->
1. **request:** 表示HttpServletRequest对象。它包含了有关浏览器请求的信息，并且提供了几个用于获取cookie, header, 和session数据的有用的方法。

2. **response**表示HttpServletResponse对象，并提供了几个用于设置送回 浏览器的响应的方法（如cookies,头信息等）

3. **out**对象是javax.jsp.JspWriter的一个实例，并提供了几个方法使你能用于向浏览器回送输出结果。

4. **pageContext:**  表示一个javax.servlet.jsp.PageContext对象。它是用于方便存取各种范围的名字空间、servlet相关的对象的API，并且包装了通用的servlet相关功能的方法。

5. **session**表示一个请求的javax.servlet.http.HttpSession对象。Session可以存贮用户的状态信息

6. **applicaton** 表示一个javax.servle.ServletContext对象。这有助于查找有关servlet引擎和servlet环境的信息

7. **config**表示一个javax.servlet.ServletConfig对象。该对象用于存取servlet实例的初始化参数。

8. **page**表示从该页面产生的一个servlet实例
9. **exception**
## 3. HTTP中Get与Post的区别 ##
hppt定义了与服务器交互的不同方法，最基本的方法有4种，分别是get,post,put,delete.URL全称是资源描述符，用来描述一个网络上的资源。对应着查、改、增、删4个操作。  

- get一般获取/查询资源  
- post一般用于更新资源信息。
### 表面区别 ###
1. get请求的数据会附在url（也就是http协议头）之后以？分割URL和数据，参数之间用&相连。英文字母/数值，原样发送，如果是空格转换为+，中文或其他字符，直接把字符串用BASE64加密，16进制的ASC|| Post提交数据放在http包的包体中。
2. get方式提交的数据有限制，理论上URL没限制，但是各个浏览器或服务器有限制，或操作系统限制,Ie的限制是2083 （2K+35）
	post 没有限制
3. post安全性要高一点
##4. Cookie and Session的区别 ##
1. cookie 实际上是一小段的文本信息，客户端请求服务器，如果服务器需要记录该用户的状态，就使用response向客户端浏览器颁发一个Cookie。客户端会cookie保存起来。当浏览器再次请求该网站时，浏览器会把请求地址和Cookie的内容一同发给服务器，服务器检查该Cookie，从而判断用户的状态。服务器还可以修改Cookie内容。  
2. session是另一种记录客户状态的机制。session保存在服务器上  
3. 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
jsp html+java
## 5. jsp的原理和生命周期 ##
	jsp就是Servlet
	第一次访问会翻译，发生改变也会翻译
	init()    初始化JSP/Serviet的方法
	destroy() 销毁JSP/Serviet之前的方法
	service() 对用户请求生成响应的方法
      JSP文件输送到客户端是标准的HTML文件
 Annnotation来配置管理Web组件
## 6. 静态引入和动态引入的区别 ##

  1. 第一种：jstl  import
``` jsp
<c:import url="inlayingJsp.jsp"></c:import>
```
  2. jsp include指令
  include指令告诉容器：复制被包含文件汇总的所有内容，再把它粘贴到这个文件中。
  ``` jsp
  <%@ include file="inlayingJsp.jsp" %>
  ```
  3. 第三种：jsp include动作

  ``` jsp
  <jsp:include   page="inlayingJsp.jsp" flush="true"/>
  ```
  注意：(1)include指令在转换时插入“Header.jsp”的源代码，而<jsp:include>动作在运行时插入“Header.jsp"的响应。
                  <%@include为静态包含，<%@include不论包含的是txt文本还是jsp文件，被包含的页面都不会从新编译。
                  <%@include为静态包含,包含了几个JSP转译成servlet时就会有  几 个 class文件，如果在jsp1定义了变量i同时在jsp2也定义了变量i那么你编译都会通不过的,
                   jsp容器会告诉你i重复定义了.
                  <jsp:include 为动态包含，<jsp:include 如包含jsp文件，这每次加载主页面的时候，被包含的页面都要重新编译。
                    就是说不管你包含了几个jsp页面转译成servlet时中有一个class文件
参考：[https://blog.csdn.net/fn_2015/article/details/70311495](https://blog.csdn.net/fn_2015/article/details/70311495)

## jsp 语法 ##

1. 模板元素  
指jsp中的html，开发时先编写模版元素  
2. java脚本  
语法<%%>出现在jsp对应的servlet的servlet方法中  
3. java表达式<%=表达式%>不加分号  
4. java声明  
语法：<%!%>作用：定义jsp对应的成员变量和方法，不建议
JSP声明中独立存在的方法，只是一种假象  ，每个Servlet在容器中只有一个实例  单例设计模式
static{}注意：开发中很少用，面试和考试用  
5. 注释  
<!---->
<%----%>jsp引擎不会翻译 不会输出到客户端
6. 指令  
格式：<%@指令 属性....%>一般放在页面顶部
配置描述符 web.xml
     3个编译指令
       	1）page
            	属性
	            language="java"告知引擎，脚本用的是java
	extends:import:session:告知引擎是否产生HttpSession对象，默认true
	buffer:autoFlush="true"
	isThreadSafe
	errorPage:告知引擎，当前页面发生了异常进行转发
	配置全局错误页面
	isErrorPage:告知引擎，是否抓住了异常，如果该属性为true
	contentType:告知引擎，响应正文的MME类型
	pageEncoding 告知引擎，翻译jsp时，磁盘上所对应的码表
	el表达式
	2）include
	属性：file以"/"开头，代表当前应用
	3）taglib 引入外部标签
JSP的转发和包含
	1。forward jsp的内置标签
	2。param 不能单独使用
	包含 静态包含
	动态包含
	jsp隐含对象 九个
	jsp对应的Servlet的service方法的局部变量
	隐含对象 类型
	request javax.servlet http
	response
	session
	application
	config javax.servlet
	page javax.servlet.http.HttpServlet
	exception java.long.Throwable
	 out
 JSP的7个动作指令
	1，forward 执行页面转向 不会丢失请求参数
	2,param 用于传递参数
	3，include 用于动态引入一个JSP页面
	4，plugin 用于下载JavaBean或Applet到客户端执行
	5，uesBean 创建一个JavaBean实例
	   语法格式：<jsp:useBean id="name" class="classname" scope="page|request|session|application"/>
		id JavaBean的实例名
                class确定JavaBean的实现类
		scope 确定JavaBean实例的作用范围
			page 该JavaBean实例只在该页面有效
			request 该JavaBean实例在本次请求有效
			session 该JavaBean实例在本次session有效
			application 该JavaBean实例在本应用内一直有效
	6，setProperty 设置JavaBean实例的属性值
	7，getProperty 输出JavaBean实例的属性值
JSP的9个内置对象
	1，application :javax.servlet.ServletContext的实例，该实例代表JSP所属的Web应用本身
			项目启动始到项目关闭
			getRealPath() 返回相对路径的绝对路径
	2，config     该实例代表该JSP的配置信息
	3，exception  代表其他页面的异常和错误
	4，out        代表JSP页面的输出流
	5, page       代表该页面本身
	6，pageContext 代表该JSP页面的上下文，使用该页面可以访问页面中的共享数据

	7，request 该对象封装了一次请求，客户端的请求参数都被封装在该对象里
	8，response 代表服务器对客户端的响应
	9，session 代表一次会话
		常用方法
		        setAttribute(String attrName,Object attribute)
			getAttribute(String AttrName)
			removeAttribute()  删除
			getMaxInctiveInterval()  设置非活动时间

四大域对象
pageContext
   pageContext抽象类
	本身是一个域对象,还能操作其他3个域对象中的属性
     页面-请求-会话-应用
ServletRequest
Http
内置对象
    1.out对象
    out.print()  and out.println()
    2,request
	String getParameter(String name)
	String[] getParameterValues(String name) 多个值
	setCharacterEncoding("")设置编码格式 处理post请求乱码
    setAttribute(); 放置值 在转发中
    getAttribute();  取值


   跳转方式
      转发
	/*dis= request.getRequestDispatcher("跳转的页面xxxxx.jsp") 客户端地址栏不换 在服务器端跳转 转发前后页面，收到的请求相同
	dis.forward(request,response);*/
      重定向
     3，response
	sendRedirect("跳转的页面xxxxx.jsp")    地址栏是换的 在客户端  转发前后页面，收到的请求不相同

get请求和post请求  
	post请求安全 ，地址栏不显示用户提交的数据，数据大小不受限
	get请求不安全 ，地址栏显示用户提交的数据，数据大小不超过255字节
		解决乱码
			1,
			name=new String(name.getBytes("iso-8869-1"),"utf-8");
			2,
			改Tomcat的配置文件


1 内置对象session的用途  
session 对象是由服务器自动创建的与用户请求相关的对象。服务器为每个用户都生成一个session对 象，用于保存该用户的信息，跟踪用户的操作状态。

在购物类型的网站，如何在用户多次购买保存用户的操作记录
session对象内部使用Map类来保存数据，因此保存数据的格式为 “Key/value”。

2 cookie记录在哪里，常常用来记录什么样的数据
Cookie是Web服务器保存在用户硬盘上的一段文本。Cookie允许一个Web站点在用户电脑上保存信息并 且随后再取回它。
Cookie是以“关键字key=值value”的格式来保存记录的。  

	1. 创建cookie对象  
		Cookie cook=new Cookie(String name, String value);  
	2. 设置Cookie的生命周期
		cook.setMaxAge(24*3600);//24小时
	3. 向客户端写Cookie
		cook.addCookie()
	4. 取出cookie 取出的是数组
		Cookie[]    request.getCookies();  

3 application对象可以用来记录什么类型的数据

Application对象包括任何类型，包括队列和对象。

4 javaweb中的作用域象指的是那些，作用域和作用域对象有什么关系

1.page指当前页面有效。在一个jsp页面里有效

2.request 指在一次请求的全过程中有效，即从http请求到服务器处理结束，返回响应的整个过程， 存放在HttpServletRequest对象中。在这个过程中可以使用forward方式跳转多个jsp。在这些页面里 你都可以使用这个变量。

3.Session是用户全局变量，在整个会话期间都有效。只要页面不关闭就一直有效（或者直到用户一直 未活动导致会话过期，默认session过期时间为30分钟，或调用HttpSession的invalidate()方法）。 存放在HttpSession对象中

4.application是程序全局变量，对每个用户每个页面都有效。存放在ServletContext对象中。它的存 活时间是最长的，如果不进行手工删除，它们就一直可以使用

总结：当数据只需要在下一个forward有用时，用request就够了；
         若数据不只是在下一个forward有用时，就用session。
         上下文，环境信息之类的，用application。

具体使用方法：

page里的变量没法从index.jsp传递到test.jsp。只要页面跳转了，它们就不见了。  
request里的变量可以跨越forward前后的两页。但是只要刷新页面，它们就重新计算了。  
session的变量一直在累加，开始还看不出区别，只要关闭浏览器，再次重启浏览器访问这页， session里的变量就重新计算了。  
application里的变量一直在累加，除非你重启tomcat，否则它会一直变大。  
而作用域规定的是变量的有效期限。  
JavaWeb的四大作用域为：PageContext，ServletRequest，HttpSession，ServletContext；
1，超链接传数据
	<a href=""?name=""&name=""></a>
 取
 该
 存
jdbc:orcle:thin:@localhost: /orcl

文件上传
   客户端文件---》inputStream流------->网络传输---》服务器端--》输出流outStream
 第三方插件
	commons-upload
/* //创建xmlhttpRequest
	function createHttpRequest(){
		if(window.XMLHttpRequest){
			return new XMLHttpRequest();
		}else{
			return new ActiveXObject("Microsoft.XMLHTTP");
		}
	}
	var xmlreq;
	//ajax主函数
	function namechech(){
		var na=document.getElementById("uname").value;
		//四个步骤
		//1，创建xmlhttprequest
		xmlreq=createHttpRequest();
		//2,设置回调函数
		xmlreq.onreadystatechange=callback;

		//3,设置请求方式
		xmlreq.open("post","url",true);
		//4，向服务器端发送数据
		xmlreq.send();
		function callback(){
			//在回调中针对不同响应状态处理
			if(xmlreq.readyState==4&&xmlreq.status==200){
				  //获取服务器返回的数据
		        //获取纯文本数据
		        var responseText =xmlHttp.responseText;
		       document.getElementById("info").innerHTML = responseText;
			}
		}
	}*/
	$(function() {
		 $("#uname").change(function() {
			var inpo = $(this);
			var na = $(this).val();
			$.ajax({
				url : "isre",
				type : "post",
				data : "userName=" + na,
				success : function(con) {
					inpo.siblings("span").html(con);
				}
			});
		});

		$("#email").change(function() {
			var inf = $(this);
			var na = $(this).val();
			$.ajax({
				url : "EmailCheckService",
				type : "post",
				data : "email=" + na,
				success : function(con) {
					inf.siblings("span").html(con);
				}
			});
		});
	});
