# Billing PurchasesUpdatedListener 排查

来源类型：官方文档 / 实战总结  
参考来源：Google Play Billing 官方文档

## 问题场景

购买页面返回后没有收到 `PurchasesUpdatedListener` 回调，回调里 purchases 为空，或者用户取消、网络异常、已拥有商品等状态没有正确处理。

## 适用范围

- 适用于合法购买流程的客户端回调排查。
- 适用于测试购买、取消购买、恢复购买和日志分析。
- 不适用于伪造回调、伪造购买状态或绕过服务端校验。

## 判断方法

- BillingClient 是否在发起购买前连接成功。
- listener 是否在 BillingClient 构建时注册，生命周期是否被提前释放。
- 回调中的 responseCode 是否被完整记录和分支处理。
- 购买成功后是否执行 acknowledge 或 consume 的合法流程。
- App 是否在恢复场景中主动查询已有购买。

## 操作步骤

```bash
adb logcat -c
```

用途：清空旧日志，便于观察一次完整购买流程。

```bash
adb logcat -v time | grep -i "PurchasesUpdatedListener\\|BillingResult\\|Purchase"
```

用途：查看购买回调、响应码和购买对象日志。

```bash
adb shell dumpsys activity top | grep ACTIVITY
```

用途：确认购买返回后当前前台 Activity，排除页面生命周期异常。

```bash
adb shell dumpsys package <your.package.name> | grep version
```

用途：确认测试包版本，避免旧代码没有注册回调。

## 验证方法

- 用户取消时能记录取消状态，不当成购买失败或成功。
- 购买成功时能拿到 purchaseToken，并只提交给服务端校验。
- App 重启后能通过官方查询接口恢复未处理购买。
- 已拥有商品、网络异常、服务不可用等响应码有明确日志。

## 注意事项

- 不在客户端把购买回调当作最终可信授权结果。
- 不保存真实 purchaseToken，示例统一写 `<PURCHASE_TOKEN>`。
- 排查重点是生命周期、响应码、网络和测试账号配置。

## 相关命令

`adb logcat`、`adb shell dumpsys activity`、`adb shell dumpsys package`。

## 参考来源

- https://developer.android.com/google/play/billing/integrate
- https://developer.android.com/google/play/billing/lifecycle

