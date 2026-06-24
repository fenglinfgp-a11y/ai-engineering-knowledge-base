# MySQL 常用命令

来源类型：官方文档 / 实战总结

## 问题场景

需要连接 MySQL、查看结构、备份恢复、分析 SQL 和清理数据。

## 适用范围

适用于测试库、授权生产库和脱敏数据维护。

## 判断方法

写操作前必须确认库名、表名、条件、备份和影响行数。

## 操作步骤

```bash
mysql -h <host> -P 3306 -u <user> -p
```

用途：连接 MySQL，密码交互输入。

```sql
SHOW DATABASES;
```

用途：查看数据库列表。

```sql
SHOW CREATE TABLE <table_name>\G
```

用途：查看完整建表语句。

```sql
EXPLAIN SELECT * FROM <table_name> WHERE <column_name> = 'test';
```

用途：查看 SQL 执行计划。

```bash
mysqldump -h <host> -u <user> -p --no-data <database_name> > schema.sql
```

用途：只导出数据库结构。

```sql
DELETE FROM <table_name> WHERE created_at < '2026-01-01' LIMIT 1000;
```

用途：分批删除旧数据。风险：会删除数据，必须先备份并用 SELECT 验证范围。

## 相关命令

常用组合：`mysql`、`mysqldump`、`SHOW CREATE TABLE`、`EXPLAIN`、`DELETE ... LIMIT`。

## 验证方法

查询结果符合预期，备份可恢复，写操作影响行数符合预估。

## 注意事项

不保存真实数据库数据和密码。

## 后续可补充内容

- 慢查询分析命令。
- 主从复制检查命令。

