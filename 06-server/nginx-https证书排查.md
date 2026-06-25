# Nginx HTTPS 证书排查

来源类型：官方文档 / 实战总结  
参考来源：Nginx 官方文档、OpenSSL 公开工具文档

## 问题场景

浏览器提示证书过期、域名不匹配、证书链不完整，或 Nginx reload 后 HTTPS 站点不可访问。

## 适用范围

- 适用于 Nginx HTTPS 配置、证书路径、证书有效期和证书链检查。
- 适用于公开证书和测试域名排查。
- 不保存私钥、真实证书文件内容或客户域名。

## 判断方法

- `ssl_certificate` 和 `ssl_certificate_key` 路径是否存在。
- 证书是否过期。
- 证书域名是否覆盖访问域名。
- 证书链是否完整。
- Nginx worker 是否有权限读取证书和私钥。

## 操作步骤

```bash
sudo nginx -t
```

用途：检查 HTTPS 配置语法和证书文件可读性。

```bash
openssl x509 -in /etc/nginx/ssl/<example-domain>.crt -noout -dates -subject -issuer
```

用途：查看证书有效期、主体和签发者。

```bash
openssl s_client -connect <example-domain>:443 -servername <example-domain> </dev/null
```

用途：从客户端视角查看服务端返回的证书链。

```bash
grep -R "ssl_certificate" /etc/nginx/sites-enabled/
```

用途：定位 Nginx 站点引用的证书文件路径。

```bash
sudo systemctl reload nginx
```

用途：重载 Nginx。风险：证书路径或权限错误会影响 HTTPS 访问，必须先执行 `nginx -t`。

## 验证方法

- `nginx -t` 通过。
- 证书未过期，域名匹配。
- 浏览器不再提示证书错误。
- `openssl s_client` 输出中证书链验证结果符合预期。

## 注意事项

- 私钥内容不能写入知识库，也不能提交到 Git。
- 替换证书前保留旧证书路径和回滚方案。
- 证书自动续期任务需要单独验证，不只看当前文件。

## 相关命令

`nginx -t`、`openssl x509`、`openssl s_client`、`grep -R ssl_certificate`。

## 参考来源

- https://nginx.org/en/docs/http/configuring_https_servers.html
- https://nginx.org/en/docs/http/ngx_http_ssl_module.html

