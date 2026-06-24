# Vue 项目结构

来源类型：官方文档 / 实战总结  
参考来源：Vue 官方文档、Vite 官方文档

## 问题场景

接手 Vue 项目时，需要判断入口、路由、状态管理、接口封装和打包方式。

## 判断方法

- `package.json` 中的 scripts 决定启动和构建命令。
- `src/main.*` 通常是入口。
- `src/router` 管路由，`src/stores` 或 `store` 管状态。
- `vite.config.*` 或 `vue.config.js` 管构建配置。

## 操作步骤

```bash
npm install
```

用途：安装前端依赖。风险：会写入 `node_modules/`，可能受锁文件和 npm 源影响。

```bash
npm run dev
```

用途：启动本地开发服务。

```bash
npm run build
```

用途：生成生产构建产物，通常输出到 `dist/`。

```bash
npm run lint
```

用途：运行项目配置的代码检查，如果项目未配置该脚本会失败。

## 验证方法

- 本地开发服务能打开页面。
- 构建命令退出码为 0。
- `dist/` 中有 `index.html` 和静态资源。

## 注意事项

- 不要提交真实 `.env.production` 密钥。
- Node 版本不一致会导致依赖安装失败，交付时要记录推荐版本。

## 适用范围

Vue/Nuxt 前端项目结构、构建、部署和后台页面维护。

## 相关命令

`node -v`、`npm install`、`npm run dev`、`npm run build`。

## 后续可补充内容

补充构建白屏、环境变量、路由刷新 404 等案例。

