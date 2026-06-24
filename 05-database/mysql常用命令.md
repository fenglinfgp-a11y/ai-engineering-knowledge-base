# MySQL 常用命令

来源类型：官方文档 / 实战总结  
参考来源：MySQL 官方文档

## 问题场景

需要查看数据库、表、字段、连接状态和基础性能信息。

## 判断方法

- 先确认连接的是测试库还是生产库。
- 所有写操作前先确认库名、表名、where 条件。
- 只读检查优先。

## 操作步骤

```bash
mysql -h <host> -P 3306 -u <user> -p
```

用途：连接 MySQL，密码通过交互输入，避免明文出现在 shell 历史。

```sql
SHOW DATABASES;
```

用途：查看当前账号可见的数据库。

```sql
USE <database_name>;
```

用途：切换到目标数据库。

```sql
SHOW TABLES;
```

用途：查看当前数据库表列表。

```sql
DESCRIBE <table_name>;
```

用途：查看表字段、类型、索引和是否允许为空。

## 验证方法

- 当前库名与预期一致。
- 表结构能正常显示。
- 查询命令无权限或语法错误。

## 注意事项

- 不要把真实连接串、密码、生产 IP 写入知识库。
- `DROP`、`TRUNCATE`、无 `WHERE` 的 `DELETE` 都是高风险操作。

## 适用范围

MySQL 表结构、备份恢复、SQL 排查和脱敏数据维护。

## 相关命令

`mysql`、`mysqldump`、`SHOW CREATE TABLE`、`EXPLAIN`。

## 后续可补充内容

补充备份演练、慢 SQL 对比和日志表归档策略。

