# Ubuntu 防火墙端口排查

来源类型：官方文档 / 实战总结  
参考来源：Ubuntu Server 官方文档

## 问题场景

服务本机可访问，但外网访问不通；端口已监听但客户端连接超时；部署后忘记开放防火墙端口。

## 适用范围

- 适用于 Ubuntu Server UFW、防火墙规则和服务监听排查。
- 适用于测试环境和生产环境只读确认。
- 不记录真实服务器 IP 和客户域名。

## 判断方法

- 服务是否监听在 `0.0.0.0`、`127.0.0.1` 或指定内网 IP。
- UFW 是否启用。
- 端口规则是否允许目标 TCP/UDP 协议。
- 云厂商安全组是否也放行。
- Nginx 或应用是否绑定了正确端口。

## 操作步骤

```bash
ss -lntp | grep <port>
```

用途：确认服务端口是否监听以及监听地址。

```bash
sudo ufw status verbose
```

用途：查看 UFW 状态和规则。

```bash
curl -i http://127.0.0.1:<port>
```

用途：从服务器本机验证服务可访问。

```bash
sudo ufw allow <port>/tcp
```

用途：开放指定 TCP 端口。风险：会改变服务器暴露面，执行前确认端口用途和来源限制。

```bash
sudo ufw delete allow <port>/tcp
```

用途：删除指定放行规则。风险：可能导致线上服务不可访问，执行前确认无业务依赖。

## 验证方法

- `ss` 显示服务监听正确地址和端口。
- `ufw status` 显示端口规则符合预期。
- 本机访问和外部测试访问都返回预期结果。
- 云安全组与系统防火墙规则一致。

## 注意事项

- 防火墙放行不是唯一条件，云安全组和应用绑定地址也会影响访问。
- 不要为了排查直接关闭全部防火墙。
- 对管理端口应限制来源 IP，不建议开放到全网。

## 相关命令

`ss -lntp`、`ufw status verbose`、`ufw allow`、`ufw delete`、`curl -i`。

## 参考来源

- https://documentation.ubuntu.com/server/how-to/security/firewalls/

