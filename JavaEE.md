### HTTP协议

应用层协议,无连接无状态

#### Http协议版本

+ Http1.0 响应完成后连接直接断开
+ Http1.1 keep-alive 可以在一段时间内保持连接不断开,多次请求按顺序响应
+ Http2.0 多路复用 对http头进行压缩

### Servlet

#### 定义

运行在服务端的Java小程序，SUN公司规范接口，处理客户端请求响应

#### 三大组件

1. servlet
2. filter 过滤器
3. listener 监听器

#### 实现

1. 创建类实现Servlet接口
2. 实现未实现方法
3. 在web.xml进行配置

#### 方法

1. init : servlet对象创建时执行
2. service : 每次请求都会执行
3. destroy : servlet销毁时执行

#### Servlet生命周期

1. 创建 : 默认第一次访问时创建
2. 销毁 : 服务器关闭时销毁

```java
public class MyServer implements Servlet {
  
    /* init
     * servlet对象创建时执行
     * 默认第一次访问servlet时创建servlet对象
     * ServletConfig: 该servlet对象的配置信息
     */
	public void init(ServletConfig config) throws ServletException {
	}
  
	/* service
	 * 每次请求都会执行
	 * ServletRequest: 内部封装http请求信息
	 * ServletResponse: 要封装的是http响应的信息
	 */
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
	}
  
	/* destroy
     * servlet对象销毁时执行
     * 服务器关闭时servlet销毁
     */
	public void destroy() {
	}

	public ServletConfig getServletConfig() {
		return null;
	}

	public String getServletInfo() {
		return null;
	}	
}
```

#### servlet配置

```xml
<servlet>
  <servlet-name>名称</servlet-name>
  <servlet-class>全类名</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>名称</servlet-name>
  <url-pattern>虚拟路径</url-pattern>
</servlet-mapping>
```

##### url-pattern配置方式

1. 完全匹配
2. 目录匹配 : 虚拟路径/*
3. 扩展名匹配 : *.扩展名
4. 缺省配置 : / 当没有匹配时默认访问
  2与3不能混用

优先级 : 完全匹配 > 目录匹配 > 扩展名匹配

##### 配置服务器启动时创建对象

`<load-on-startup>3</load-on-start>`

web应用中所有的资源的响应都是servlet负责，包括静态资源。

#### Servlet3.0配置

```java
@WebServlet(
  name = "类名",
  urlPatterns = {"虚拟路径"},
  loadOnstart = 3;
)
```

### ServletContext

#### 定义

代表一个web应用的环境（上下文）对象，内部封装web应用信息，一个web应用只有一个ServletContext对象，有多个Servlet对象

##### 生命周期

​	创建 : 该web应用被加载（服务器开启）
​	销毁 : web应用被卸载 （服务器关闭）

#### 获取

```java
// 1.
ServletContext servletContext = config.getServletContext();
// 2.
ServletContext servletContext = this.getServletContext();
```

#### ServletContext作用

1. 获得web应用全局的初始化参数
   `getInitParameter()`

2. 获得web应用中任何资源的绝对路径（重要）
  `getRealPath(相对路径)` 该api不同的web容器实现不同

  `this.getClass.getClassLoader.getResource(文件).getPath()` 所有web服务器通用

3. 获得文件MIME类型

   getMimeType(文件名)

4. ServletContext是一个域对象（重要）
  作用范围 : 整个web应用，所有的web资源都可以随意向ServletContex域中存取数据，数据可以共享。

### 域对象

#### 通用方法

```java
serAttribute(String name,Object obj);
Object getAttribute(String name);
removeAttribute(String name);
```

### response

```java
// 设置响应行
// 设置状态码
setStatus(302);
/*
 * 常见状态码
 * 200 正常
 * 302 重定向
 * 304 拿本地缓存
 * 404 没有该资源
 * 500 服务器端错误
 */

// 设置响应头
addHeader(String name,String value);
addIntHeader(String name,int value);
addDateHeader(String name,long date);

