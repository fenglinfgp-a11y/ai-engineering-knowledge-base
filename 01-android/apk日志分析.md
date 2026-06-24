# APK 日志分析

来源类型：官方文档 / 实战总结  
参考来源：Android Logcat 官方文档

## 问题场景

APK 闪退、接口请求失败、页面无响应、第三方 SDK 初始化失败。

## 判断方法

- 是否能稳定复现。
- 是否有 `FATAL EXCEPTION`、`ANR`、`NetworkSecurityConfig`、`SSLHandshakeException` 等关键字。
- 日志时间是否与操作时间一致。

## 操作步骤

```bash
adb logcat -c
```

用途：清空当前 Logcat 缓冲区，方便只采集本次复现日志。风险：会清掉设备当前日志缓冲，执行前确认不需要保留旧日志。

```bash
adb logcat -v time > apk-debug.log
```

用途：按时间格式持续保存日志到 `apk-debug.log`，用于复现后分析。

```bash
adb logcat -d | grep -i "FATAL EXCEPTION\\|ANR\\|Exception\\|Error"
```

用途：导出当前日志并过滤常见错误关键字。

## 验证方法

- 修复后重复同一操作，不再出现同类异常。
- 关键接口返回正常状态码。
- 页面、SDK 或服务按预期初始化。

## 注意事项

- 发给他人分析前要脱敏手机号、邮箱、Token、订单号、客户 ID。
- 不要只看最后一行错误，通常根因在异常堆栈第一段或更早的初始化日志。

## 适用范围

Android 授权测试、构建、日志分析和合法调试场景。

## 相关命令

`adb devices`、`adb logcat`、`./gradlew tasks`。

## 后续可补充内容

补充脱敏日志片段、版本兼容矩阵和合法测试流程截图说明。

