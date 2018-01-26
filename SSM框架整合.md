### 配置web.xml

```xml
<!--让Spring随web项目启动而创建的监听器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--配置Spring配置文件位置参数-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring/applicationContext-*.xml</param-value>
    </context-param>

    <!-- 解决POST提交乱码 -->
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
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 配置前端控制器 -->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring/springMVC.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

### 配置SpringMVC.xml

```xml
<!-- 扫描注解 -->	
<context:component-scan base-package="包名"/>

<!-- 加载属性文件 -->
<context:property-placeholder location="classpath:rescurce.properties"/>
<!-- 配置注解驱动 -->
<mvc:annotation-driven/>

<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!-- 后缀 -->
    <property name="suffix" value=".jsp"/>
</bean>

<!-- 对静态资源放行 -->
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/js/" mapping="/js/**"/>
<mvc:resources location="/fonts/" mapping="/fonts/**"/>
```

### 配置Spring

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>
    <!-- 配置Druid连接池 -->
    <bean name="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!-- 配置Mybatis工厂 -->
    <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 核心配置文件 -->
        <property name="configLocation" value="classpath:mybatis/SqlMapConfig.xml"/>
    </bean>

    <!-- 配置Mapper动态代理扫描 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 基本包 -->
        <property name="basePackage" value="包名"/>
    </bean>

    <!--配置核心事务管理器-->
    <bean name="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 配置注解驱动 -->
    <tx:annotation-driven/>
```

