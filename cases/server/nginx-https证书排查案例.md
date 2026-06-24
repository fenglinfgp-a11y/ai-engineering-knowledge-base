# Nginx HTTPS 证书排查案例

来源类型：官方文档 / 实战总结

## 问题现象

访问 `https://<example-domain>` 提示证书过期、证书不受信任、HTTPS 无法连接，或浏览器显示连接不安全。

## 环境背景

- 系统：Ubuntu `<version>`
- Web 服务：Nginx
- 域名：`<example-domain>`
- 证书路径：`/etc/nginx/ssl/<example-domain>/fullchain.pem`
- 私钥路径：`/etc/nginx/ssl/<example-domain>/privkey.pem`

## 初步判断

先判断证书是否过期、域名是否匹配、证书链是否完整、Nginx 是否加载了正确路径。

## 排查命令

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法，不会重载服务。

```bash
openssl x509 -in /etc/nginx/ssl/<example-domain>/fullchain.pem -noout -dates -subject -issuer
```

用途：查看证书有效期、签发者和证书主体。

```bash
openssl s_client -connect <example-domain>:443 -servername <example-domain> </dev/null
```

用途：从客户端角度检查 443 端口返回的证书链。

```bash
sudo systemctl reload nginx
```

用途：重载 Nginx 配置。风险：如果配置错误会影响线上访问，必须先执行 `nginx -t`。

## 关键日志

```text
SSL_CTX_use_PrivateKey_file("/etc/nginx/ssl/<example-domain>/privkey.pem") failed
```

含义：Nginx 无法读取私钥文件，可能是路径错误、权限错误或私钥文件损坏。

## 解决步骤

1. 用 `openssl x509` 确认证书未过期且域名匹配。
2. 检查 Nginx 配置中的 `ssl_certificate` 和 `ssl_certificate_key` 路径。
3. 用 `nginx -t` 验证配置语法。
4. 语法通过后 reload Nginx。
5. 用浏览器和 `openssl s_client` 再次确认返回的证书。

## 验证方式

- 浏览器访问不再提示证书错误。
- `openssl s_client` 返回的证书主体包含 `<example-domain>`。
- Nginx error log 不再出现 SSL 读取错误。

## 可复用经验

HTTPS 问题不要只看浏览器提示，要同时检查本地证书文件、Nginx 配置路径和外部实际返回证书。

## 禁止事项

- 不保存真实私钥内容。
- 不把证书私钥提交到 Git。
- 不用关闭 HTTPS 校验的方式掩盖证书问题。

## 后续可补充内容

- Let's Encrypt 自动续期失败案例。
- 多域名 SAN 证书配置案例。