setHeader(String name,String value);
setIntHeader(String name,int value);
setDateHeader(String name,long date);
```

### 重定向

特点：

1. 访问服务器两次
2. 地址栏的地址发生变化

```java
// 1.
setStatus(302);
setHeader("location",重定向地址);

// 2.
sendRedirect(重定向地址);

// 3.
setHeader("refresh", "秒;url=重定向地址");
```

### 编码

```java
// 1.客户端
response.setContentType("text/html;charset=utf-8");
// 2.
response.setCharacterEncoding("utf-8");
```

### URL编码

将中文进行getBytes编码,得到字节数组,将每个字节转成十六进制,将每个十六进制用%号拼接

### 文件下载

```java
// 1.获取文件名
request.getParameter(get参数);

// 2.设置文件类型到响应头
response.setContentType(getServletContext().getMimeType(文件名));

// 3.设置客户端解析方式
response.setHeader("Content-Disposition", "attachment;filename="+文件名);

// 4.获取文件地址
getServletContext().getRealPath(文件);

// 5.通过IO流写入
response.getOutputStream().writer();
```

### request

#### 获取请求行

```java
// 1.获取请求方式 GET POST
request.getMethod();

// 2.获取URI
request.getRequestURI();

// 3.获取URI
request.getRequestURL();

// 4.获取web应用名称
request.getContextPath();

// 5.获取GET提交的参数
request.getQueryString();
```

#### 获取客户信息

```java
request.getRemoteAddr();
request.getRemoteHost();
request.getRemotePort();
request.getRemoteUser();
```

#### 获取请求头

```java
request.getHeader(String name);

// 常见请求头
referer 获取该次访问的来源
```

#### 获得请求体

```java
request.getParameter(String name);
request.getParameterValues(String name);
Enumeration getParameterNames();
Map getParameterMap();
```

#### request 域

作用范围：一次请求中

####  转发

```java
RequesDispatcher dispatcher request.getRequestDispatcher(地址);
dispatcher.forward(request,response);
```



#### ServletContext域与Request域的生命周期比较

ServletContext:

​	创建：服务器启动

​	销毁：服务器关闭

​	域作用的范围：整个web应用

request:

​	创建：访问时创建request

​	销毁：响应结束request销毁

​	域的作用范围：一次请求中

#### 转发与重定向的区别

1. 重定向两次请求，转发一次请求
2. 重定向地址栏的地址变化，转发地址不变
3. 重定向可以访问外部网站，转发只能访问内部资源
4. 转发性能优于重定向

#### 设置POST提交编码

request.setCharacterEncoding("utf-8");

#### Cookie

```java
// 1.创建Cookie
Cookie cookie = new Cookie(键,值);

// 2.发送Cookie
response.addCookie(cookie);
// Cookie中不能有中文

// 3.设置Cookie
// 设置cookie存储时间,默认为会话
cookie.setMaxAge(秒);

// 设置携带路径,默认为该cookie所在资源的路径
cookie.setPath(路径);	

// 4.删除Cookie
// 发送同名同携带路径储存时间为0Cookie
```

#### Session

也是一个域对象

借助cookie存储JSESSIONID

##### 获取方法

```java
/*
 * 方法内部会判断该客户端是否在服务器端已经存在session
 * 如果不存在，就创建一个session，并返回
 * 如果存在，返回该session
 */
request.getSession()
```

##### session生命周期

创建：第一次执行request.getSession()时差创建

销毁：1. 服务器（非正常）关闭时

    	   2. session 过期/失效（默认30分钟），从不操作服务端的资源开始计时。

```xml
<!-- 配置session过期时间 web.xml -->
<session-config>
	<session-timeout>30</session-timeout>
