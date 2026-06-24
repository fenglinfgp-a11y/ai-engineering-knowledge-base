# rootless / roothide 基础

来源类型：开源项目 / 开源社区资料 / 实战总结  
参考来源：Theos Rootless 文档、roothide Developer 文档

## 问题场景

同一个 iOS 插件在 rootful、rootless、roothide 环境下路径不同，导致安装成功但运行失败。

## 判断方法

- 包的 `Architecture` 是否适配 rootless。
- 安装路径是否使用目标环境的前缀。
- 运行日志是否提示找不到 dylib、plist、资源文件。

## 操作步骤

```bash
dpkg-deb -I package.deb | grep -E "Package|Version|Architecture|Depends"
```

用途：快速查看包名、版本、架构和依赖字段。

```bash
dpkg-deb -c package.deb | head -50
```

用途：查看包内前 50 个路径，判断是否使用了正确目录布局。

```bash
grep -R "/Library/MobileSubstrate\\|/var/jb\\|/var/containers" .
```

用途：在源码或配置中查找硬编码路径，排查 rootless 兼容问题。

## 验证方法

- 安装器不再提示架构不支持。
- 插件进程能加载 dylib。
- 日志不再出现路径不存在或依赖缺失。

## 注意事项

- roothide 相关内容只记录兼容性、路径、打包排查，不记录规避检测或绕过限制方案。
- 路径问题优先使用官方宏或构建系统能力，不要到处硬编码绝对路径。

## 适用范围

iOS deb、dylib、rootless/rootful 兼容性和合法安装排查。

## 相关命令

`dpkg-deb -I`、`dpkg-deb -c`、`file`、`otool -L`。

## 后续可补充内容

补充 deb 字段示例、rootless 路径对照和安装错误样例。

