# jar 不是后端源码判断案例

来源类型：实战总结 / 我的项目经验

## 问题现象

交付包只有 `app.jar` 和配置文件，没有 Java 后端源码。

## 环境背景

- 交付包：`<delivery-package>`
- 后端框架：Spring Boot
- 可执行文件：`app.jar`

## 初步判断

判断是否存在 `pom.xml` 或 `build.gradle`、`src/main/java`、配置示例、启动说明和数据库结构。

## 排查命令

```bash
find . -maxdepth 3 -name "pom.xml" -o -name "build.gradle" -o -name "settings.gradle"
```

用途：查找 Java 项目构建文件。

```bash
find . -maxdepth 4 -type d -path "*src/main/java*"
```

用途：查找 Java 源码目录。

```bash
jar tf app.jar | head
```

用途：查看 jar 内容概览。风险：只用于判断包结构，不代表可继续开发源码。

## 关键日志

```text
仅发现 app.jar，未发现 src/main/java 和构建文件
```

含义：当前交付更像运行包，不是完整后端源码。

## 解决步骤

1. 检查是否有源码目录和构建文件。
2. 检查是否有 README 和环境变量示例。
3. 要求补充后端源码、数据库结构和构建说明。
4. 补齐后运行构建或启动验证。

## 验证方式

- 存在源码目录和构建文件。
- 能执行 Maven 或 Gradle 构建。
- 能在测试环境启动服务。

## 可复用经验

jar 可用于部署，但不是可维护源码交付的替代品。

## 禁止事项

- 不反编译第三方或闭源 jar 作为源码。
- 不保存真实配置文件。
- 不把运行包当完整交付结论。

## 后续可补充内容

- Maven 项目交付目录样例。
- Gradle 项目交付目录样例。

