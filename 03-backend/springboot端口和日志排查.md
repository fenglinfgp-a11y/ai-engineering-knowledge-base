# Spring Boot 端口和日志排查

来源类型：官方文档 / 实战总结  
参考来源：Spring Boot 官方文档、Ubuntu Server 文档

## 问题场景

Spring Boot 启动失败、端口被占用、服务启动了但接口访问不到，或者日志位置不清楚。

## 适用范围

- 适用于 jar 前台运行、systemd 托管、Nginx 反向代理后的端口排查。
- 适用于测试环境和生产环境的只读诊断。

## 判断方法

- 先确认应用配置的 `server.port`。
- 用 `ss` 确认端口是否监听。
- 用 `curl` 从本机访问，排除外网、域名和防火墙影响。
- 查第一处 `Caused by`，不要只看最后一行错误。
- 区分应用日志、systemd 日志和 Nginx 日志。

## 操作步骤

```bash
ss -lntp | grep <port>
```

用途：查看目标端口是否被进程监听。

```bash
curl -i http://127.0.0.1:<port>/actuator/health
```

用途：从服务器本机验证健康接口是否可访问。

```bash
journalctl -u <service-name> -n 300 --no-pager
```

用途：查看 systemd 托管服务的最近日志。

```bash
tail -n 200 /opt/<app-name>/logs/app.log
```

用途：查看应用文件日志最近 200 行。

```bash
java -jar /opt/<app-name>/<app-name>.jar --server.port=<port>
```

用途：前台启动 jar 观察完整启动日志。风险：可能启动第二个服务实例，执行前确认端口未被线上服务占用。

## 验证方法

- 端口监听进程与预期服务一致。
- 本机 `curl` 返回预期状态码。
- 日志中没有持续出现端口占用、数据库连接失败或配置解析错误。
- Nginx 代理访问与本机直连结果一致或能解释差异。

## 注意事项

- 不要直接 `kill -9` 线上进程，除非已确认影响并有回滚方案。
- Actuator 暴露范围要最小化，不能把敏感健康细节暴露到公网。
- 日志中出现真实连接串、用户信息时必须脱敏后再沉淀。

## 相关命令

`ss -lntp`、`curl -i`、`journalctl -u`、`tail -n`、`java -jar`。

## 参考来源

- https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html
- https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.logging

