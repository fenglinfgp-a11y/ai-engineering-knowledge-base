# 公开来源入口

来源类型：官方文档 / 开源项目 / 开源社区资料

## 问题场景

后续维护知识库时，需要知道哪些公开资料可以作为参考来源，避免引用不明来源、闭源代码或敏感资料。

## 适用范围

- 适用于新增技术文档、案例模板、命令索引、排查清单。
- 适用于学习合法开发、调试、部署、排错、项目交付和代码维护。
- 不适用于保存真实项目代码、密钥、客户资料、真实数据库数据。

## 判断方法

- 优先使用官方文档。
- 其次使用开源项目 README、Wiki、Issue 中的通用经验。
- 引用社区资料时，只总结方法，不复制大段原文。

## 操作步骤

| 来源 | 地址 | 用途 |
| --- | --- | --- |
| Android Developers | https://developer.android.com/ | 学习 Android 构建、调试、Logcat、ADB、应用兼容性。 |
| Google Play Billing 官方文档 | https://developer.android.com/google/play/billing | 理解合法 Billing 接入、测试购买、License tester 和服务端校验流程。 |
| Gradle 官方文档 | https://docs.gradle.org/ | 学习 Gradle Wrapper、任务、依赖树、构建缓存和构建失败排查。 |
| Spring Boot 官方文档 | https://docs.spring.io/spring-boot/ | 学习 Spring Boot 项目结构、配置、打包、部署和健康检查。 |
| Vue 官方文档 | https://vuejs.org/ | 学习 Vue 项目结构、组件、构建和前端维护。 |
| Nuxt 官方文档 | https://nuxt.com/docs | 学习 Nuxt 路由、SSR、静态生成、构建和部署。 |
| MySQL 官方文档 | https://dev.mysql.com/doc/ | 学习表结构、索引、备份恢复、EXPLAIN 和 SQL 排查。 |
| Ubuntu Server 官方文档 | https://documentation.ubuntu.com/server/ | 学习服务器初始化、服务管理、防火墙和基础运维。 |
| Nginx 官方文档 | https://nginx.org/en/docs/ | 学习 Nginx 配置、反向代理、静态资源、日志和 reload 流程。 |
| Docker 官方文档 | https://docs.docker.com/ | 学习镜像、容器、Compose、日志、卷和部署排查。 |
| Theos 官方文档 | https://theos.dev/docs/ | 学习 iOS tweak/deb 打包结构、rootless 构建和开源工具链使用。 |
| LSPosed 公开项目资料 | https://github.com/LSPosed/LSPosed | 学习 LSPosed 模块基础、环境兼容和公开项目说明。 |
| XposedBridge 公开项目资料 | https://github.com/rovo89/XposedBridge | 学习 Xposed API 历史资料和公开接口概念。 |

## 相关命令

```bash
grep -R "来源类型" . --include="*.md"
```

用途：检查 Markdown 文档是否标注来源类型。

```bash
grep -R "http" . --include="*.md"
```

用途：查找文档中已有外部链接，便于审核来源是否公开可信。

## 验证方法

- 新增文档能在本文件找到对应参考来源或明确标注为实战总结。
- 链接指向官方文档、开源项目或公开社区资料。
- 文档没有大段复制第三方内容。

## 注意事项

- 不要保存需要登录才能访问的客户资料或私有仓库内容。
- 不要记录绕过登录、支付、授权、风控、伪造订单或攻击第三方系统的内容。
- 官方文档可能更新，重要流程应定期回查原文。

## 后续可补充内容

- 按主题补充更细的官方页面链接，例如 Logcat、UFW、mysqldump、Spring Boot Actuator。
- 增加“资料可信度等级”和“最后核对日期”字段。

