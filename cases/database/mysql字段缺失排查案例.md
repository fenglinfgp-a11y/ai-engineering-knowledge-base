# MySQL 字段缺失排查案例

来源类型：官方文档 / 实战总结

## 问题现象

后端接口报错 `Unknown column '<column_name>'`，测试环境正常但部署环境异常。

## 环境背景

- 数据库：MySQL `<version>`
- 表：`<table_name>`
- 缺失字段：`<column_name>`
- 发布方式：SQL 脚本或迁移工具

## 初步判断

优先判断数据库结构是否同步、迁移是否执行、连接的是否正确库、代码版本与数据库版本是否匹配。

## 排查命令

```sql
DESCRIBE <table_name>;
```

用途：查看表字段列表。

```sql
SHOW CREATE TABLE <table_name>\G
```

用途：查看完整建表结构。

```bash
grep -R "<column_name>" .
```

用途：在项目中查找字段引用位置。

## 关键日志

```text
Unknown column '<column_name>' in 'field list'
```

含义：SQL 引用了当前数据库表中不存在的字段。

## 解决步骤

1. 确认应用连接的数据库名。
2. 对比测试库和部署库表结构。
3. 查找是否有未执行迁移文件。
4. 在测试库验证补字段 SQL。
5. 生产变更前备份并评估影响。

## 验证方式

- `DESCRIBE` 能看到目标字段。
- 接口不再报 Unknown column。
- 新旧功能写入和查询都正常。

## 可复用经验

字段缺失常见于“代码已更新，数据库迁移未执行”，排查时要确认代码版本和数据库结构版本一起看。

## 禁止事项

- 不在生产库直接试错加字段。
- 不保存真实表数据。
- 不绕过字段校验让接口吞掉错误。

## 后续可补充内容

- Laravel migration 案例。
- Flyway/Liquibase 迁移案例。

