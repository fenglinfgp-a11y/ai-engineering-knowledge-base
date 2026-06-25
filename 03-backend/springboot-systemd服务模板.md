# Spring Boot systemd 服务模板

来源类型：官方文档 / 实战总结  
参考来源：Spring Boot 官方文档、Ubuntu Server systemd 资料

## 问题场景

Spring Boot jar 已能前台启动，但需要作为 Linux 服务托管，或服务开机不自启、重启失败、日志难以定位。

## 适用范围

- 适用于 Ubuntu Server 上托管 Spring Boot 可执行 jar。
- 适用于测试环境、预发环境和生产环境的服务模板整理。
- 不保存真实服务器 IP、用户名、密码、Token 或客户配置。

## 判断方法

- jar 文件路径是否固定且权限可读。
- 运行用户是否是最小权限用户，而不是直接使用 root。
- `WorkingDirectory` 是否指向应用目录。
- Java 路径、环境变量和配置文件路径是否明确。
- 服务日志是否能通过 `journalctl` 查看。

## 操作步骤

```ini
[Unit]
Description=<app-name> Spring Boot Service
After=network.target

[Service]
Type=simple
User=<app-user>
WorkingDirectory=/opt/<app-name>
ExecStart=/usr/bin/java -jar /opt/<app-name>/<app-name>.jar --spring.profiles.active=<profile>
Restart=on-failure
RestartSec=5
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```

用途：systemd 服务模板，路径、用户、应用名和 profile 必须使用实际测试环境的脱敏配置替换。

```bash
sudo systemctl daemon-reload
```

用途：重新加载 systemd 单元文件。风险：会让 systemd 重新读取服务定义，修改服务文件后再执行。

```bash
sudo systemctl start <service-name>
```

用途：启动指定服务。风险：可能占用端口或连接测试数据库，执行前确认配置环境。

```bash
sudo systemctl status <service-name>
```

用途：查看服务状态和最近日志摘要。

```bash
journalctl -u <service-name> -n 200 --no-pager
```

用途：查看服务最近 200 行日志。

## 验证方法

- `systemctl status` 显示服务为 active。
- `journalctl` 中出现 Spring Boot started 日志。
- `ss -lntp` 能看到 Java 进程监听预期端口。
- 健康检查接口返回 200 或业务约定的健康状态。

## 注意事项

- `systemctl restart` 会中断正在处理的请求，生产环境需要维护窗口。
- 不在 unit 文件中硬编码密码、Token、数据库真实连接串。
- 配置文件建议使用 `<DB_HOST>`、`<DB_NAME>` 等占位符记录。

## 相关命令

`systemctl daemon-reload`、`systemctl start`、`systemctl status`、`journalctl -u`、`ss -lntp`。

## 参考来源

- https://docs.spring.io/spring-boot/docs/current/reference/html/deployment.html
- https://documentation.ubuntu.com/server/

