# 升级指南 - Node.js 22 & Next.js 15

本文档说明项目从 Node.js 18 + Next.js 13 升级到 Node.js 22 + Next.js 15 的变更内容。

## 主要变更

### 版本升级

#### 运行环境
- **Node.js**: 18.x → 22.x
- **npm**: 建议使用 10.x+

#### 核心框架
- **Next.js**: 13.1.6 → 15.0.3
- **React**: 18.2.0 → 18.3.1
- **TypeScript**: 5.1.3 → 5.7.2

#### 国际化
- **next-i18next**: 13.0.3 → 15.3.1
- **i18next**: 22.4.9 → 23.16.8
- **react-i18next**: 12.1.4 → 15.1.3

#### 开发工具
- **eslint**: 8.43.0 → 8.57.1
- **eslint-config-next**: 13.4.7 → 15.0.3
- **prettier**: 2.8.3 → 3.4.1
- **@types/node**: 18.16.18 → 22.10.1

### 配置文件变更

#### 1. tsconfig.json
- `target`: "es5" → "ES2020" (更好地利用现代 JavaScript 特性)
- `moduleResolution`: "node" → "bundler" (Next.js 15 推荐)
- 新增 Next.js TypeScript 插件配置
- 新增 `.next/types/**/*.ts` 到 include 列表

#### 2. package.json
- 新增 `engines` 字段，明确要求 Node.js >= 22.0.0

#### 3. Tailwind CSS
- 移除 `@tailwindcss/line-clamp` 插件（已内置于 Tailwind CSS 3.3+）
- 如果代码中使用了 `line-clamp-{n}` 类，现在可以直接使用（无需更改）

#### 4. .nvmrc
- 新增此文件，指定项目使用 Node.js 22

## 升级步骤

### 1. 升级 Node.js

如果使用 nvm（推荐）：

```bash
nvm install 22
nvm use 22
```

或从 [Node.js 官网](https://nodejs.org/) 下载安装 v22.x LTS 版本。

### 2. 清理旧依赖

```bash
rm -rf node_modules package-lock.json
```

### 3. 安装新依赖

```bash
npm install
```

### 4. 验证升级

```bash
# 检查 Node.js 版本
node --version  # 应显示 v22.x.x

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build
```

## 潜在的 Breaking Changes

### Next.js 15 重要变更

1. **React 编译器支持**
   - Next.js 15 支持 React Compiler，可能需要调整某些组件写法

2. **Async Request APIs**
   - `headers()`, `cookies()` 等 API 现在是异步的
   - 如果项目中使用了这些 API，需要添加 `await`

3. **Turbopack 改进**
   - 开发模式下使用 Turbopack 会更快
   - 某些 webpack 特定的配置可能不再适用

4. **Fetch 请求缓存**
   - 默认的 `fetch()` 缓存行为有所改变
   - 建议显式指定缓存策略

### 迁移检查清单

- [ ] 检查所有使用 `headers()`, `cookies()` 等 API 的地方，确保使用 `await`
- [ ] 测试所有路由和 API endpoints
- [ ] 测试国际化功能（所有语言）
- [ ] 检查所有使用 `fetch()` 的地方，确认缓存行为符合预期
- [ ] 运行完整的测试套件（如果有）
- [ ] 在本地构建并测试生产版本

## 性能提升

升级后预期的性能改进：

1. **更快的开发体验**: Turbopack 带来更快的 HMR
2. **更好的构建性能**: Next.js 15 的构建优化
3. **更小的 bundle 大小**: React 18.3 的优化
4. **更好的 TypeScript 支持**: TypeScript 5.7 的性能改进

## 回滚方案

如果升级后遇到问题，可以通过 Git 回滚：

```bash
git checkout HEAD~1 package.json package-lock.json tsconfig.json tailwind.config.js
rm .nvmrc
rm -rf node_modules
nvm use 18  # 或切换回 Node.js 18
npm install
```

## 参考资料

- [Next.js 15 发布说明](https://nextjs.org/blog/next-15)
- [Next.js 15 升级指南](https://nextjs.org/docs/app/building-your-application/upgrading/version-15)
- [React 18.3 更新日志](https://react.dev/blog/2024/04/25/react-19)
- [Node.js 22 发布说明](https://nodejs.org/en/blog/release/v22.0.0)

## 支持

如果升级过程中遇到问题：

1. 检查 [Next.js 官方文档](https://nextjs.org/docs)
2. 查看 [项目 GitHub Discussions](https://github.com/spencerwooo/onedrive-vercel-index/discussions)
3. 检查控制台错误信息并搜索相关问题

---

**最后更新**: 2025-11-06

