# PHP 后端

来源类型：官方文档 / 实战总结  
参考来源：PHP 官方文档、Composer 官方文档

## 问题场景

PHP 后台无法启动、接口 500、依赖缺失、运行目录或权限错误。

## 判断方法

- 是否有 `composer.json`。
- 入口文件是否在 `public/index.php` 或框架指定目录。
- PHP 版本和扩展是否满足项目要求。
- Web 服务器错误日志是否有 fatal error。

## 操作步骤

```bash
php -v
```

用途：查看当前 PHP 版本，确认是否满足项目要求。

```bash
php -m
```

用途：查看已启用扩展，例如 `pdo_mysql`、`curl`、`mbstring`。

```bash
composer install
```

用途：按 `composer.lock` 安装依赖。风险：会写入 `vendor/`，生产环境执行前确认依赖来源可信。

```bash
php -S 127.0.0.1:8000 -t public
```

用途：用 PHP 内置服务器在本地测试入口目录，不适合作为生产服务。

## 验证方法

- `php -v` 与项目要求一致。
- `composer install` 无错误。
- 本地访问健康检查接口返回 200。
- Web 错误日志不再出现同类 fatal error。

## 注意事项

- `.env` 只保存示例，不提交真实数据库密码。
- 生产环境不要使用 PHP 内置服务器。

## 适用范围

后端服务结构、接口排查、日志分析和部署验证。

## 相关命令

`curl -i`、`tail -n`、`journalctl`、`systemctl status`。

## 后续可补充内容

补充接口错误码、日志关键字和部署失败案例。

