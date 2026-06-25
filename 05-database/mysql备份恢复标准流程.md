# MySQL 备份恢复标准流程

来源类型：官方文档 / 实战总结  
参考来源：MySQL 官方文档

## 问题场景

需要迁移数据库、恢复测试环境、上线前备份、交付结构 SQL，或验证备份是否可用。

## 适用范围

- 适用于 MySQL 逻辑备份、测试库恢复和交付 SQL。
- 适用于脱敏数据或测试数据。
- 不保存真实生产备份、客户资料和数据库密码。

## 判断方法

- 明确备份对象：结构、数据，还是结构加数据。
- 明确恢复目标：空库、测试库，还是生产回滚。
- 恢复前记录目标库名称和表数量。
- 生产恢复必须先在测试库演练。

## 操作步骤

```bash
mysqldump -h <host> -u <user> -p --databases <test_database> > backup.sql
```

用途：备份测试库结构和数据，密码交互输入。

```bash
mysqldump -h <host> -u <user> -p --no-data <test_database> > schema.sql
```

用途：只导出表结构，适合交付和结构审查。

```bash
mysql -h <host> -u <user> -p -e "CREATE DATABASE <restore_database> DEFAULT CHARACTER SET utf8mb4;"
```

用途：创建测试恢复库。

```bash
mysql -h <host> -u <user> -p <restore_database> < backup.sql
```

用途：导入备份到恢复库。风险：会写入大量数据，执行前确认目标库是测试库或已获授权。

## 验证方法

- `SHOW TABLES;` 表数量符合预期。
- 核心测试表 `SELECT COUNT(*)` 与备份记录一致。
- 应用使用测试配置能连接恢复库。
- 恢复过程无 SQL 语法错误和字符集错误。

## 注意事项

- 备份文件可能包含敏感数据，不能提交到 Git。
- 命令中的库名必须使用 `<test_database>` 等占位符记录。
- 大库备份恢复要评估磁盘空间、耗时和锁影响。

## 相关命令

`mysqldump`、`mysql < backup.sql`、`CREATE DATABASE`、`SHOW TABLES`、`SELECT COUNT(*)`。

## 参考来源

- https://dev.mysql.com/doc/refman/8.4/en/backup-and-recovery.html
- https://dev.mysql.com/doc/refman/8.4/en/mysqldump.html

