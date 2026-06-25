# Gradle 依赖冲突排查

来源类型：官方文档 / 实战总结  
参考来源：Gradle 官方文档、Android Developers 构建文档

## 问题场景

Android 构建失败、运行时报 `Duplicate class`、依赖版本被覆盖、某个库在 Debug 和 Release 中解析结果不同。

## 适用范围

- 适用于 Android Gradle 项目的依赖树、版本冲突和构建失败排查。
- 适用于公开依赖和开源库版本分析。
- 不适用于复制第三方项目源码或保存私有依赖仓库凭据。

## 判断方法

- 先确认失败发生在编译期、打包期还是运行期。
- 查看报错中的模块名、类名、坐标和 configuration。
- 用 `dependencies` 看实际依赖树。
- 用 `dependencyInsight` 查某个依赖为什么被选中。
- 检查多模块项目是否同时引入不同版本。

## 操作步骤

```bash
./gradlew :app:dependencies --configuration debugRuntimeClasspath
```

用途：查看 Debug 运行时依赖树，确认实际解析到的版本。

```bash
./gradlew :app:dependencyInsight --dependency <group-or-artifact> --configuration debugRuntimeClasspath
```

用途：追踪指定依赖由谁引入、为什么选择当前版本。

```bash
./gradlew :app:assembleDebug --stacktrace
```

用途：重新构建 Debug 包并输出完整异常堆栈。

```bash
./gradlew --stop
```

用途：停止 Gradle Daemon。风险：会中断当前 Gradle 后台进程，正在构建时不要执行。

## 验证方法

- `dependencyInsight` 能解释冲突来源。
- 依赖版本统一后，`assembleDebug` 能通过。
- 运行时不再出现重复类或缺失类异常。
- 修改只影响必要模块，没有引入无关升级。

## 注意事项

- 优先用版本约束、平台 BOM 或统一版本管理解决冲突。
- 不要盲目 `exclude`，排除前确认是否还有代码需要该传递依赖。
- 私有 Maven 仓库地址、用户名、Token 不写入文档。

## 相关命令

`./gradlew dependencies`、`./gradlew dependencyInsight`、`./gradlew assembleDebug --stacktrace`。

## 参考来源

- https://docs.gradle.org/current/userguide/viewing_debugging_dependencies.html
- https://docs.gradle.org/current/userguide/dependency_constraints.html
- https://developer.android.com/build

