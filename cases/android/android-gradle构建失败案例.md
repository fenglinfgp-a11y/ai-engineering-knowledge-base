# Android Gradle 构建失败案例

来源类型：官方文档 / 实战总结

## 问题现象

执行 `./gradlew assembleDebug` 失败，无法生成 APK。

## 环境背景

- Gradle Wrapper：`<gradle-version>`
- Android Gradle Plugin：`<agp-version>`
- JDK：`<jdk-version>`
- 模块：`:app`

## 初步判断

优先看第一处 `FAILURE` 和 `Caused by`，再判断是 JDK、AGP、依赖、仓库源、签名或资源问题。

## 排查命令

```bash
./gradlew tasks
```

用途：查看项目可用 Gradle 任务。

```bash
./gradlew :app:assembleDebug --stacktrace
```

用途：构建 Debug 包并输出完整异常堆栈。

```bash
./gradlew :app:dependencies
```

用途：查看 app 模块依赖树。

```bash
./gradlew clean
```

用途：清理构建产物。风险：会删除本地构建输出，下一次构建会变慢。

## 关键日志

```text
Unsupported class file major version <version>
```

含义：JDK 或 Gradle/AGP 版本不兼容。

## 解决步骤

1. 确认项目要求的 JDK、Gradle、AGP 版本。
2. 使用项目自带 `./gradlew`。
3. 根据 `Caused by` 修复版本或依赖问题。
4. 重新构建 Debug 包。

## 验证方式

- 构建命令退出码为 0。
- APK 出现在 `app/build/outputs/apk/debug/`。
- 安装到测试设备可启动。

## 可复用经验

Gradle 失败不要只看最后一行，要找第一处 `Caused by`。

## 禁止事项

- 不随意升级 Gradle/AGP 大版本。
- 不提交签名私钥。
- 不保存第三方闭源项目源码。

## 后续可补充内容

- 依赖冲突案例。
- Maven 仓库访问失败案例。

