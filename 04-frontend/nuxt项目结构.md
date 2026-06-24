# Nuxt 项目结构

来源类型：官方文档 / 实战总结  
参考来源：Nuxt 官方文档

## 问题场景

Nuxt 项目本地能跑，部署后路由、接口代理、SSR 或静态资源异常。

## 判断方法

- `nuxt.config.*` 是否配置运行时变量和模块。
- `pages/` 或 `app/` 是否决定路由。
- 是否使用 SSR、SPA、静态生成。
- 部署方式是否匹配 `.output/` 产物。

## 操作步骤

```bash
npm install
```

用途：安装项目依赖。

```bash
npm run dev
```

用途：启动 Nuxt 开发服务。

```bash
npm run build
```

用途：生成 Nuxt 生产产物，常见输出为 `.output/`。

```bash
node .output/server/index.mjs
```

用途：本地运行 Nuxt 生产服务，验证构建产物。

## 验证方法

- 开发模式和生产模式都能打开核心页面。
- 生产模式接口请求地址正确。
- 服务端日志没有运行时变量缺失错误。

## 注意事项

- Nuxt 运行时变量分 public/private，不能把服务端密钥暴露到 public。
- 静态生成和 SSR 部署方式不同，交付说明必须写清楚。

## 适用范围

Vue/Nuxt 前端项目结构、构建、部署和后台页面维护。

## 相关命令

`node -v`、`npm install`、`npm run dev`、`npm run build`。

## 后续可补充内容

补充构建白屏、环境变量、路由刷新 404 等案例。

