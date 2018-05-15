### 核心配置

sqlMapConfig.xml

```xml
<configuration>
	<!-- 定义别名 -->
  	<typeAliases>
      	<!-- 单个定义 -->
        <typeAlias type="全类名" alias="别名"/>
      	<!-- 批量定义,别名为类名(首字母大小写都可以) -->
        <package name="包名"/>
    </typeAliases>

    <!-- 引入Mapper文件 -->
    <mappers>
      	<mapper resource="mapper文件"/>
      	<!-- 批量引入 要求mapper接口文件与mapper.xml文件同名并在同一包下--> 
        <package name="包名"/>
    </mappers>
</configuration>
```

### Mapper配置

```xml
<mapper namespace="Dao全类名">
  	<!-- sql片段 -->
  	<sql id="sql片段名">
        sql语句
    </sql>
  
  	<!-- 查询 -->
    <select id="Dao方法名" parameterType="参数类型" resultType="返回值类型">
        <!-- 引入sql片段 -->
      	<include refid="sql片段名"/>
      	<!-- where条件 -->
        <where>
          	<!-- if判断 -->
            <if test="判断条件">
                条件语句
            </if>
          	<!-- foreach -->
          	<!-- collection
				数组 array
				list集合 list
			-->
          	<foreach collection="遍历对象名" item="元素名" separator="," open="(" close=")">
                #{元素名}
            </foreach>
        </where>
    </select>
  
  	<!-- 添加 -->
  	<insert id="Dao方法名" parameterType="参数类型">
      	<!-- 返回主键 -->
      	<selectKey keyProperty="id" resultType="Integer" order="AFTER">
            select last_insert_id()
        </selectKey>
      	insert语句
  	</insert>
  	
  	<!-- resultMap配置 -->
  	<mapper namespace="Dao全类名">
    <resultMap id="名" type="返回值类型">
        <id property="对象ID名" column="表中ID名"/>
      	<!-- 
		 单表查询时对象属性名与表中字段名相同可省略,多表查询不能省略 
		-->
        <result property="对象属性名" column="表中字段"/>
      
      	<!-- 一对一 -->
      	<association property="关联对象" javaType="对象类型">
            <id property="对象ID名" column="表中ID名"/>
        	<result property="对象属性名" column="表中字段"/>
        </association>
      
      	<!-- 一对多 -->
      	<collection property="关联集合" ofType="集合泛型">
            <id property="对象ID名" column="表中ID名"/>
        	<result property="对象属性名" column="表中字段"/>
        </collection>
    </resultMap>
    <select id="Dao方法名" resultMap="名">
        select语句
    </select>
</mapper>
```

### #{}和${}的区别

- \#{}是预编译处理,${}是字符串替换
- \#{}可以防止SQL注入

### MyBatis的编程步骤

1. 创建SqlSessionFactory
2. 通过SqlSessionFactory创建SqlSession
3. 通过SqlSession执行数据库操作
4. 调用session.commit()方法提交事务
5. 调用session.close()关闭会话

### 调用存储过程

```xml
<select id="setDefaulta" statementType="CALLABLE" parameterType="Map" flushCache="false">
        <![CDATA[
        call setDefault(
        #{luaid,mode=IN,jdbcType=BIGINT},
        #{uid,mode=IN,jdbcType=INTEGER},
        #{isdefault,mode=IN,jdbcType=INTEGER})
        ]]>
</select>
```

