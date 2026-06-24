# GitHub 同步流程

来源类型：实战总结 / 我的项目经验

## 问题场景

本地知识库更新后，需要安全提交并同步到 GitHub，不改历史、不泄露凭据。

## 适用范围

适用于本知识库的日常 commit 和 push。

## 判断方法

- 本地工作区是否干净。
- 是否已经 commit。
- 是否已有正确的 `origin`。
- 命令行凭据是否可用；不可用时使用 GitHub Desktop。

## 操作步骤

1. 检查远程地址。
2. 检查分支和状态。
3. 执行 Markdown 空文件检查。
4. `git add .`。
5. `git commit -m "<message>"`。
6. 如果命令行凭据可用，执行 `git push`。
7. 如果命令行凭据不可用，打开 GitHub Desktop，选择当前仓库，点击 `Push origin`。
8. 推送后用 `git ls-remote --heads origin main` 验证远端 commit。

## 相关命令

```bash
git remote -v
```

用途：查看 GitHub 远程地址。

```bash
git status --short --branch
```

用途：查看本地分支和工作区状态。

```bash
git ls-remote --heads origin main
```

用途：查看远端 main 分支当前 commit。

```bash
git push
```

用途：把本地提交推送到远端。风险：如果凭据未配置会失败；不要使用 `--force`。

## 验证方法

- 本地 `git log --oneline -1` 与远端 `git ls-remote --heads origin main` hash 一致。
- `git status --short --branch` 显示本地和远端对齐。
- GitHub 页面能看到最新提交。

## 注意事项

- 不要 force push。
- 不要重写历史。
- 不要在聊天中输入 Token。
- 不要操作 `~/.ssh`，除非用户单独明确要求。

## 后续可补充内容

- GitHub Desktop 截图版推送指南。
- 命令行 credential helper 配置说明。

