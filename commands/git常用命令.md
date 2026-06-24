# Git 常用命令

来源类型：官方文档 / 实战总结

## 问题场景

维护知识库或项目时，需要查看变更、提交记录、分支状态和提交文件。

## 适用范围

适用于本知识库和合法授权代码仓库的版本管理。

## 判断方法

先查看状态和 diff，再决定是否 add、commit、push。

## 操作步骤

```bash
git status --short
```

用途：查看当前工作区变更。

```bash
git diff --stat
```

用途：查看变更文件和行数概览。

```bash
git diff
```

用途：查看详细未暂存变更。

```bash
git add <file>
```

用途：暂存指定文件。

```bash
git commit -m "message"
```

用途：创建提交记录。

```bash
git log --oneline -5
```

用途：查看最近 5 条提交。

## 相关命令

常用组合：`git status --short`、`git diff --stat`、`git add`、`git commit`、`git log --oneline -5`。

## 验证方法

提交后 `git status --short` 没有未提交变更，`git log --oneline -1` 显示最新提交。

## 注意事项

不要用 `git reset --hard` 或 checkout 覆盖用户改动，除非用户明确要求。

## 后续可补充内容

- GitHub 远程仓库初始化流程。
- PR 检查清单。

