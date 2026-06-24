# deb 包结构

来源类型：官方文档 / 开源项目 / 实战总结  
参考来源：Debian Policy Manual、Theos Packaging 文档

## 问题场景

iOS 插件或工具以 `.deb` 形式分发时，安装失败、依赖缺失、文件路径错误。

## 判断方法

- `.deb` 是否能被 `dpkg-deb` 正常读取。
- `control` 文件字段是否完整。
- `data.tar` 内文件路径是否符合目标环境。

## 操作步骤

```bash
dpkg-deb -I package.deb
```

用途：查看 deb 控制信息，例如包名、版本、架构、依赖。

```bash
dpkg-deb -c package.deb
```

用途：列出 deb 内将安装到系统的文件路径。

```bash
mkdir -p /tmp/deb-check && dpkg-deb -x package.deb /tmp/deb-check
```

用途：把 deb 内容解包到临时目录用于检查。风险：如果目标目录已有同名文件可能被覆盖，示例使用临时新目录降低风险。

## 验证方法

- `dpkg-deb -I` 无解析错误。
- `Architecture` 与目标环境匹配。
- 依赖包名称和版本范围合理。
- 文件路径符合 rootless/rootful 目标。

## 注意事项

- 不要在知识库保存第三方完整 deb 内容。
- 只记录结构、字段、排查方法，不记录绕过授权或规避检测方案。

## 适用范围

iOS deb、dylib、rootless/rootful 兼容性和合法安装排查。

## 相关命令

`dpkg-deb -I`、`dpkg-deb -c`、`file`、`otool -L`。

## 后续可补充内容

补充 deb 字段示例、rootless 路径对照和安装错误样例。

