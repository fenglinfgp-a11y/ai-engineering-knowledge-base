# Android ANR 日志分析案例

来源类型：官方文档 / 实战总结

## 问题现象

测试设备上 App 无响应，系统弹出“应用无响应”提示。

## 环境背景

- 设备：`<test-device>`
- Android：`<android-version>`
- 包名：`<your.package.name>`
- 场景：点击 `<feature-name>` 后卡住

## 初步判断

优先判断主线程是否被耗时任务阻塞、广播/服务超时、输入事件处理超时或死锁。

## 排查命令

```bash
adb devices
```

用途：确认测试设备连接。

```bash
adb logcat -v time | grep -i "ANR\|Input dispatching timed out\|ActivityManager"
```

用途：过滤 ANR 相关日志。

```bash
adb bugreport bugreport.zip
```

用途：导出系统诊断包。风险：可能包含设备和应用敏感信息，分享前必须脱敏。

## 关键日志

```text
Input dispatching timed out
```

含义：主线程未及时处理输入事件，常见于 UI 线程执行耗时操作。

## 解决步骤

1. 清空或标记复现前日志。
2. 复现 ANR。
3. 收集 Logcat 和 bugreport。
4. 查找主线程栈和发生时间。
5. 将耗时任务移出主线程或优化锁等待。

## 验证方式

- 同一路径不再弹 ANR。
- Logcat 不再出现同类超时。
- 页面操作响应时间恢复正常。

## 可复用经验

ANR 不是普通崩溃，要重点看主线程当时在做什么。

## 禁止事项

- 不保存真实用户日志。
- 不分析未授权 App 的敏感逻辑。
- 不记录绕过授权、支付或风控内容。

## 后续可补充内容

- traces.txt 分析案例。
- 主线程磁盘 IO 案例。

