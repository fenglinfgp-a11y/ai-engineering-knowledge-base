# 常见 SQL 问题

来源类型：官方文档 / 实战总结  
参考来源：MySQL EXPLAIN 官方文档

## 问题场景

SQL 慢、查询结果不对、删除误操作风险、日志表过大。

## 判断方法

- 慢查询先看执行计划。
- 结果不对先检查条件、连接方式、排序和分页。
- 删除清理前必须先 `SELECT` 验证范围。

## 操作步骤

```sql
EXPLAIN SELECT * FROM <table_name> WHERE <column_name> = 'test';
```

用途：查看查询计划，判断是否全表扫描。

```sql
SELECT COUNT(*) FROM <log_table> WHERE created_at < '2026-01-01';
```

用途：删除前先统计将影响的数据量。

```sql
DELETE FROM <log_table> WHERE created_at < '2026-01-01' LIMIT 1000;
```

用途：分批删除旧日志，降低锁表风险。风险：会删除数据，必须先备份并确认条件。

```sql
OPTIMIZE TABLE <log_table>;
```

用途：尝试整理表空间。风险：可能锁表或占用大量 IO，生产环境低峰执行。

## 验证方法

- `EXPLAIN` 显示使用索引或扫描行数下降。
- 删除后业务功能正常，日志表行数符合预期。
- 应用错误日志无 SQL 异常。

## 注意事项

- 不允许在知识库保存真实 SQL 数据。
- 大表清理优先考虑归档、分区、分批任务，不要一次性全量删除。

## 适用范围

MySQL 表结构、备份恢复、SQL 排查和脱敏数据维护。

## 相关命令

`mysql`、`mysqldump`、`SHOW CREATE TABLE`、`EXPLAIN`。

## 后续可补充内容

补充备份演练、慢 SQL 对比和日志表归档策略。

