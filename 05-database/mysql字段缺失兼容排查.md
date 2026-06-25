# MySQL 字段缺失兼容排查

来源类型：官方文档 / 实战总结  
参考来源：MySQL 官方文档

## 问题场景

接口报 unknown column、前后端版本不一致、代码访问字段但测试库或客户库没有该字段。

## 适用范围

- 适用于 MySQL 表结构差异、迁移脚本遗漏、交付 SQL 不完整排查。
- 适用于测试库和脱敏结构对比。
- 不保存真实客户表数据。

## 判断方法

- 确认报错字段名和表名。
- 对比当前库、目标库、交付 SQL 的表结构。
- 检查迁移脚本是否执行过。
- 判断新增字段是否允许为空、是否有默认值。
- 应用发布前确认代码和数据库迁移顺序。

## 操作步骤

```sql
DESCRIBE test_orders;
```

用途：查看测试表字段列表。

```sql
SHOW COLUMNS FROM test_orders LIKE '<column_name>';
```

用途：确认指定字段是否存在。

```sql
SHOW CREATE TABLE test_orders;
```

用途：查看完整建表语句，便于与交付 SQL 对比。

```sql
ALTER TABLE test_orders ADD COLUMN extra_info VARCHAR(255) NULL;
```

用途：在测试库演练添加兼容字段。风险：会修改表结构，生产执行前必须备份并评估锁表影响。

## 验证方法

- 报错字段在目标库存在。
- 应用启动和相关接口不再报 unknown column。
- 老数据在新增字段为空或默认值情况下仍可正常读取。
- 交付 SQL 或迁移脚本已补齐。

## 注意事项

- 字段类型、默认值、字符集要与代码预期一致。
- 生产表结构变更需评估表大小、锁、回滚方案。
- 只记录字段占位符或通用字段，不记录客户业务敏感字段。

## 相关命令

`DESCRIBE`、`SHOW COLUMNS`、`SHOW CREATE TABLE`、`ALTER TABLE`。

## 参考来源

- https://dev.mysql.com/doc/refman/8.4/en/show-columns.html
- https://dev.mysql.com/doc/refman/8.4/en/alter-table.html

