# Spring Boot 后端

来源类型：官方文档 / 实战总结  
参考来源：Spring Boot Reference Documentation

## 问题场景

Spring Boot 服务启动失败、端口冲突、配置未生效、打包后运行异常。

## 判断方法

- JDK 版本是否满足项目要求。
- `application.yml` 或环境变量是否正确。
- 端口是否被占用。
- 启动日志中第一处 `Caused by` 是什么。

## 操作步骤

```bash
java -version
```

用途：查看当前 JDK 版本。

```bash
./mvnw clean package -DskipTests
```

用途：使用 Maven Wrapper 打包项目并跳过测试。风险：跳过测试可能漏掉问题，交付前应至少跑一次测试。

```bash
java -jar target/app.jar --server.port=8080
```

用途：启动 Spring Boot 可执行 jar，并指定端口。

```bash
ss -lntp | grep 8080
```

用途：确认 8080 端口是否被监听。

## 验证方法

- 应用日志出现 `Started ... in ... seconds`。
- 健康接口或首页返回 200。
- `ss -lntp` 能看到 Java 进程监听目标端口。

## 注意事项

- `kill -9 <pid>` 会强制结束进程，可能中断请求或造成未写完日志，生产环境慎用。
- 配置优先级可能来自环境变量、命令行参数、配置文件，排查时要确认实际生效来源。

## 适用范围

后端服务结构、接口排查、日志分析和部署验证。

## 相关命令

`curl -i`、`tail -n`、`journalctl`、`systemctl status`。

## 后续可补充内容

补充接口错误码、日志关键字和部署失败案例。

