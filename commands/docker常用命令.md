# Docker 常用命令

来源类型：官方文档 / 实战总结

## 问题场景

容器无法启动、端口不通、镜像构建失败、日志报错、数据卷异常。

## 适用范围

适用于 Docker 和 Docker Compose 的合法部署排查。

## 判断方法

先看容器状态，再看日志，再看端口映射和挂载目录。

## 操作步骤

```bash
docker ps
```

用途：查看运行中的容器。

```bash
docker ps -a
```

用途：查看所有容器，包括退出状态。

```bash
docker logs --tail=200 <container_name>
```

用途：查看容器最近 200 行日志。

```bash
docker compose up -d
```

用途：按 Compose 文件后台启动服务。风险：可能创建、重建或重启容器。

```bash
docker compose down
```

用途：停止并移除 Compose 创建的容器和网络。风险：会中断服务。

```bash
docker inspect <container_name>
```

用途：查看容器配置、网络、挂载等详细信息。

## 相关命令

常用组合：`docker ps -a`、`docker logs`、`docker inspect`、`docker compose up -d`、`docker compose down`。

## 验证方法

容器状态为 Up，端口可访问，日志没有启动异常。

## 注意事项

删除数据卷会造成数据丢失，执行前必须确认。

## 后续可补充内容

- Compose 环境变量排查。
- 容器网络排查。

