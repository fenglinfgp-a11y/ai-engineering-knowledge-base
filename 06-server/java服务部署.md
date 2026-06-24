# Java 服务部署

来源类型：官方文档 / 实战总结  
参考来源：Spring Boot 官方文档、systemd 文档

## 问题场景

Java 服务需要作为后台服务运行，并支持开机启动、日志查看、平滑更新。

## 判断方法

- JDK 版本是否正确。
- jar 路径、配置文件、运行用户是否正确。
- systemd 服务是否能启动。

## 操作步骤

```bash
java -jar /opt/app/app.jar --spring.profiles.active=prod
```

用途：手动启动 jar，验证配置和运行环境。

```bash
sudo systemctl status <service-name>
```

用途：查看 Java 服务运行状态。

```bash
journalctl -u <service-name> -n 200 --no-pager
```

用途：查看服务最近 200 行日志。

```bash
sudo systemctl restart <service-name>
```

用途：重启 Java 服务。风险：会中断正在处理的请求，生产环境需要维护窗口或滚动发布。

## 验证方法

- 服务状态为 active。
- 应用健康检查接口返回 200。
- 日志出现启动成功信息。
- 重启后端口仍正常监听。

## 注意事项

- jar、配置、日志目录权限要归属运行用户。
- 不要在 systemd 文件中写真实数据库密码，优先使用受控环境变量或配置文件权限保护。

## 适用范围

Ubuntu、Nginx、Docker、Java 服务和防火墙等服务器部署排查。

## 相关命令

`ss -lntp`、`nginx -t`、`systemctl status`、`docker ps`、`ufw status`。

## 后续可补充内容

补充不同技术栈部署案例和回滚流程。

