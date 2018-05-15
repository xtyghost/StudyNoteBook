### Spring

作用 : 负责管理项目中所有对象

### Spring配置

applicationContext.xml

```xml
<beans>
    <!-- scope属性 -->
    <!-- singleton : 单例对象 (默认) -->
    <!-- prototype : 多例,整合struts2时ActionBean必须配置为多例 -->
    <bean name="对象名" class="对象类名" scope="prototype"></bean>
    <!-- 引入其他配置文件 -->
    <import resource="其他配置文件"/>
</beans>
```

### 管理容器在项目中的生命周期

```xml
<!-- 配置Spring容器随项目启动创建,项目关闭销毁 -->
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!-- 指定加载Spring配置文件的位置 -->
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

### 获取容器

```java
// 获得ServletContext对象
ServletContext sc = ServletActionContext.getServletContext();
// 从ServletContext获得webApplicationContext对象
WebApplicationContext ac = WebApplicationContextUtils.getWebApplicationContext(sc);
```

### 获取容器中对象

```java
// 创建容器对象
ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
// 获得容器中对象
ac.getBean(对象名);
```

### IOC&DI

IOC : Inverse Of Control 反转控制,反转了对象的创建方式,由Spring完成创建及注入

DI : Dependency Injection 依赖注入

注入方式 

+ set方法注入
+ 构造方法注入
+ 字段注入

注入类型

+ 值类型注入
+ 引用类型注入

### BeanFactory&ApplicationContext

#### BeanFactory

+ spring原始接口,实现类功能较为单一
+ 实现类特点是每次获得对象时才会创建对象

#### ApplicationContext

+ 每次容器启动是就会创建容器中配置的所有对象
+ 提供更多功能

### Spring属性注入

set方法注入

```xml
<bean name="对象名" class="对象类名">
  <!-- 值类型注入 -->
  <property name="属性名" value="属性值"></property>
  <!-- 引用类型注入 -->
  <property name="属性名" ref="属性值"></property>
</bean>
```

构造函数注入

```xml
<bean name="对象名" class="对象类名">
  <!-- 值类型注入 -->
  <constructor-arg name="属性名" value="属性值" index="参数索引" type="参数类型"></constructor-arg>
  <!-- 引用类型注入 -->
  <constructor-arg name="属性名" ref="属性值"></constructor-arg>
</bean>
```

#### 复杂类型注入

数组

```xml
<property name="数组名">
	<array>
		<value>值</value>
        <ref bean="值"/>
	</array>
</property>
```

List

```xml
<property name="list名">
	<list>
		<value>值</value>
        <ref bean="值"/>
	</list>
</property>
```

Map

```xml
<property name="map名">
    <map>
    	<entry key="键" value="值"></entry>
        <entry key="键" value-ref="值"></entry>
        <entry key-ref="键" value-ref="值"></entry>
    </map>
</property>
```

Properties

```xml
<property name="Properties名">
    <props>
    	<prop key="键">值</prop>
    </props>
</property>
```

### Spring注解配置

开启使用注解配置,applicationContext.xml

```xml
<!-- 扫描指定包下所有注解,包括子包 -->
<context:component-scan base-package="包名"/>
```

类中使用

#### 配置对象

```java
@Component("对象名")
@Service("对象名") // 用于service层
@Controller("对象名") // 用于web层
@Repository("对象名") // 用于dao层
// 对象名不指定默认为类名首字母小写
// 相当于<bean name="对象名" class="对象类名"/>

@Scope(scopeName = "prototype") //指定scope属性
```

#### 属性注入

```java
// 值类型注入
@Value("值")
值类型属性
@Value("值")
set方法

// 引用类型注入
@Autowired // 自动装配
@Qualifier(对象名) // 指定自动装配对象

@Resource(name="对象名") // 手动注入指定对象
```

### Spring与junit整合测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Demo {
    @Resource(name="user")
    private User user;
    @Test
    public void fun1(){
        System.out.println(user);
    }
}
```

### Spring中的AOP

#### AOP思想

