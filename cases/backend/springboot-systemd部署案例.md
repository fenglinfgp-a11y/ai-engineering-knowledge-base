# Spring Boot systemd 部署案例

来源类型：官方文档 / 实战总结

## 问题现象

手动 `java -jar` 可以启动，但配置成 systemd 服务后启动失败或无法读取配置。

## 环境背景

- 系统：Ubuntu `<version>`
- JDK：`<jdk-version>`
- 服务名：`<service-name>`
- jar 路径：`/opt/<app-name>/<app-name>.jar`
- 运行用户：`<deploy-user>`

## 初步判断

systemd 与手动启动的差异通常来自工作目录、运行用户、环境变量、文件权限和日志路径。

## 排查命令

```bash
sudo systemctl status <service-name>
```

用途：查看服务状态和最近启动错误。

```bash
journalctl -u <service-name> -n 200 --no-pager
```

用途：查看服务最近 200 行日志。

```bash
ls -lah /opt/<app-name>/
```

用途：检查 jar、配置文件和日志目录权限。

```bash
sudo systemctl restart <service-name>
```

用途：重启服务。风险：会中断服务，生产环境执行前确认维护窗口。

## 关键日志

```text
Failed at step EXEC spawning /usr/bin/java: Permission denied
```

含义：systemd 无法执行 Java 或访问配置，可能是路径、权限或服务文件配置错误。

## 解决步骤

1. 确认 `ExecStart` 中 Java 和 jar 路径存在。
2. 确认 `WorkingDirectory` 指向应用目录。
3. 确认运行用户能读取 jar 和配置文件。
4. 用 `systemctl daemon-reload` 让服务文件变更生效。
5. 重启服务并查看 `journalctl`。

## 验证方式

- `systemctl status` 显示 active。
- 健康检查接口返回 200。
- 重启服务器后服务能自动启动。

## 可复用经验

systemd 问题要对比“手动启动环境”和“服务启动环境”，尤其是工作目录和环境变量。

## 禁止事项

- 不在 service 文件中写真实数据库密码。
- 不用 root 用户运行所有服务来掩盖权限问题。
- 不重写历史提交。

## 后续可补充内容

- systemd service 模板。
- 环境变量文件加载案例。

