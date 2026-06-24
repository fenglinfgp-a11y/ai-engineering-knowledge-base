# MySQL 慢 SQL EXPLAIN 案例

来源类型：官方文档 / 实战总结

## 问题现象

某接口响应变慢，后端日志显示查询 `<table_name>` 耗时明显升高。

## 环境背景

- 数据库：MySQL `<version>`
- 表：`<table_name>`
- 查询字段：`<column_name>`
- 示例值：`<test-value>`

## 初步判断

优先判断是否全表扫描、索引未命中、排序临时表、返回行数过大或分页方式不合理。

## 排查命令

```sql
EXPLAIN SELECT * FROM <table_name> WHERE <column_name> = '<test-value>';
```

用途：查看查询执行计划，判断是否命中索引。

```sql
SHOW INDEX FROM <table_name>;
```

用途：查看表索引列表。

```sql
SHOW CREATE TABLE <table_name>\G
```

用途：查看完整表结构和索引定义。

## 关键日志

```text
Query took <seconds>s: SELECT ... FROM <table_name> ...
```

含义：应用层记录到慢查询，需要结合 `EXPLAIN` 判断根因。

## 解决步骤

1. 复制脱敏 SQL 到测试库。
2. 执行 `EXPLAIN`。
3. 检查 `type`、`key`、`rows`、`Extra`。
4. 优先优化 where 条件、索引和分页。
5. 在测试库验证后再评估生产变更。

## 验证方式

- `EXPLAIN` 显示扫描行数下降。
- 接口耗时下降。
- 数据结果与优化前一致。

## 可复用经验

慢 SQL 先看执行计划，不要凭感觉加索引；索引要服务真实查询条件。

## 禁止事项

- 不保存真实 SQL 参数和用户数据。
- 不在生产大表直接加索引而不评估锁表风险。
- 不用删除数据掩盖慢查询问题。

## 后续可补充内容

- 覆盖索引案例。
- 深分页优化案例。

