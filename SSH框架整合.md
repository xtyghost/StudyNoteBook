### 三大框架整个理论

Spring与Struts2整合就是将Action对象交给Spring容器负责创建

Spring与Hibernate整合就是将sessionFactory交给Spring负责,Spring负责session维护以及AOP事务

### Struts2与Spring整合

struts.xml

```xml
<struts>
    <!--配置将Action创建交给Spring容器-->
    <constant name="struts.objectFactory" value="spring"/>
  
    <package name="包名" namespace="/" extends="struts-default">
        <!--
            整合方案一 : class属性上仍然配置Action的完整类名
            struts2仍然创建Action,由Spring负责组装Action中的依赖属性
        -->
        <action name="Action名" class="Action全类名" method="方法名">
            <result name="返回值"></result>
        </action>
        <!--
			整合方案二 : class属性上填写Spring中Action对象的BeanName
			完全由Spring管理Action生命周期,Action需要配置为多例
		-->
        <action name="Action名" class="BeanName" method="方法名">
            <result name="返回值"></result>
        </action>
    </package>
</struts>
```

applicationContext.xml

```xml
<!--配置Action-->
<bean name="BeanName" class="Action全类名" scope="prototype"/>
```

### Hibernate与Spring整合

整合原理 : 将SessionFactory对象交给Spring容器管理

applicationContext.xml

```xml
<!--配置sessionFactory-->
<bean name="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
	<!-- 加载配置方案一 : 使用外部hibernate.cfg.xml -->
	<property name="configLocation" value="classpath:hibernate.cfg.xml"/>
	<!--加载配置方案二 : 在Spring配置中放置Hibernate配置信息-->
	<!--配置基本信息-->
	<property name="hibernateProperties">
		<props>
			<prop key="键">值</prop>
        </props>
    </property>
    <!--引入ORM元数据-->
    <property name="mappingDirectoryLocations" value="classpath:domain"/>
</bean>
```

#### 引入C3P0连接池

```xml
<!--读取db.properties文件-->
<context:property-placeholder location="classpath:db.properties"/>
<!--配置c3p0连接池-->
<bean name="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<property name="属性名" value="${属性值}"/>
</bean>

<!--配置sessionFactory-->
<bean name="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
	<!--将连接池注入到sessionFactory,hibernate会通过连接池获得连接-->
    <property name="dataSource" ref="dataSource"/>
</bean>
```

### Spring的AOP事务

配置核心事务管理器

```xml
<!--配置核心事务管理器-->
<bean name="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
	<property name="sessionFactory" ref="sessionFactory"/>
</bean>
```



注解配置AOP事务