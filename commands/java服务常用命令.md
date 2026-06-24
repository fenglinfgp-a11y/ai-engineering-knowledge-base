# Java 服务常用命令

来源类型：官方文档 / 实战总结

## 问题场景

Java 或 Spring Boot 服务启动失败、端口冲突、日志异常、systemd 托管失败。

## 适用范围

适用于 Java 后端服务部署和合法运维排查。

## 判断方法

先看 JDK 版本和启动日志，再确认端口、配置和 systemd 状态。

## 操作步骤

```bash
java -version
```

用途：查看 JDK 版本。

```bash
java -jar <app.jar> --spring.profiles.active=<profile>
```

用途：前台启动 jar，便于查看启动日志。

```bash
ss -lntp | grep <port>
```

用途：查看目标端口监听情况。

```bash
sudo systemctl status <service-name>
```

用途：查看 systemd 服务状态。

```bash
journalctl -u <service-name> -n 200 --no-pager
```

用途：查看服务最近 200 行日志。

```bash
sudo systemctl restart <service-name>
```

用途：重启服务。风险：会中断正在运行的服务，生产环境需确认维护窗口。

## 相关命令

常用组合：`java -version`、`java -jar`、`ss -lntp`、`systemctl status`、`journalctl`。

## 验证方法

日志显示启动成功，端口监听，健康检查返回预期结果。

## 注意事项

不要在命令中写真实数据库密码或 Token。

## 后续可补充内容

- JVM 参数排查。
- systemd service 文件模板。

