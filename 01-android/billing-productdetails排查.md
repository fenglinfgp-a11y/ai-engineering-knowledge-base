# Billing ProductDetails 排查

来源类型：官方文档 / 实战总结  
参考来源：Google Play Billing 官方文档

## 问题场景

调用商品查询后，`ProductDetails` 为空、商品 ID 查不到、订阅 offer 信息缺失，或者测试环境和正式配置不一致。

## 适用范围

- 适用于 Google Play Billing 商品查询和合法测试。
- 适用于一次性商品、订阅商品、测试轨道配置核对。
- 不适用于伪造商品、篡改价格、绕过购买流程。

## 判断方法

- `productId` 是否与 Play Console 完全一致，大小写也要一致。
- 商品类型是否传对，例如一次性商品和订阅不能混用。
- App 包名、签名、版本是否来自同一个 Play Console 应用。
- 测试账号是否可访问对应测试轨道。
- 订阅商品是否配置了 base plan 和 offer。

## 操作步骤

```bash
adb logcat -c
```

用途：清空旧日志，方便只保留本次商品查询过程。

```bash
adb logcat -v time | grep -i "ProductDetails\\|queryProductDetails\\|BillingClient"
```

用途：查看商品查询请求、响应和 BillingClient 状态。

```bash
adb shell dumpsys package <your.package.name> | grep version
```

用途：确认设备安装版本是否为 Play Console 中参与测试的版本。

```bash
./gradlew :app:dependencies --configuration debugRuntimeClasspath
```

用途：确认 Billing Library 版本，排除依赖被旧版本覆盖。

## 验证方法

- 日志中商品查询返回成功响应码。
- 返回对象里能看到预期 productId。
- 订阅商品能看到 base plan 或 offerToken。
- 同一个测试账号在 Play 商店环境下能看到对应测试版本。

## 注意事项

- 商品配置、测试轨道、测试账号状态可能需要等待同步。
- 不把真实商品后台截图、客户账号、订单信息写入文档。
- 排查时只记录 productId 的脱敏示例，例如 `<product_id>`。

## 相关命令

`adb logcat`、`adb shell dumpsys package`、`./gradlew :app:dependencies`。

## 参考来源

- https://developer.android.com/google/play/billing/integrate
- https://developer.android.com/google/play/billing/test

