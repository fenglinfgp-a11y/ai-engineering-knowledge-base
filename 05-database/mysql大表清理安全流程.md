# MySQL 大表清理安全流程

来源类型：官方文档 / 实战总结  
参考来源：MySQL 官方文档

## 问题场景

日志表、消息表、临时表过大，影响查询和磁盘空间，需要安全清理历史数据。

## 适用范围

- 适用于测试库演练后的 MySQL 大表分批清理。
- 适用于日志、流水、临时数据等可按规则归档的数据。
- 不适用于未确认业务含义的生产核心数据直接删除。

## 判断方法

- 先确认数据是否允许删除，是否需要归档。
- 用 `COUNT(*)` 估算清理范围。
- 用索引字段作为清理条件，例如时间或自增 ID。
- 分批执行，避免长事务和锁等待。
- 每批后验证剩余数量和业务状态。

## 操作步骤

```sql
SELECT COUNT(*) FROM test_logs WHERE created_at < '<YYYY-MM-DD>';
```

用途：统计待清理范围。

```sql
SELECT MIN(id), MAX(id) FROM test_logs WHERE created_at < '<YYYY-MM-DD>';
```

用途：确认待清理数据的 ID 范围。

```sql
DELETE FROM test_logs WHERE created_at < '<YYYY-MM-DD>' ORDER BY id LIMIT 1000;
```

用途：按小批量删除历史日志。风险：会删除数据，必须先备份并确认业务允许清理。

```sql
OPTIMIZE TABLE test_logs;
```

用途：整理表空间。风险：可能锁表或消耗大量 IO，生产环境需维护窗口。

## 验证方法

- 每批删除后 `ROW_COUNT()` 或客户端返回影响行数符合预期。
- 剩余数据数量逐步下降。
- 应用核心查询正常。
- 磁盘空间、慢查询和备份耗时有可观察改善。

## 注意事项

- 禁止直接在未知生产表上执行无条件 `DELETE`、`DROP`、`TRUNCATE`。
- 大表清理前必须备份，生产环境必须有回滚预案。
- 如果需要长期清理，应考虑归档表、分区或定时任务。

## 相关命令

`SELECT COUNT(*)`、`DELETE ... LIMIT`、`OPTIMIZE TABLE`、`mysqldump`。

## 参考来源

- https://dev.mysql.com/doc/refman/8.4/en/delete.html
- https://dev.mysql.com/doc/refman/8.4/en/optimize-table.html

