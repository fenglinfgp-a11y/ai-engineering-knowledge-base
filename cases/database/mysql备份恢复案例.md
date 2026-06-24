# MySQL 备份恢复案例

来源类型：官方文档 / 实战总结

## 问题现象

需要把 `<database_name>` 迁移到测试环境，或在变更前做可恢复备份。

## 环境背景

- 数据库：MySQL `<version>`
- 源库：`<source_database>`
- 目标库：`<target_database>`
- 备份文件：`backup.sql`

## 初步判断

先确认备份范围、目标库状态、字符集、是否包含真实敏感数据，以及是否需要只导出结构。

## 排查命令

```bash
mysqldump -h <host> -u <user> -p --databases <database_name> > backup.sql
```

用途：导出指定数据库结构和数据。

```bash
mysqldump -h <host> -u <user> -p --no-data <database_name> > schema.sql
```

用途：只导出表结构，适合交付或测试初始化。

```bash
mysql -h <host> -u <user> -p <target_database> < backup.sql
```

用途：恢复 SQL 到目标库。风险：可能写入大量数据或覆盖目标库内容，执行前必须确认目标库。

```sql
SHOW TABLES;
```

用途：恢复后查看表是否导入。

## 关键日志

```text
ERROR 1049 (42000): Unknown database '<target_database>'
```

含义：目标库不存在，需要先创建空库或修正数据库名。

## 解决步骤

1. 确认源库和目标库名称。
2. 根据场景选择完整备份或只导出结构。
3. 在测试库先演练恢复。
4. 恢复后检查表数量和核心表行数。
5. 记录恢复命令但不记录密码。

## 验证方式

- `SHOW TABLES` 能看到预期表。
- 核心表行数符合预期。
- 应用连接测试库能启动。

## 可复用经验

恢复前先确认目标库，不要把生产备份直接导入错误环境。

## 禁止事项

- 不提交真实备份文件。
- 不把密码写在命令行或文档中。
- 不使用未脱敏生产数据做公开交付。

## 后续可补充内容

- 只恢复单表案例。
- 大库压缩备份和恢复案例。

