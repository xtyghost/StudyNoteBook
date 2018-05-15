### SQL及索引优化

#### 慢查日志

```sql
/* 查看是否开启慢查日志 */
show variables like 'slow_query_log';
/* 开启未记录索引的查询日志 */
set global log_queries_not_using_indexes=on;
/* 查看超过多长时间的查询记录到日志 */
show variables like 'long_query_time';
/* 开启慢查日志 */
set global slow_query_log=on;
/* 查看慢查日志存储位置 */
show variables like 'slow%';
/* 分析日志 */
mysqldumpslow
```



### 数据库表结构优化

### 系统配置优化

### 硬件优化