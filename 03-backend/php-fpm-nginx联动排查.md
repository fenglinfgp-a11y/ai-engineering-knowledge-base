# PHP-FPM Nginx 联动排查

来源类型：官方文档 / 实战总结  
参考来源：PHP-FPM 官方文档、Nginx FastCGI 文档

## 问题场景

Nginx 返回 502、504、File not found，PHP-FPM 服务运行但接口不可用，或 socket / TCP 配置不一致。

## 适用范围

- 适用于 Nginx 通过 FastCGI 转发到 PHP-FPM。
- 适用于 Ubuntu 常见 PHP 版本共存环境。
- 不保存真实域名、客户路径和生产配置。

## 判断方法

- `fastcgi_pass` 是 Unix socket 还是 TCP 端口。
- PHP-FPM pool 的 `listen` 是否与 Nginx 配置一致。
- Nginx worker 是否有权限访问 socket。
- `root`、`SCRIPT_FILENAME`、`try_files` 是否指向正确入口。
- PHP-FPM 是否因为慢请求或资源限制返回超时。

## 操作步骤

```bash
sudo nginx -t
```

用途：检查 Nginx 配置语法。风险：只检查语法，不代表后端可用。

```bash
systemctl status php*-fpm --no-pager
```

用途：查看 PHP-FPM 服务状态。

```bash
grep -R "fastcgi_pass\\|SCRIPT_FILENAME" /etc/nginx/sites-enabled/
```

用途：检查 Nginx FastCGI 转发目标和脚本路径配置。

```bash
grep -R "^listen" /etc/php/*/fpm/pool.d/
```

用途：检查 PHP-FPM pool 实际监听 socket 或端口。

```bash
tail -n 200 /var/log/nginx/error.log
```

用途：查看 Nginx 是否报 socket 权限、upstream 超时或脚本路径错误。

## 验证方法

- `nginx -t` 通过。
- PHP-FPM 服务 active。
- `fastcgi_pass` 与 PHP-FPM `listen` 一致。
- 本机访问 PHP 测试路由返回预期状态码。
- 错误日志不再出现 connection refused、permission denied、Primary script unknown。

## 注意事项

- `systemctl restart php*-fpm` 会中断正在处理的 PHP 请求，生产环境需评估影响。
- 不建议为修复 socket 权限直接给目录 `777`。
- 记录路径时使用 `/var/www/<site>` 等占位符。

## 相关命令

`nginx -t`、`systemctl status php*-fpm`、`grep -R fastcgi_pass`、`tail -n`。

## 参考来源

- https://www.php.net/manual/en/install.fpm.php
- https://nginx.org/en/docs/http/ngx_http_fastcgi_module.html

