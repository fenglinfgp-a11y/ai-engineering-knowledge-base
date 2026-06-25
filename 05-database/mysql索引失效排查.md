# MySQL 索引失效排查

来源类型：官方文档 / 实战总结  
参考来源：MySQL 官方文档

## 问题场景

表明明有索引，但查询计划没有使用，接口变慢，或新增条件后索引命中率下降。

## 适用范围

- 适用于 MySQL B-Tree 索引常见失效场景排查。
- 适用于测试库和脱敏 SQL。
- 不记录真实表数据和客户字段值。

## 判断方法

- 字段类型是否一致，避免字符串和数字混用导致隐式转换。
- 是否对索引列使用函数或表达式。
- 模糊查询是否以通配符开头。
- 复合索引是否满足最左前缀。
- 排序字段和过滤字段是否能被同一个索引支持。

## 操作步骤

```sql
EXPLAIN SELECT * FROM test_users WHERE phone = '<PHONE_HASH>';
```

用途：查看等值查询是否命中 phone 索引，示例字段已脱敏。

```sql
EXPLAIN SELECT * FROM test_users WHERE DATE(created_at) = '<DATE>';
```

用途：识别对索引列使用函数导致无法有效使用索引的情况。

```sql
SHOW INDEX FROM test_users;
```

用途：查看表上已有索引及列顺序。

```sql
ANALYZE TABLE test_users;
```

用途：更新表统计信息，帮助优化器选择计划。风险：会读取并更新统计信息，生产高峰期慎用。

## 验证方法

- 改写 SQL 后 `EXPLAIN` 中 `key` 使用预期索引。
- `rows` 估算下降。
- 查询结果与改写前一致。
- 慢查询日志中同类 SQL 耗时下降。

## 注意事项

- 不要把所有慢 SQL 都归因于“索引失效”，先看执行计划。
- 新增索引会增加写入成本和磁盘占用。
- 复合索引顺序要基于查询频率和过滤选择性判断。

## 相关命令

`EXPLAIN`、`SHOW INDEX`、`SHOW CREATE TABLE`、`ANALYZE TABLE`。

## 参考来源

- https://dev.mysql.com/doc/refman/8.4/en/mysql-indexes.html
- https://dev.mysql.com/doc/refman/8.4/en/optimization.html

