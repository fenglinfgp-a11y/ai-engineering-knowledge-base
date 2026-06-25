# Nginx 反向代理完整排查

来源类型：官方文档 / 实战总结  
参考来源：Nginx 官方文档

## 问题场景

域名访问 502、504、404，静态资源正常但 API 不通，或后端本机可访问但经过 Nginx 后异常。

## 适用范围

- 适用于 Nginx 反向代理到 Spring Boot、PHP、Node、Docker 服务。
- 适用于 Ubuntu Server 上的只读排查和低风险重载。
- 不记录真实域名、客户路径和生产配置。

## 判断方法

- 先确认 Nginx 配置语法。
- 再确认 upstream 后端端口本机可访问。
- 查 Nginx access log 和 error log。
- 对比本机直连后端和外部域名访问结果。
- 检查 `proxy_pass` 路径斜杠、Host 头、超时配置。

## 操作步骤

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法，不会重载服务。

```bash
ss -lntp | grep <port>
```

用途：确认后端服务端口是否监听。

```bash
curl -i http://127.0.0.1:<port>/<health-path>
```

用途：从服务器本机直连后端接口。

```bash
curl -I http://<example-domain>/<path>
```

用途：验证经过 Nginx 后的响应头和状态码。

```bash
tail -n 200 /var/log/nginx/error.log
```

用途：查看代理错误，例如 connection refused、timeout、upstream prematurely closed。

```bash
sudo systemctl reload nginx
```

用途：重载 Nginx 配置。风险：必须先通过 `nginx -t`，错误配置可能影响访问。

## 验证方法

- 本机后端健康接口可访问。
- Nginx 域名访问返回预期状态码。
- error log 不再出现同类 upstream 错误。
- access log 能看到请求到达且状态码符合预期。

## 注意事项

- `proxy_pass` 是否带尾部 `/` 会影响路径拼接。
- 502 多看后端是否存活，504 多看超时和慢接口。
- 不把真实域名、证书路径、客户接口路径写入知识库。

## 相关命令

`nginx -t`、`ss -lntp`、`curl -i`、`tail -n`、`systemctl reload nginx`。

## 参考来源

- https://nginx.org/en/docs/http/ngx_http_proxy_module.html
- https://nginx.org/en/docs/beginners_guide.html

