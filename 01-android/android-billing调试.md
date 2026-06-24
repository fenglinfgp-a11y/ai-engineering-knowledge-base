# Android Billing 合法调试

来源类型：官方文档 / 实战总结  
参考来源：Google Play Billing 官方文档、Play Console License Testing 文档

## 问题场景

接入 Google Play Billing 后，测试商品无法拉取、购买流程失败、订单状态与服务端不一致。

## 判断方法

- App 包名是否与 Play Console 配置一致。
- 测试账号是否加入 License testers 或测试轨道。
- 商品 ID 是否已在 Play Console 创建并激活。
- Billing Library 日志是否返回明确错误码。

## 操作步骤

```bash
adb logcat | grep -i "BillingClient\\|billing\\|Purchase"
```

用途：查看 BillingClient 连接、商品查询、购买回调相关日志。

```bash
adb shell dumpsys package <your.package.name> | grep version
```

用途：确认当前安装包的版本号和版本名，避免测试了旧包。

```bash
adb uninstall <your.package.name>
```

用途：卸载测试包，清理本地安装状态后重新安装。风险：会删除该 App 本地数据，执行前确认测试数据可丢弃。

## 验证方法

- `BillingClient` 能成功连接。
- 商品查询能返回配置的 productId。
- 测试购买能在 Play Console 或回调日志中看到对应状态。
- 服务端只接受 Google Play 合法签名和购买令牌验证结果。

## 注意事项

- 只使用 Google 官方测试轨道、License tester、Play Billing Lab 等合法测试方式。
- 不记录伪造订单、绕过支付、修改购买状态的内容。
- 客户端日志只用于定位问题，最终订单可信状态应以后端校验为准。

## 适用范围

Android 授权测试、构建、日志分析和合法调试场景。

## 相关命令

`adb devices`、`adb logcat`、`./gradlew tasks`。

## 后续可补充内容

补充脱敏日志片段、版本兼容矩阵和合法测试流程截图说明。

