# Nginx 配置

来源类型：官方文档 / 实战总结  
参考来源：Nginx 官方文档

## 问题场景

域名无法访问、反向代理 502、静态资源 404、HTTPS 配置错误。

## 判断方法

- 配置语法是否通过。
- 上游服务是否运行。
- 域名解析是否指向服务器。
- Nginx 错误日志是否有明确原因。

## 操作步骤

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法，不重载服务。

```bash
sudo systemctl status nginx
```

用途：查看 Nginx 服务状态。

```bash
curl -I http://127.0.0.1
```

用途：查看本机 HTTP 响应头。

```bash
sudo systemctl reload nginx
```

用途：重载 Nginx 配置。风险：错误配置可能影响访问，必须先执行 `nginx -t`。

## 验证方法

- `nginx -t` 返回 successful。
- 本机和外网访问都返回预期状态码。
- 错误日志不再出现 upstream 或证书错误。

## 注意事项

- 502 通常先查后端服务是否运行和端口是否正确。
- HTTPS 证书路径、权限和过期时间都要检查。

## 适用范围

Ubuntu、Nginx、Docker、Java 服务和防火墙等服务器部署排查。

## 相关命令

`ss -lntp`、`nginx -t`、`systemctl status`、`docker ps`、`ufw status`。

## 后续可补充内容

补充不同技术栈部署案例和回滚流程。

