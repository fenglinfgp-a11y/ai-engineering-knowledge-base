# PHP-FPM 异常排查案例

来源类型：官方文档 / 实战总结

## 问题现象

Nginx 返回 502 或 504，PHP 页面无法访问，PHP-FPM 进程异常或 socket 不存在。

## 环境背景

- PHP-FPM：`php<version>-fpm`
- Nginx 配置：`fastcgi_pass unix:/run/php/php<version>-fpm.sock`
- 项目目录：`/var/www/<app-name>`

## 初步判断

优先判断 PHP-FPM 服务是否运行、socket 路径是否匹配、进程池是否耗尽、文件权限是否正确。

## 排查命令

```bash
sudo systemctl status php<version>-fpm
```

用途：查看 PHP-FPM 服务状态。

```bash
ls -lah /run/php/
```

用途：查看 PHP-FPM socket 文件是否存在。

```bash
sudo tail -n 100 /var/log/php<version>-fpm.log
```

用途：查看 PHP-FPM 最近日志。风险：日志可能包含路径和错误细节，外发前要脱敏。

```bash
sudo systemctl restart php<version>-fpm
```

用途：重启 PHP-FPM。风险：会中断 PHP 请求，生产环境需确认维护窗口。

## 关键日志

```text
connect() to unix:/run/php/php<version>-fpm.sock failed (2: No such file or directory)
```

含义：Nginx 配置的 socket 路径不存在，可能 PHP-FPM 未运行或版本路径不一致。

## 解决步骤

1. 查看 PHP-FPM 服务状态。
2. 确认 Nginx `fastcgi_pass` 路径与实际 socket 一致。
3. 检查 PHP-FPM 日志。
4. 修改配置后先执行 `nginx -t`。
5. 依次重启 PHP-FPM 和 reload Nginx。

## 验证方式

- PHP-FPM 服务 active。
- socket 文件存在。
- PHP 页面返回 200。
- Nginx error log 不再出现 socket 连接错误。

## 可复用经验

PHP-FPM 502 很多不是业务代码问题，而是 Nginx 与 PHP-FPM 的 socket 或端口配置不一致。

## 禁止事项

- 不随意提高进程数到极大值，避免耗尽内存。
- 不记录真实项目路径中的客户名称。
- 不在未检查配置语法前 reload Nginx。

## 后续可补充内容

- PHP-FPM 进程池耗尽案例。
- slowlog 分析案例。

