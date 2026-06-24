# Nginx 常用命令

来源类型：官方文档 / 实战总结

## 问题场景

排查 Nginx 配置、反向代理、静态资源、HTTPS 和服务状态。

## 适用范围

适用于合法服务器上的 Nginx 运维和部署排查。

## 判断方法

先检查配置语法，再重载；先检查后端服务，再判断 Nginx 问题。

## 操作步骤

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法。

```bash
sudo systemctl status nginx
```

用途：查看 Nginx 服务状态。

```bash
sudo systemctl reload nginx
```

用途：重载 Nginx 配置。风险：配置错误可能影响访问，必须先执行 `nginx -t`。

```bash
sudo tail -n 100 /var/log/nginx/error.log
```

用途：查看 Nginx 最近错误日志。

```bash
curl -I http://127.0.0.1
```

用途：查看本机 HTTP 响应头。

## 相关命令

常用组合：`nginx -t`、`systemctl status nginx`、`systemctl reload nginx`、`tail -n`、`curl -I`。

## 验证方法

配置检查通过，服务状态正常，请求返回预期状态码。

## 注意事项

reload 前必须确认配置语法通过。

## 后续可补充内容

- HTTPS 证书检查命令。
- upstream 超时参数说明。

