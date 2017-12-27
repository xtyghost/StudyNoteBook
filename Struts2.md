### Struts2入门

#### 作用
代替Servlet，处理访问服务器的请求
#### 用法
1. 导包
2. 创建Action类
3. 创建struts.xml
4. 将struts2核心过滤器配置到web.xml
  org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter

#### Struts.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
        "http://struts.apache.org/dtds/struts-2.3.dtd">

<struts>
    <package name="package名" namespace="/命名空间" extends="struts-default">
      	<!-- 找不到包下的action,默认访问该action -->
      	<default-action-ref name="action类名"></default-action-ref>
      	<!-- 出现异常跳转到error结果 -->
      	<global-exception-mappings>
            <exception-mapping exception="异常全类名" result="error"></exception-mapping>
        </global-exception-mappings>
        <action name="Action名" class="action类名" method="类中方法名">
            <result name="success">/hello.jsp</result>
        </action>
    </package>
  <!-- 引入其他配置文件 -->
  <include file="配置文件路径"></include>
</struts>
```

pagckage : 将action配置封装,就是可以在package中配置多个action

+ name : pagckage名,不能重复,标示作用
+ namespace : 给action访问路径定义名称空间
+ extends : 继承一个指定包
+ abstract : 包是否为抽象,标示该包不能独立运行

action : 配置action类
+ name : 决定了action访问资源名
+ class : action完整类名
+ method : 指定调用action中哪个方法处理请求

result : 结果配置
+ name : 结果处理名称,与action方法返回值对应
+ type : 指定调用哪个result类处理结果,默认使用转发
+ 标签体 : 页面相对路径

### Struts2常量配置

#### 配置位置

+ 方式一 : struts.xml中 (重点)

+ 方式二 : src下创建struts.properties

```xml
<constant name="键" value="值"></constant>
```

+ 方式三 : web.xml

```xml
<context-param>
	<param-name></param-name>
  	<param-value></param-value>
</context-param>
```

#### 常用配置

```xml
<!-- 指定访问action时的后缀名 -->
<!-- action,, 表示action和空都可以 -->
<constant name="struts.action.extension" value="action,,"></constant>

<!-- 指定struts2是否以开发模式运行 -->
<!-- 开发模式:热加载主配置,更多错误信息输出 -->
<constant name="struts.devMode" value="true"></constant>
```



### Struts2访问流程和架构

![Struts2访问流程和架构](/Users/zhangaochong/Library/Mobile Documents/iCloud~com~coderforart~iOS~MWeb/Documents/image/Struts2访问流程和架构.jpg)

### 动态方法调用

#### 方法一

+ 配置常量

```xml
<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>
```

+ 调用格式

名称空间/action名!方法名

#### 方法二

+ 配置方法

```xml
<action name="action名_*" class="类名" method="{1}">
	<result name=""></result>
</action>
```

+ 调用方法

名称空间/action名_方法名

### Struts2默认配置

method属性 : execute

result的name属性 : success

result的type属性 : dispatcher转发

class属性 : com.opensymphony.xwork2.ActionSupport(继承自struts-default.xml)

### action类的书写

+ 方式一 : 创建一个类,可以是POJO

POJO : 不继承任何父类,也不需要实现任何接口

+ 方式二 : 实现一个Action接口

实现execute方法,提供action方法的规范,预置返回值字符串

+ 方式三 : 继承ActionSupport类(常用)

ActionSupport类实现了Validateable , validationAware , TextProvider , LocaleProvider接口

### 结果跳转方式

```xml
<!-- 转发 -->
<action name="action名" class="action类名" method="action方法名">
	<result name="success" type="dispatcher">转发地址</result>
</action>

<!-- 重定向 -->
<action name="action名" class="action类名" method="action方法名">
	<result name="success" type="redirect">重定向地址</result>
</action>

<!-- 转发到Action -->
<action name="action名" class="action类名" method="action方法名">
	<result name="success" type="chain">
  		<param name="actionName">转发action名</param>
      	<param name="nameSpace">转发action名称空间</param>
    </result>
