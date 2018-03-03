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
  	<!-- 查询 -->
    <select id="Dao方法名" parameterType="参数类型" resultType="返回值类型">
        select语句
        <where>
            <if test="判断条件">
                条件语句
            </if>
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
        <result property="对象属性名" column="表中字段"/>
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