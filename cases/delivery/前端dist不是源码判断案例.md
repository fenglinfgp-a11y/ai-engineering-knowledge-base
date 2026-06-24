# 前端 dist 不是源码判断案例

来源类型：实战总结 / 我的项目经验

## 问题现象

交付包只有 `dist/`、`assets/`、`index.html`，没有可继续开发的前端源码。

## 环境背景

- 交付包：`<delivery-package>`
- 前端框架：`<frontend-framework>`
- 构建目录：`dist/`

## 初步判断

判断是否存在 `package.json`、`src/`、路由、组件、构建配置和 lock 文件。

## 排查命令

```bash
find . -maxdepth 3 -type f | sort
```

用途：查看交付包文件结构。

```bash
find . -maxdepth 3 -name "package.json" -o -name "vite.config.*" -o -name "nuxt.config.*"
```

用途：查找前端依赖和构建配置。

```bash
find . -maxdepth 3 -type d -name "src"
```

用途：查找前端源码目录。

## 关键日志

```text
只发现 dist/index.html，未发现 package.json 和 src/
```

含义：当前包更像构建产物，不是完整前端源码。

## 解决步骤

1. 列出当前交付包结构。
2. 查找依赖文件和源码目录。
3. 如果缺失，要求补充前端源码、构建命令和环境变量示例。
4. 补齐后运行安装和构建验证。

## 验证方式

- 存在 `package.json` 和源码目录。
- 能安装依赖并执行 `npm run build`。
- 构建产物可复现。

## 可复用经验

`dist/` 是构建产物，可以部署但不等于可开发源码。

## 禁止事项

- 不把压缩后的 JS 当源码交付。
- 不保存真实接口密钥。
- 不用反编译产物替代源码。

## 后续可补充内容

- Vue 项目完整交付目录样例。
- Nuxt 项目完整交付目录样例。

