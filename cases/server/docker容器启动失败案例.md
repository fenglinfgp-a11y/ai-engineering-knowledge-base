# Docker 容器启动失败案例

来源类型：官方文档 / 实战总结

## 问题现象

执行 `docker compose up -d` 后容器不断退出，`docker ps` 看不到运行中的服务。

## 环境背景

- Docker：`<docker-version>`
- Compose 文件：`compose.yml`
- 服务名：`<service-name>`
- 镜像：`<image-name>:<tag>`

## 初步判断

优先看容器退出码、启动日志、环境变量、端口冲突和挂载目录权限。

## 排查命令

```bash
docker ps -a
```

用途：查看所有容器状态，包括已退出容器。

```bash
docker logs --tail=200 <container_name>
```

用途：查看容器最近 200 行日志。

```bash
docker inspect <container_name>
```

用途：查看容器环境变量、网络、挂载、退出码等详细信息。

```bash
docker compose up -d
```

用途：按 Compose 文件后台启动服务。风险：可能重建或重启容器，生产环境执行前确认变更。

## 关键日志

```text
permission denied: /app/data
```

含义：容器内进程无权读写挂载目录，常见于宿主机目录权限不匹配。

## 解决步骤

1. 用 `docker ps -a` 找到退出容器。
2. 用 `docker logs` 查看第一处报错。
3. 检查 `compose.yml` 的环境变量、端口和挂载。
4. 修正配置或目录权限。
5. 重新启动容器并观察日志。

## 验证方式

- `docker ps` 显示容器状态为 Up。
- 服务端口可以访问。
- 容器日志没有重复启动异常。

## 可复用经验

Docker 启动失败先看日志，不要直接反复重启；重复重启只会覆盖现场线索。

## 禁止事项

- 不提交真实 `.env`。
- 不删除数据卷来“快速恢复”，除非明确确认数据可丢弃。
- 不记录真实生产镜像仓库凭据。

## 后续可补充内容

- Compose 环境变量缺失案例。
- 容器端口冲突案例。

