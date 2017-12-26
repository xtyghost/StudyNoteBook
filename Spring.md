### Spring配置

applicationContext.xml

```xml
<beans>
    <!--  vscope属性 -->
    <!-- singleton : 单例对象 (默认) -->
    <!-- prototype : 多例,整合struts2时ActionBean必须配置为多例 -->
    <bean name="对象名" class="对象类名" scope="prototype"></bean>
</beans>
```

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
	<lsit>
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
<context:component-scan base-package="包名"></context:component-scan>
```

类中使用

#### 配置对象

```java
@Component("对象名")
@Service("对象名") // 用于service层
@Controller("对象名") // 用于web层
@Repository("对象名") // 用于dao层
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
+ Pointcut (切入点) : 目标对象,要增强的方法
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

#### XML配置AOP

```xml
<!-- 将通知织入目标对象 -->
<aop:config>
	<!-- 配置切入点 -->
	<aop:pointcut expression="execution(切点表达式)" id="切入点名"/>
	<!-- 配置切面(手动指定通知方法) -->
	<aop:aspect ref="通知名">
		<!-- 配置前置通知 -->	             <!-- 指定切入点 -->
		<aop:befor method="前置通知方法名" pointcut-ref="切入点名"/>
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

