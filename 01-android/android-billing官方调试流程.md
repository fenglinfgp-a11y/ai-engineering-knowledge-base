# Android Billing 官方调试流程

来源类型：官方文档 / 实战总结  
参考来源：Google Play Billing 官方文档、Android Developers、Play Console 测试说明

## 问题场景

接入 Google Play Billing 后，测试购买无法拉起、商品查不到、回调状态不符合预期，或者测试账号与 Play Console 配置不一致。

## 适用范围

- 适用于合法接入 Google Play Billing、测试轨道、License tester 和内部测试。
- 适用于客户端日志分析、测试包版本核对、商品配置核对。
- 不适用于伪造订单、绕过支付、绕过授权验证或规避风控。

## 判断方法

- 确认应用包名、签名、versionCode 与 Play Console 中的测试版本一致。
- 确认测试账号已加入 License testers 或对应测试轨道。
- 确认商品 ID、商品类型和状态在 Play Console 中已配置并可测试。
- 确认 Billing Library 版本与项目 Gradle 配置一致。
- 日志中优先看 `BillingClient` 连接结果、商品查询结果和购买回调错误码。

## 操作步骤

```bash
adb devices
```

用途：确认测试设备已连接，避免排查时实际没有设备或设备未授权。

```bash
adb shell dumpsys package <your.package.name> | grep -E "versionCode|versionName"
```

用途：确认设备上安装的是预期测试版本。

```bash
adb logcat -c
```

用途：清空旧日志，便于只观察本次复现。风险：会清掉设备当前 logcat 缓冲区，不影响 App 数据。

```bash
adb logcat -v time | grep -i "BillingClient\\|billing\\|Purchase"
```

用途：观察 BillingClient 连接、商品查询、购买流程和购买回调日志。

```bash
./gradlew :app:dependencies --configuration debugRuntimeClasspath
```

用途：查看 Debug 运行时依赖中实际解析到的 Billing Library 版本。

## 验证方法

- BillingClient 能成功连接 Google Play 服务。
- 商品查询能返回预期 productId 和 offer 信息。
- 测试购买只能通过 Google Play 官方测试流程完成。
- 客户端收到购买回调后，服务端按官方接口校验购买令牌。

## 注意事项

- 不把测试账号、订单号、购买令牌写入知识库，记录时统一脱敏。
- 客户端日志只能帮助定位流程，订单可信状态以后端校验为准。
- Play Console 配置变更可能有延迟，排查时记录变更时间。

## 相关命令

`adb devices`、`adb logcat`、`adb shell dumpsys package`、`./gradlew :app:dependencies`。

## 参考来源

- https://developer.android.com/google/play/billing
- https://developer.android.com/google/play/billing/test
- https://developer.android.com/studio/command-line/logcat

