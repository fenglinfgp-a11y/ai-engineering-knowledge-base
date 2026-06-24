# APK 安装失败排查案例

来源类型：官方文档 / 实战总结

## 问题现象

执行 `adb install` 安装 APK 失败，提示 `INSTALL_FAILED_*`。

## 环境背景

- 设备：`<test-device>`
- Android：`<android-version>`
- APK：`<app-debug.apk>`
- 包名：`<your.package.name>`

## 初步判断

优先判断签名不一致、版本降级、ABI 不兼容、存储不足、SDK 版本不匹配。

## 排查命令

```bash
adb devices
```

用途：确认设备连接和授权状态。

```bash
adb install -r <app-debug.apk>
```

用途：覆盖安装 APK，保留应用数据。

```bash
adb install -r -d <app-debug.apk>
```

用途：允许降级安装测试包。风险：降级可能导致本地数据结构不兼容。

```bash
adb shell pm clear <your.package.name>
```

用途：清空 App 本地数据。风险：会删除本地登录态、数据库和缓存。

## 关键日志

```text
INSTALL_FAILED_UPDATE_INCOMPATIBLE
```

含义：设备上已有同包名但签名不同的应用，无法覆盖安装。

## 解决步骤

1. 记录安装失败完整错误。
2. 判断是否为签名、降级或 ABI 问题。
3. 测试机可卸载旧包后重新安装。
4. 保留需要的数据时，不要直接清空或卸载。

## 验证方式

- `adb install` 返回 `Success`。
- App 能在测试设备启动。
- Logcat 无启动崩溃。

## 可复用经验

`INSTALL_FAILED_*` 错误码通常已经指出方向，先按错误码分类排查。

## 禁止事项

- 不绕过系统安装校验。
- 不保存真实用户数据。
- 不在生产设备随意清空应用数据。

## 后续可补充内容

- ABI 不兼容案例。
- minSdk 不兼容案例。

