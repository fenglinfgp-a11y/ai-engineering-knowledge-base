# Nginx 502 排查案例

来源类型：实战总结

## 问题场景

Nginx 作为反向代理时，请求已经到达网关，但后端 upstream 无法正常响应。

## 问题现象

访问 `https://<example-domain>` 返回 502 Bad Gateway，静态页面可能正常，但接口代理失败。

## 环境背景

- 系统：Ubuntu `<version>`
- 网关：Nginx
- 后端：运行在 `127.0.0.1:<backend-port>` 的服务
- 域名、IP、客户名称均使用占位符，不记录真实信息。

## 初步判断

优先判断后端服务是否未启动、端口错误、Nginx upstream 配置错误或后端超时。

## 排查命令

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法，不重载服务。

```bash
ss -lntp | grep <backend-port>
```

用途：确认后端端口是否监听。

```bash
curl -i http://127.0.0.1:<backend-port>/health
```

用途：绕过 Nginx，直接验证后端本机接口是否正常。

```bash
sudo tail -n 100 /var/log/nginx/error.log
```

用途：查看 Nginx 最近错误日志。风险：日志可能包含路径或请求参数，外发前必须脱敏。

## 关键日志

```text
connect() failed (111: Connection refused) while connecting to upstream
```

含义：Nginx 能收到请求，但连接后端 upstream 被拒绝，通常是后端未监听或端口不一致。

## 解决步骤

1. 确认后端服务是否启动。
2. 确认 Nginx 配置中的 upstream 端口与后端监听端口一致。
3. 执行 `nginx -t` 检查配置。
4. 配置通过后再 reload Nginx。

```bash
sudo systemctl reload nginx
```

用途：重载 Nginx 配置。风险：错误配置可能影响线上访问，必须先通过 `nginx -t`。

## 验证方式

- `curl -i http://127.0.0.1:<backend-port>/health` 返回 200。
- `curl -I https://<example-domain>` 不再返回 502。
- Nginx error log 不再出现同类 upstream 错误。

## 可复用经验

502 排查顺序：Nginx 配置语法 -> 后端端口监听 -> 本机 curl 后端 -> Nginx error log -> 防火墙和超时。

## 禁止事项

- 不记录真实域名、真实 IP、客户名称。
- 不用修改绕过鉴权或伪造请求的方式解决接口问题。
- 不在未验证配置语法前 reload。

## 适用范围

适用于合法运维场景下的 Nginx 反向代理 502 排查。

## 判断方法

如果直接访问后端失败，优先修后端；如果后端正常但域名失败，优先查 Nginx 配置和日志。

## 操作步骤

见“排查命令”和“解决步骤”。

## 验证方法

见“验证方式”。

## 注意事项

生产环境变更前要记录当前配置并准备回滚方式。

## 相关命令

核心命令：`nginx -t`、`ss -lntp`、`curl -i`、`tail -n`、`systemctl reload nginx`。

## 后续可补充内容

- upstream 超时案例。
- PHP-FPM socket 502 案例。
- Docker 网络导致的 502 案例。
