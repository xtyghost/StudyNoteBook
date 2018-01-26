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
      	<!-- 批量引入 --> 
        <package name="包名"/>
    </mappers>
</configuration>
```

### Mapper配置

```xml
<mapper namespace="Dao全类名">
    <select id="Dao方法名" parameterType="参数类型" resultType="返回值类型">
        SQL语句 #{value} ${value}
        <where>
            <if test="判断条件">
                条件语句
            </if>
        </where>
    </select>
</mapper>
```