面向切面编程,纵向重复横向抽取

#### Spring实现AOP的原理

1. 动态代理(优先)
2. cglib代理(没有接口时)

#### SpringAOP名词

+ Joinpoint (接入点) : 目标对象中,所有可以增强的方法
+ Pointcut (切入点) : 目标对象要增强的方法
+ Advice (通知/增强) : 增强的代码
+ Target (目标对象) : 被代理对象
+ Weaving (织入) : 将通知应用到切点的过程
+ Proxy (代理) : 将通知织入到目标对象后,形成的代理对象
+ Aspect (切面) : 切入点+通知

#### 定义通知

+ 前置通知 : 目标方法运行之前调用
+ 后置通知(出现异常不会调用) : 目标方法运行之后调用
+ 环绕通知 : 目标方法运行之前之后都调用
+ 异常拦截通知 : 出现异常调用
+ 后置通知 : 目标方法运行之后调用

```java
public class MyAdvice {

    // 前置通知
    public void before(){
        System.out.println("before...");
    }
    // 后置通知，出现异常不调用
    public void afterReturning(){
        System.out.println("afterReturning...");
    }
    // 环绕通知
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before...");
        // 调用目标方法
        Object proceed = pjp.proceed();
        System.out.println("around after...");
        return proceed;
    }
    // 异常拦截通知
    public void afterException(){
        System.out.println("afterException...");
    }
    // 后置通知
    public void after(){
        System.out.println("after...");
    }
}
```



#### XML配置AOP

```xml
<!-- 配置目标对象 -->
<bean name="目标对象名" class="全类名"/>
<!-- 配置通知对象 -->
<bean name="通知对象名" class="全类名"/>
<!-- 将通知织入目标对象 -->
<aop:config>
	<!-- 配置切入点 -->
	<aop:pointcut expression="execution(切点表达式)" id="切入点名"/>
	<!-- 配置切面(手动指定通知方法) -->
	<aop:aspect ref="通知名">
		<aop:befor method="前置通知方法名" pointcut-ref="切入点名"/>
      	<aop:after-returning method="前置通知方法名" pointcut-ref="切入点名"/>
        <aop:around method="后置通知方法名" pointcut-ref="切入点名"/>
        <aop:after-throwing method="异常通知方法名" pointcut-ref="切入点名"/>
        <aop:after method="后置通知方法名" pointcut-ref="切入点名"/>
	</aop:aspect>
	<!-- 配置切面 -->
	<aop:advisor advice-ref="通知名" pointcut-ref="切入点名"/>
</aop:config>
```

切点表达式

```java
void 包名.类名.方法名()
* 包名.类名.方法名()
* 包名.类名.*()
* 包名.类名.*(..)
* 包名.*类名.*(..)
```

### 注解配置AOP

```xml
<!-- 开启使用注解完成织入 -->
<aop:aspectj-autoproxy/>
```

```java
// 配置通知类
@Aspect
// 配置前置通知
@Before("execution(execution(切点表达式))")
// 配置后置通知(出现异常不调用)
@AfterReturning("execution(execution(切点表达式))")
// 配置环绕通知
@Around("execution(execution(切点表达式))")
// 配置异常拦截通知
@AfterThrowing("execution(execution(切点表达式))")
// 配置后置通知
@After("execution(execution(切点表达式))")
```

### Spring的Java配置方式

```java
// 表示该类是一个Spring的配置
@Configuration
// 配置扫描的包
@ComponentScan(basePackages = "包名")
// 读取properties文件
@PropertySource("classpath:properties文件")
// 读取多个properties文件
@PropertySource(value = {"classpath:properties文件1","classpath:properties文件2"})
// 读取properties文件,忽略不存在文件
@PropertySource("classpath:properties文件",ignoreResourceNotFound = true)
public class SpringConfig {
    // 配置一个bean
    @Bean
    public UserDao getUserDao(){
        return new UserDao();
    }
    
    // 读取properties文件中的值
    @Value("${名}")
 	private String 名;
}
```

