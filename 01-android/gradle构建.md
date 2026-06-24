# Gradle 构建

来源类型：官方文档 / 实战总结  
参考来源：Gradle User Manual、Android Gradle Build Overview

## 问题场景

Android 项目构建失败、依赖冲突、Gradle Wrapper 版本不一致、打包产物找不到。

## 判断方法

- 是否使用项目自带 `./gradlew`。
- `settings.gradle` 是否包含目标模块。
- Android Gradle Plugin、Gradle、JDK 版本是否兼容。
- 失败日志中第一处 `FAILURE` 或 `Caused by` 是什么。

## 操作步骤

```bash
./gradlew tasks
```

用途：列出当前项目可执行任务，确认模块和任务名称。

```bash
./gradlew assembleDebug --stacktrace
```

用途：构建 Debug APK，并输出完整异常堆栈。

```bash
./gradlew :app:dependencies
```

用途：查看 `app` 模块依赖树，定位重复依赖或版本冲突。

```bash
./gradlew clean
```

用途：清理 Gradle 构建产物。风险：会删除本地构建缓存产物，下一次构建会变慢。

## 验证方法

- 构建命令退出码为 0。
- APK/AAB 出现在模块的 `build/outputs/` 目录。
- 运行安装后 App 能打开，核心页面无启动崩溃。

## 注意事项

- 优先使用 `./gradlew`，不要依赖本机全局 Gradle。
- 不要随意升级 AGP 或 Gradle，大版本升级可能牵动 JDK、Kotlin、插件兼容。

## 适用范围

Android 授权测试、构建、日志分析和合法调试场景。

## 相关命令

`adb devices`、`adb logcat`、`./gradlew tasks`。

## 后续可补充内容

补充脱敏日志片段、版本兼容矩阵和合法测试流程截图说明。

