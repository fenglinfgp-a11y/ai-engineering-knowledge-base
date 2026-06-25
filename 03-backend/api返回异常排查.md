# API 返回异常排查

来源类型：官方文档 / 实战总结  
参考来源：Spring Boot 官方文档、PHP 官方文档、Nginx 官方文档

## 问题场景

接口返回 400、401、403、404、500，或返回 JSON 结构不符合前端预期，导致页面报错。

## 适用范围

- 适用于后端 API 的状态码、响应体、请求参数、代理链路排查。
- 适用于 Spring Boot、PHP、Node 等通用 HTTP 接口。
- 不记录真实用户资料、Token、订单号或客户请求体。

## 判断方法

- 先用同一个脱敏请求稳定复现。
- 看 HTTP 状态码，再看响应体错误码。
- 检查请求方法、路径、请求头、Content-Type 和参数。
- 查后端日志第一处异常。
- 若经过 Nginx，分别测试本机后端端口和外部域名。

## 操作步骤

```bash
curl -i -X GET "http://127.0.0.1:<port>/<api-path>"
```

用途：从服务器本机请求 API，查看状态码、响应头和响应体。

```bash
curl -i -H "Content-Type: application/json" -d '{"id":"<test_id>"}' "http://127.0.0.1:<port>/<api-path>"
```

用途：用脱敏 JSON 请求复现 POST 接口问题。

```bash
journalctl -u <service-name> -n 200 --no-pager
```

用途：查看后端服务日志。

```bash
tail -n 200 /var/log/nginx/access.log
```

用途：查看 Nginx 访问日志，确认请求是否到达代理层。

```bash
tail -n 200 /var/log/nginx/error.log
```

用途：查看 Nginx 错误日志，判断是否代理失败。

## 验证方法

- 同一脱敏请求返回稳定且符合接口约定。
- 后端日志无未处理异常。
- 前端 Network 中状态码、响应体结构与后端直连一致。
- Nginx 外部访问和本机后端访问差异可解释。

## 注意事项

- 401/403 优先排查认证、权限和 Token 过期，不记录真实 Token。
- 500 优先查服务端日志，避免只根据前端报错猜测。
- 不把客户请求体、手机号、邮箱、订单号写入案例。

## 相关命令

`curl -i`、`journalctl -u`、`tail -n`、`grep`。

## 参考来源

- https://docs.spring.io/spring-boot/docs/current/reference/html/web.html
- https://www.php.net/manual/en/errorfunc.configuration.php
- https://nginx.org/en/docs/http/ngx_http_proxy_module.html

