# Docker 部署

来源类型：官方文档 / 实战总结  
参考来源：Docker 官方文档

## 问题场景

容器无法启动、端口映射错误、镜像构建失败、容器日志报错。

## 判断方法

- 镜像是否存在。
- 容器是否退出。
- 端口映射是否正确。
- 环境变量和挂载目录是否正确。

## 操作步骤

```bash
docker ps
```

用途：查看正在运行的容器。

```bash
docker ps -a
```

用途：查看所有容器，包括已退出容器。

```bash
docker logs --tail=200 <container_name>
```

用途：查看容器最近 200 行日志。

```bash
docker compose up -d
```

用途：按 `compose.yml` 后台启动服务。风险：可能创建、重建或重启容器，生产环境执行前确认变更。

```bash
docker compose down
```

用途：停止并移除 compose 创建的容器和网络。风险：会中断服务，可能影响线上访问。

## 验证方法

- `docker ps` 显示容器为 Up。
- 映射端口能 `curl` 访问。
- 容器日志无启动异常。
- 挂载目录中数据正常保留。

## 注意事项

- 不要提交 `.env` 中的真实密码。
- 删除容器不等于删除卷，但删除卷会造成数据丢失，执行前必须确认。

## 适用范围

Ubuntu、Nginx、Docker、Java 服务和防火墙等服务器部署排查。

## 相关命令

`ss -lntp`、`nginx -t`、`systemctl status`、`docker ps`、`ufw status`。

## 后续可补充内容

补充不同技术栈部署案例和回滚流程。

