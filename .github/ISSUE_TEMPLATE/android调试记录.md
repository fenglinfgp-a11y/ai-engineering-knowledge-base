# Android 调试记录

来源类型：实战总结

## 问题背景

- App 包名：`<your.package.name>`
- 测试设备：`<test-device>`
- 调试类型：`<crash/anr/gradle/install/lsposed/billing>`

## 环境信息

- Android 版本：`<android-version>`
- 构建工具：`<gradle/agp/jdk-version>`
- 测试包版本：`<app-version>`

## 现象描述

描述闪退、ANR、安装失败、构建失败、模块未加载等现象。

## 已执行操作

```bash
adb devices
```

用途：确认测试设备连接。

```bash
adb logcat -v time
```

用途：采集 Android 日志，外发前必须脱敏。

## 关键日志

```text
<脱敏后的 FATAL EXCEPTION / ANR / INSTALL_FAILED 日志>
```

## 判断结论

- 是否可复现：
- 初步根因：
- 是否与设备/版本相关：

## 解决方案

只记录合法开发、授权测试和兼容性调试方案。

## 验证结果

- [ ] 同设备复测通过
- [ ] Logcat 无同类错误
- [ ] 构建或安装成功

## 是否已脱敏

- [ ] 是，已脱敏
- [ ] 否，暂不写入知识库

## 后续沉淀位置

建议路径：`cases/android/<case-name>.md`