</action>

<!-- 重定向到Action -->
<action name="action名" class="action类名" method="action方法名">
	<result name="success" type="redirectAction">
  		<param name="actionName">重定向action名</param>
      	<param name="nameSpace">重定向action名称空间</param>
      	<!-- 使用OGNL表达式添加参数 -->
      	<param name="参数名">${参数名}</param>
    </result>
</action>
```

### ActionContext

作用 : 数据中心

生命周期 : 每次请求时都会创建一个与请求对应的ActionContext对象,请求处理完销毁

| 名称               | 类型                  |
| ---------------- | ------------------- |
| 原生request        | HttpServletRequest  |
| 原生response       | HttpServletResponse |
| 原生ServletContext | ServletContext      |
| request域         | Map                 |
| session域         | Map                 |
| application域     | Map                 |
| param参数          | Map                 |
| attr域            | Map                 |
| ValueStack       | 值栈                  |
| ...              | ...                 |

### Struts2获得参数

#### 属性驱动获得参数

准备与参数键名称相同的属性

自动类型转换只能转换8大基本数据类型以及对应包装类

支持yyyy-MM-dd字符串转换成Date

```java
private 属性类型 属性名
对应get/set方法
```

```html
<input name="属性名">
```

#### 对象驱动获得参数

```java
private 对象类型 对象名
对应get/set方法
```

```java
对象类
private 属性类型 属性名
对应get/set方法
```

```html
<input name="对象名.属性名">
```

#### 模型驱动获得参数

```java
实现ModelDriven<对象类型>接口
private 对象类型 对象名 = new 对象类型()
@Override
public 对象类型 getModel() {
   return 对象名;
}
```

### OGNL表达式

ognl : 对象视图导航语言

#### OgnlContext

构成

+ root : 任何对象, ognlContext.setRoot(对象名);
+ Context : Map, ognlContext.setValues(Map);

#### 取值语法

```java
// 取值
Ognl.getValue(ognl表达式,OgnlContext,root);
// 取出root中的对象属性
属性
// 给root中的对象属性赋值
属性='值'
// 调用root中的对象的方法
方法名()
// 取出context中的对象的属性
#键.属性名
#键.属性名='值'
#键.方法名()
// 获取静态属性及方法
@全类名@属性名
@全类名@方法名
// 创建list对象
{值1,值2,...}
// 创建map对象
#{键1:值1,键2:值2,...}
```



### 拦截器Interceptor

生命周期 : 整个项目

#### 创建方式

方式一 : 实现Iterceptor接口

方式二 : 继承AbstractInterceptor类

方式三 : 继承MethodFilterInterceptor类

#### MethodFilterInterceptor

方法过滤拦截器,定制拦截器拦截的方法

```java
protected String doIntercept(ActionInvocation actionInvocation) throws Exception {
    // 前处理
  	
    // 放行
    actionInvocation.invoke();
  
    // 后处理
  
    // 不放行时返回处理结果
    return null;
}
```

#### 配置拦截器

```xml
<interceptors>
    <!-- 1.注册拦截器 -->
	<interceptor name="拦截器名" class="拦截器类名"/>
    <!-- 2.注册拦截器栈 -->
	<interceptor-stack name="拦截器栈名">
        <!-- 引入自定义拦截器,建议放在默认拦截器前 -->
		<interceptor-ref name="拦截器名">
             <!-- 指定是否拦截指定方法(可选配置) -->
          	 <!-- excludeMethods 不拦截 -->
             <!-- includeMethods 拦截 -->
        	 <param name="excludeMethods">方法一,方法二</param>
        </interceptor-ref>
        <!-- 引用默认拦截器栈(20个拦截器) -->
		<interceptor-ref name="defaultStack"></interceptor-ref>
	</interceptor-stack>
</interceptors>
<!-- 指定默认拦截器 -->
<default-interceptor-ref name="myStack"></default-interceptor-ref>
```

