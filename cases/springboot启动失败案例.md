# Spring Boot 启动失败案例

来源类型：实战总结

## 问题场景

Spring Boot 服务部署后无法启动或启动后立刻退出，需要从环境、配置和日志定位原因。

## 问题现象

执行 `java -jar` 后服务退出，端口没有监听，前端或 Nginx 无法访问接口。

## 环境背景

- JDK：`<jdk-version>`
- Spring Boot：`<spring-boot-version>`
- 启动端口：`<port>`
- 配置文件：`application-<profile>.yml`

## 初步判断

优先检查 JDK 版本、端口占用、配置文件、数据库连接、缺失环境变量。

## 排查命令

```bash
java -version
```

用途：确认 JDK 版本。

```bash
java -jar target/app.jar --spring.profiles.active=<profile>
```

用途：前台启动服务，直接查看启动日志。

```bash
ss -lntp | grep <port>
```

用途：查看目标端口是否已被占用或启动成功。

```bash
journalctl -u <service-name> -n 200 --no-pager
```

用途：查看 systemd 托管服务最近日志。

## 关键日志

```text
APPLICATION FAILED TO START
Port <port> was already in use
```

含义：端口已被其他进程占用，需要确认占用进程和部署端口。

## 解决步骤

1. 根据第一条 `Caused by` 定位根因。
2. 如果端口冲突，确认旧进程是否应停止或改端口。
3. 如果配置缺失，补充测试环境配置占位符，不写真实密码。
4. 前台启动验证通过后，再交给 systemd 管理。

## 验证方式

- 日志出现 `Started ... in ... seconds`。
- `ss -lntp` 能看到 Java 进程监听目标端口。
- `curl -i http://127.0.0.1:<port>/health` 返回预期结果。

## 可复用经验

Spring Boot 启动失败不要只看最后一屏日志，要找第一处 `Caused by` 和 `APPLICATION FAILED TO START`。

## 禁止事项

- 不把真实数据库密码写入命令或文档。
- 不用强杀进程代替根因排查。
- 不在未确认影响时重启生产服务。

## 适用范围

适用于合法部署环境和测试环境下的 Spring Boot 服务启动排查。

## 判断方法

如果前台启动能成功但 systemd 失败，优先查服务文件的工作目录、运行用户和环境变量。

## 操作步骤

见“排查命令”和“解决步骤”。

## 验证方法

见“验证方式”。

## 注意事项

`kill -9` 是高风险操作，可能中断请求和日志写入，生产环境慎用。

## 相关命令

核心命令：`java -version`、`java -jar`、`ss -lntp`、`journalctl`、`curl -i`。

## 后续可补充内容

- 数据库连接失败案例。
- 配置 profile 未生效案例。
- systemd 环境变量缺失案例。
