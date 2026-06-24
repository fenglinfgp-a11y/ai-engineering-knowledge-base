# Ubuntu 环境初始化

来源类型：官方文档 / 实战总结  
参考来源：Ubuntu Server 官方文档

## 问题场景

新服务器需要初始化基础环境，用于部署 Web、Java、PHP、Docker 等服务。

## 判断方法

- 系统版本是否受支持。
- 是否创建普通用户和 sudo 权限。
- 防火墙、时间、软件源是否正常。

## 操作步骤

```bash
lsb_release -a
```

用途：查看 Ubuntu 发行版本。

```bash
sudo apt update
```

用途：更新软件包索引，不升级软件本体。

```bash
sudo apt upgrade
```

用途：升级已安装软件包。风险：可能升级内核或服务依赖，生产环境执行前先确认维护窗口。

```bash
timedatectl
```

用途：查看系统时间、时区和 NTP 状态。

```bash
sudo ufw status
```

用途：查看防火墙状态。

## 验证方法

- 软件源更新无错误。
- 时间和时区符合部署要求。
- SSH 连接稳定。
- 防火墙规则与开放端口一致。

## 注意事项

- 不要把 root 密码、SSH 私钥、服务器公网 IP 写入知识库。
- 生产服务器初始化前先确认备份和回滚方式。

## 适用范围

Ubuntu、Nginx、Docker、Java 服务和防火墙等服务器部署排查。

## 相关命令

`ss -lntp`、`nginx -t`、`systemctl status`、`docker ps`、`ufw status`。

## 后续可补充内容

补充不同技术栈部署案例和回滚流程。

