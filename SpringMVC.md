### SpringMVC执行流程

![SpringMVC](image/SpringMVC.png)

### 配置前端控制器

```xml
<servlet>
	<servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:SpringMVC.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```

### 配置视图解析器

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!-- 后缀 -->
    <property name="suffix" value=".jsp"/>
</bean>
```

### 配置注解驱动

` <mvc:annotation-driven/> `

### Controller类

```java
@Controller
@RequestMapping(value = "请求前缀")
public class 类名 {
  	// 请求URL映射
    @RequestMapping(value = "请求路径")
  	// 多个请求URL映射
  	@RequestMapping(value = {"请求路径1","请求路径2"})
  	// 指定方法请求URL映射
  	@RequestMapping(value = "请求路径", method=RequestMethod.请求方式)
  	// 返回对象转为json
  	@ResponseBody
    public String list(Model model, @RequestParam(value="请求参数")){
        return "视图地址";
      	return "redirect:视图地址";
      	return "forward:视图地址";
    }
```

```java
// 获取请求参数

// /url/参数值
@RequestMapping("/url/{参数名}")
public String 方法(@PathVariable 参数类型 参数名)

// /url?参数名=参数值
@RequestMapping("/url")
public String 方法(@RequestParam(参数名) 参数类型 变量名)
  
// RequestParam参数
value 参数名
required 是否必传(默认true) // 为true时不传会报错:HTTP Status 400 - Required 参数类型 parameter '参数名' is not present
defaultValue 参数默认值
```



### 解决POST请求乱码

web.xml

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>*.action</url-pattern>
</filter-mapping>
```
### 解决GET请求乱码

+ 修改 tomcat 配置文件添加编码与工程编码一致
+ 对参数进行重新编码 ` String userName = new String(Request.getParameter("userName").getBytes("ISO8859-1"),"utf-8"); `

### 异常处理

```java
public class CustomerException implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("msg",e.getMessage());
        modelAndView.setViewName("exception");
        return modelAndView;
    }
}
// 在springmvc.xml中配置该类
```

### 上传文件

jsp

```jsp
<form id="itemForm"	action="${pageContext.request.contextPath }/updateItem" method="post" enctype="multipart/form-data">
	<input type="file"  name="名"/>
</form>
```

controller类

```java
public String 方法(MultipartFile 名){
        
}
```

springmvc配置文件

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  	<!-- 上传文件最大值 -->
    <property name="maxUploadSize" value="5000000"/>
</bean>
```

### Json交互

```javascript
$(function(){
  $.ajax({
    url: "请求地址",
    data: json字符串,
    contenType: "application/json;charset=utf-8",
    type: "post",
    dataType: "json",
    success: function(data){
      
    }
  })
})
```

```java
@RequestMapping("请求地址")
@ResponseBody
public 对象类型 方法名(@RequestBody 对象类型 对象名){
  return 对象名;
}
```

### 拦截器

```java
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        System.out.println("方法执行前");
      	// 是否放行
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {
        System.out.println("方法执行后");
    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
        System.out.println("视图渲染后");
    }
}
```

```xml
<mvc:interceptors>
	<mvc:interceptor>
      	<!-- 拦截路径 -->
		<mvc:mapping path="/**"/>
        <bean class="拦截器全类名"/>
    </mvc:interceptor>
</mvc:interceptors>
```

#### 多个拦截器执行顺序

拦截器1 方法执行前

拦截器2 方法执行前

...

执行方法

...

拦截器2 方法执行后

拦截器1 方法执行后

渲染视图

...

拦截器2 视图渲染后

拦截器1 视图渲染后