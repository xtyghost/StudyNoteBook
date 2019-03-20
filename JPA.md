### SpringBootDataJpa

依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

配置

```yaml
spring:
  datasource:
    url: jdbc:mysql://数据库地址/数据库名?characterEncoding=utf-8&useSSL=false
    username: 用户名
    password: 密码
    driver-class-name: com.mysql.jdbc.Driver
  jpa:
    show-sql: true
```

实体类

```java
@Table(name = "表名")
@Entity
// 动态更新时间
@DynamicUpdate
public class 类名 {
    @Id
    @GeneratedValue
    private 主键类型 主键名;
}
```

dao

```java
public interface dao名 extends JpaRepository<实体类, 主键类型> {
}
```

#### 复杂查询

```java
public interface dao名 extends JpaRepository<实体类, 主键类型>, JpaSpecificationExecutor<实体类> {
}

List<实体类> 返回值 = dao名.findAll((root, query, cb) -> {
    // =
    Predicate p1 = cb.equal(root.get("查询字段"), 条件值);
    // >
    Predicate p2 = cb.gt(root.get("查询字段"), 条件值);
    // and
    Predicate p3 = cb.and(p1, p2);
    return query.where(p3).getRestriction();
});
```

