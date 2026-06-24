# Codex 协作流程

来源类型：实战总结 / 我的项目经验

## 问题场景

以后让 Codex 维护本知识库时，需要避免误改业务项目、扩大范围、写入敏感信息或忘记更新索引。

## 适用范围

适用于本仓库内的文档新增、案例整理、索引更新、Git 提交前检查。

## 判断方法

- 当前目录必须是 `/Users/dora/Documents/ai-engineering-knowledge-base`。
- 任务必须属于知识库维护。
- 新增案例必须是脱敏后的通用经验。

## 操作步骤

1. 让 Codex 先执行 `pwd` 和 `git status --short --branch`。
2. 明确只允许修改本知识库。
3. 明确禁止保存密码、Token、密钥、客户资料、真实数据库数据。
4. 要求新增文档包含来源类型和固定结构。
5. 要求所有命令写用途，危险命令写风险。
6. 要求更新 `99-index/`。
7. 要求执行 `find . -name "*.md" -size 0`。
8. 提交前要求输出 `git status`。
9. 需要 push 时，优先用 GitHub Desktop，避免在对话中输入 Token。

## 相关命令

```bash
pwd
```

用途：确认当前目录，避免误改其他项目。

```bash
git status --short --branch
```

用途：查看当前分支和工作区状态。

```bash
find . -name "*.md" -size 0
```

用途：检查空 Markdown 文件。

## 验证方法

- Codex 最终输出新增文件、修改文件、检查结果、commit hash。
- 工作区没有无关业务项目变更。
- 新增文档已进入索引。

## 注意事项

- 不让 Codex 输入账号、密码、Token。
- 不让 Codex force push、改历史、操作 `~/.ssh`，除非用户单独明确授权。
- 不确定的内容先只分析，不写入。

## 后续可补充内容

- 增加 Codex 维护任务 checklist。
- 增加“只分析不提交”流程。

