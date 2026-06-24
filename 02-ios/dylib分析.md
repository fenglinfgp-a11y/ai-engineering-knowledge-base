# dylib 分析

来源类型：官方文档 / 开源项目 / 实战总结

## 问题场景

dylib 加载失败、依赖库缺失、架构不匹配、符号找不到。

## 判断方法

- 当前 dylib 是否包含目标架构。
- 依赖路径是否存在。
- 加载进程日志是否有 `image not found`、`symbol not found`。

## 操作步骤

```bash
file plugin.dylib
```

用途：查看 dylib 文件类型和包含的 CPU 架构。

```bash
otool -L plugin.dylib
```

用途：查看 dylib 依赖的动态库路径。

```bash
nm -gU plugin.dylib | head
```

用途：查看导出符号的前几行，判断符号是否存在。风险：大型 dylib 输出很多，建议先用 `head` 限制。

## 验证方法

- `file` 显示包含目标设备需要的架构。
- `otool -L` 中的依赖路径在目标环境存在。
- 运行日志不再出现动态库加载错误。

## 注意事项

- 不要分析或保存商业 App 的闭源敏感实现。
- 只在授权测试和兼容性排查范围内使用。

## 适用范围

iOS deb、dylib、rootless/rootful 兼容性和合法安装排查。

## 相关命令

`dpkg-deb -I`、`dpkg-deb -c`、`file`、`otool -L`。

## 后续可补充内容

补充 deb 字段示例、rootless 路径对照和安装错误样例。