</session-config>
```

3. 手动销毁

   ` session.invalidate(); `

### JSP

#### jsp脚本

```jsp
<%java 代码%> java代码翻译到service方法内部
<%=java变量或表达式> 翻译成service方法内部out.print()
<%!java代码%> 翻译成servlet的成员内容
```

#### jsp注释

|        | 可见范围 | jsp源码 | 翻译后的servlet | 页面显示html源码 |
| ------ | :--: | :---: | :---------: | :--------: |
| html注释 |  可见  |  可见   |     可见      |     可见     |
| java注释 |  可见  |  可见   |     可见      |            |
| jsp注释  |      |  可见   |             |            |

#### jsp指令

1. page指令

   1. 常用属性：language: jsp脚本嵌入语言种类

      ​		  pageEncoding: 当前jsp文本编码

      ​		  contentType: 翻译成response.setContentType(编码);

      ​		  import: 导包

      ​		  errorPage: 配置错误页面，不配置默认使用web.xml中配置

2. include指令

   将一个jsp页面包含到另一个jsp页面中

   + 静态包含
     + 格式：`<%@ include file="被包含的文件地址"%>`

3. taglib指令

   在jsp页面中引入标签库

   格式：<%@ taglib uri="标签库地址" prefix="前缀"%>

#### jsp隐式对象

| 名称          | 类型                  | 描述                |
| ----------- | ------------------- | ----------------- |
| request     | HttpServletRequest  | 得到用户请求信息          |
| response    | HttpServletResponse | 服务器向客户端的回应信息      |
| session     | HttpSession         | 用来保存用户的信息         |
| application | ServletContext      | 所有用户的共享信息         |
| page        | Object              | 当前页面转换后的Servlet实例 |
| pageContext | PageContext         | JSP的页面容器          |
| out         | JspWriter           | 用于页面输出            |
| config      | ServletConfig       | 服务器配置，可以取得初始化参数   |
| exception   | Throwable           | JSP页面发生的异常，错误页中有效 |

#### pageContext

pageContext是一个域对象

```java
/*向其他作用域中存储数据*/
setAttribute(String name,Object obj,int scope);
getAttribute(String name,int scope);
removeAttribute(String name,int scope);
/*查找属性*/
findAttribute(String name);
```



#### 四大作用域

pageContext < request < session < application

当前页 < 一次请求 < 一次会话 < 整个web应用

#### jsp标签（动作）

动态包含

- 格式：`<jsp:include page="被包含的文件地址">`

请求转发

+ `<jsp:forward page="要转发的资源"`


#### EL技术

##### EL表达式从域中取数据

```jsp
${EL表达式}
${pageContextScope.key}; <!--获得pageContext域中的值-->
${requestScope.key}; <!--获得request域中的值-->
${sessionScope.key}; <!--获得session域中的值-->
${applicationScope.key}; <!--获得application域中的值-->
${key}; <!--获取所有域中的值-->
```

##### EL的内置对象11个

```jsp
<!--获取域中数据-->
pageScope, requestScope, sessionScope, applicationScope
<!--接收参数-->
param, paramValues
<!--获取请求头信息-->
header, headerValues
<!--获取全局初始化参数-->
initParam
<!--web开发中的cookie-->
cookie
<!--web开发中的pageContext-->
pageContext
```

##### EL执行表达式

```jsp
${1+1} <!--显示运算结果-->
${obj==null?true:false} <!--显示三元运算符结果-->
${empty obj} <!--判断是否为null,为null返回true,否则返回false-->
```

#### JSTL技术

```jsp
<!--导入JSTL核心标签库
<%@ taglib uri="http://java.sum.com/jsp/jstl/core" prefix="c"%>

<c:if test="布尔表达式">表达式为true时显示</c:if>

<c:forEach befin="0" end="5" var="i"></c:forEach>
<c:forEach items="" var=""></c:forEach>
```

### JavaEE开发模式

### MVC

+ M : Model 模型 javaBean 封装数据
+ V : View 视图 jsp 页面显示
+ C : Controller 控制器 Servlet 获取封装传递数据

#### JavaEE三层架构

+ web层 ： 与客户端交互（MVC）
+ service层 ：复杂业务处理
+ dao层 ：与数据库交互