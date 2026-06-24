# LSPosed 模块未加载排查案例

来源类型：开源项目 / 实战总结

## 问题现象

授权测试设备上模块已安装，但目标 App 启动后没有任何模块日志或效果。

## 环境背景

- 测试设备：`<test-device>`
- Android：`<android-version>`
- LSPosed：`<lsposed-version>`
- 目标包名：`<target.package.name>`

## 初步判断

优先确认模块是否启用、目标 App 是否勾选、包名是否正确、重启是否完成、日志是否输出。

## 排查命令

```bash
adb shell pm list packages | grep "<target-package-keyword>"
```

用途：确认目标 App 包名。

```bash
adb logcat | grep -i "LSPosed\|Xposed\|<module-log-keyword>"
```

用途：查看 LSPosed 和模块日志。

```bash
adb shell am force-stop <target.package.name>
```

用途：强制停止目标 App，便于重新触发启动。风险：会中断当前 App 运行状态。

## 关键日志

```text
module not enabled for package <target.package.name>
```

含义：模块没有作用到目标包名，通常需要在管理器中勾选作用域。

## 解决步骤

1. 确认目标包名。
2. 在 LSPosed 管理器启用模块并勾选目标 App。
3. 重启目标 App，必要时重启设备。
4. 采集 Logcat 验证模块初始化日志。

## 验证方式

- Logcat 出现模块初始化日志。
- LSPosed 管理器显示模块启用。
- 仅在授权测试范围内验证功能。

## 可复用经验

模块未加载常见原因是作用域没有勾选或包名选错。

## 禁止事项

- 不记录绕过登录、支付、授权、风控的内容。
- 不在未授权 App 上测试。
- 不保存目标 App 敏感实现。

## 后续可补充内容

- Android 版本兼容表。
- Zygisk/LSPosed 版本差异案例。

