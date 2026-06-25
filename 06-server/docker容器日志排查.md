# Docker 容器日志排查

来源类型：官方文档 / 实战总结  
参考来源：Docker 官方文档

## 问题场景

容器启动后退出、接口不可访问、日志刷屏、Compose 服务状态异常，或需要查看容器最近错误。

## 适用范围

- 适用于 Docker 单容器和 Docker Compose 日志排查。
- 适用于测试环境、预发环境和生产环境只读诊断。
- 不保存镜像仓库密码、环境变量密钥或客户数据。

## 判断方法

- `docker ps -a` 看容器是否退出和退出码。
- `docker logs` 看应用标准输出和错误。
- `docker inspect` 看端口、挂载、环境变量占位符。
- Compose 项目看服务名和依赖顺序。
- 区分镜像启动失败、应用启动失败和端口映射失败。

## 操作步骤

```bash
docker ps -a
```

用途：查看所有容器状态、名称和退出情况。

```bash
docker logs --tail 200 <container_name>
```

用途：查看容器最近 200 行日志。

```bash
docker logs -f <container_name>
```

用途：实时跟踪容器日志。风险：日志量大时会刷屏，排查完及时停止。

```bash
docker inspect <container_name>
```

用途：查看容器端口、挂载、网络和环境变量。注意输出可能包含敏感环境变量，沉淀前必须脱敏。

```bash
docker compose logs --tail 200 <service_name>
```

用途：查看 Compose 指定服务日志。

## 验证方法

- 容器状态为 Up。
- 日志中没有持续出现启动异常、端口冲突、连接失败。
- 容器内服务端口和宿主机映射符合预期。
- 本机 `curl` 能访问映射端口。

## 注意事项

- `docker rm`、`docker volume rm`、`docker compose down -v` 可能删除容器或数据卷，属于高风险操作。
- `docker inspect` 输出中的环境变量必须脱敏。
- 日志中若有 Token、手机号、邮箱、订单号，不能直接写入文档。

## 相关命令

`docker ps -a`、`docker logs`、`docker inspect`、`docker compose logs`、`curl -i`。

## 参考来源

- https://docs.docker.com/reference/cli/docker/container/logs/
- https://docs.docker.com/compose/reference/logs/

