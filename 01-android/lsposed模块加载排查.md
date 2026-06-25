# LSPosed 模块加载排查

来源类型：开源项目 / 开源社区资料 / 实战总结  
参考来源：LSPosed 公开项目资料、XposedBridge 公开项目资料

## 问题场景

授权测试环境中，LSPosed 模块安装后没有生效，目标应用没有被选中，模块日志为空，或 Android 版本、作用域、进程名导致加载异常。

## 适用范围

- 仅适用于自有设备、授权测试设备、兼容性调试和公开 API 学习。
- 适用于模块是否安装、是否启用、作用域是否命中、日志是否输出的排查。
- 不适用于绕过登录、支付、授权、风控或攻击第三方应用。

## 判断方法

- 模块是否已安装且在 LSPosed 中启用。
- 目标应用是否在模块作用域内。
- 目标应用包名、进程名、版本是否与测试记录一致。
- 模块入口类、清单配置和构建产物是否匹配。
- 日志中是否有模块加载、Hook 初始化或异常堆栈。

## 操作步骤

```bash
adb shell pm list packages | grep <package_keyword>
```

用途：确认目标应用和测试模块都已安装。

```bash
adb shell dumpsys package <module.package.name> | grep version
```

用途：确认当前安装的模块版本，避免测试旧包。

```bash
adb logcat -c
```

用途：清空旧日志，便于只观察本次启动和加载过程。

```bash
adb logcat -v time | grep -i "LSPosed\\|Xposed\\|<module_tag>"
```

用途：查看 LSPosed、XposedBridge 和模块自定义 tag 的加载日志。

```bash
adb shell am force-stop <target.package.name>
```

用途：停止目标应用后重新启动，触发模块加载。风险：会中断目标应用当前会话，仅在测试环境使用。

## 验证方法

- LSPosed 管理器中模块为启用状态。
- 目标应用在模块作用域内。
- 重新启动目标应用后能看到模块初始化日志。
- 若仍未加载，日志中能定位到类名错误、作用域错误、版本不兼容或进程不匹配。

## 注意事项

- 只记录包名占位符和通用现象，不记录客户应用真实数据。
- 不写任何绕过授权、支付、风控的 Hook 方法。
- Android 版本、Zygisk/Root 环境和目标进程差异都可能影响加载。

## 相关命令

`adb shell pm list packages`、`adb shell dumpsys package`、`adb logcat`、`adb shell am force-stop`。

## 参考来源

- https://github.com/LSPosed/LSPosed
- https://github.com/rovo89/XposedBridge

