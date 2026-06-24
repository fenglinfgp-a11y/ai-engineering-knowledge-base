# Android 日志分析案例

来源类型：官方文档 / 实战总结

## 问题场景

Android 测试包在授权设备上出现闪退、卡死或异常行为，需要通过 Logcat 定位原因。

## 问题现象

测试设备上 App 点击某页面后闪退，用户界面只看到应用退出。

## 环境背景

- 设备：`<test-device-model>`
- Android：`<android-version>`
- 包名：`<your.package.name>`
- 版本：`<app-version>`

## 初步判断

优先采集复现时段 Logcat，查找 `FATAL EXCEPTION`、权限、网络、资源缺失或空指针异常。

## 排查命令

```bash
adb devices
```

用途：确认测试设备已连接并授权。

```bash
adb logcat -c
```

用途：清空当前日志缓冲，便于只采集本次复现。风险：会清除当前 Logcat 缓冲。

```bash
adb logcat -v time > android-crash.log
```

用途：按时间格式保存复现日志。风险：日志可能包含敏感信息，保存和分享前必须脱敏。

```bash
adb logcat -d | grep -i "FATAL EXCEPTION\|AndroidRuntime\|Exception"
```

用途：导出当前日志并过滤崩溃关键字。

## 关键日志

```text
FATAL EXCEPTION: main
java.lang.NullPointerException
```

含义：主线程发生未捕获异常，需要根据堆栈第一处业务代码定位。

## 解决步骤

1. 清空日志后复现一次。
2. 保存完整日志。
3. 查找 `FATAL EXCEPTION` 上下文。
4. 定位第一处业务代码栈。
5. 修复后重新安装测试包并复现。

## 验证方式

- 同样操作路径不再闪退。
- Logcat 不再出现同类异常。
- 相关页面和接口返回正常。

## 可复用经验

Android 崩溃日志要看完整堆栈，根因通常在第一处业务代码，而不是最后一行系统调用。

## 禁止事项

- 不保存真实用户日志。
- 不记录绕过付费、登录、授权或风控的调试内容。
- 不分析未授权 App 的敏感实现。

## 适用范围

适用于授权测试设备上的 APK 崩溃和异常日志分析。

## 判断方法

如果日志时间与复现时间不一致，应先清空日志再重新复现。

## 操作步骤

见“排查命令”和“解决步骤”。

## 验证方法

见“验证方式”。

## 注意事项

日志外发前要脱敏手机号、邮箱、Token、订单号和用户 ID。

## 相关命令

核心命令：`adb devices`、`adb logcat -c`、`adb logcat -v time`、`grep`。

## 后续可补充内容

- ANR 日志分析案例。
- 网络证书错误案例。
- 多进程日志过滤案例。
