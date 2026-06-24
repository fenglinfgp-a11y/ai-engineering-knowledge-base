# Xposed / LSPosed 基础

来源类型：官方文档 / 开源项目 / 实战总结  
参考来源：LSPosed GitHub、LSPosed 官方站、Xposed API 社区资料

## 问题场景

需要在授权测试设备上分析 Android App 行为、验证兼容性或调试自研模块。

## 判断方法

- 设备是否为测试机，且 App 是否有授权测试范围。
- LSPosed 是否正常启用，模块是否被勾选到目标 App。
- 模块日志是否能在 Logcat 或 LSPosed 日志中看到。

## 操作步骤

```bash
adb devices
```

用途：确认测试设备已连接并授权 ADB 调试。

```bash
adb logcat | grep -i "lsposed\\|xposed\\|模块关键字"
```

用途：过滤 LSPosed、Xposed 或模块自己的日志，判断模块是否加载。

```bash
adb shell pm list packages | grep "<package_keyword>"
```

用途：确认目标 App 包名是否存在，避免勾选错目标。

## 验证方法

- LSPosed 管理器中模块显示已启用。
- 重启目标 App 后，模块日志出现初始化记录。
- 目标功能只在授权测试范围内生效。

## 注意事项

- 禁止记录绕过登录、支付、授权、风控、检测的操作。
- 不在生产机和非授权 App 上测试。
- Android 版本、ROM、Zygisk/LSPosed 版本都会影响兼容性，排查时要记录环境。

## 适用范围

Android 授权测试、构建、日志分析和合法调试场景。

## 相关命令

`adb devices`、`adb logcat`、`./gradlew tasks`。

## 后续可补充内容

补充脱敏日志片段、版本兼容矩阵和合法测试流程截图说明。

