# Nginx 反向代理 502 案例

来源类型：官方文档 / 实战总结

## 问题现象

访问 `https://<example-domain>/api` 返回 502，静态页面正常，后端接口不可用。

## 环境背景

- Nginx：监听 80/443
- 后端服务：`127.0.0.1:<backend-port>`
- 配置文件：`/etc/nginx/sites-enabled/<example-domain>.conf`

## 初步判断

502 常见原因是后端服务未启动、端口配置错误、upstream 超时、容器网络不通或 PHP-FPM socket 异常。

## 排查命令

```bash
ss -lntp | grep <backend-port>
```

用途：确认后端端口是否监听。

```bash
curl -i http://127.0.0.1:<backend-port>/health
```

用途：绕过 Nginx，直接访问后端健康接口。

```bash
sudo tail -n 100 /var/log/nginx/error.log
```

用途：查看 Nginx 最近错误日志。风险：日志可能包含请求路径或参数，外发前要脱敏。

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法。

## 关键日志

```text
connect() failed (111: Connection refused) while connecting to upstream
```

含义：Nginx 连接 upstream 被拒绝，通常是后端端口未监听或 upstream 地址写错。

## 解决步骤

1. 确认后端服务启动并监听目标端口。
2. 确认 Nginx `proxy_pass` 地址与后端监听地址一致。
3. 如果后端在 Docker 内，确认容器网络和暴露端口。
4. 执行 `nginx -t`。
5. 配置正确后 reload Nginx。

## 验证方式

- 本机 `curl` 后端健康接口返回 200。
- 域名接口不再返回 502。
- Nginx error log 不再出现同类 upstream 错误。

## 可复用经验

502 要先证明后端本身可访问，再排查 Nginx；如果后端本机都访问不了，先修后端。

## 禁止事项

- 不记录真实域名、IP、客户接口参数。
- 不在未检查配置语法前 reload Nginx。
- 不通过关闭鉴权或改业务逻辑绕过接口错误。

## 后续可补充内容

- upstream timeout 案例。
- Docker Compose 服务名代理案例。

