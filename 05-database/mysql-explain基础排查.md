# MySQL EXPLAIN 基础排查

来源类型：官方文档 / 实战总结  
参考来源：MySQL 官方文档

## 问题场景

SQL 执行慢、接口超时、数据库 CPU 升高，需要判断查询是否走索引、扫描行数是否异常。

## 适用范围

- 适用于 MySQL 查询计划初步分析。
- 适用于测试库、脱敏 SQL 和慢查询定位。
- 不保存真实业务数据、客户字段值或生产库账号。

## 判断方法

- 看 `type` 是否从 `ALL`、`index` 改善到 `range`、`ref`、`const`。
- 看 `key` 是否使用预期索引。
- 看 `rows` 是否远高于预期。
- 看 `Extra` 是否出现 `Using temporary`、`Using filesort`。
- 结合 `WHERE`、`ORDER BY`、`LIMIT` 判断索引是否匹配。

## 操作步骤

```sql
EXPLAIN SELECT * FROM test_orders WHERE user_id = '<USER_ID>' ORDER BY created_at DESC LIMIT 20;
```

用途：查看查询计划，判断索引使用情况。示例只使用测试表和脱敏字段。

```sql
SHOW INDEX FROM test_orders;
```

用途：查看测试表已有索引。

```sql
SHOW CREATE TABLE test_orders;
```

用途：查看表结构和索引定义，便于判断字段类型是否匹配。

```sql
EXPLAIN FORMAT=JSON SELECT * FROM test_orders WHERE status = '<STATUS>';
```

用途：查看更详细的 JSON 查询计划。

## 验证方法

- 优化前后同一条脱敏 SQL 的 `rows` 明显下降。
- `key` 使用了预期索引。
- 接口响应时间或慢查询日志中同类 SQL 耗时下降。
- 结果集数量与优化前一致。

## 注意事项

- `EXPLAIN` 只说明执行计划，不代表实际执行耗时一定相同。
- 不要为了让单条 SQL 走索引盲目新增大量索引。
- 生产库优化前先在测试库验证。

## 相关命令

`EXPLAIN`、`EXPLAIN FORMAT=JSON`、`SHOW INDEX`、`SHOW CREATE TABLE`。

## 参考来源

- https://dev.mysql.com/doc/refman/8.4/en/explain.html
- https://dev.mysql.com/doc/refman/8.4/en/optimization-indexes.html

