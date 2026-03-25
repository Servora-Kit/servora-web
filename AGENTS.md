# AGENTS.md - servora-web

<!-- Generated: 2026-03-25 -->

## 项目概览

`servora-web` 是 [Servora-Kit](https://github.com/Servora-Kit) 组织的**前端共享库仓库**，提供跨业务仓库复用的前端基础设施。使用 pnpm workspace monorepo 管理多个 npm 包。

当前包含：

| 包 | 路径 | 说明 |
|---|------|------|
| `@servora/web-pkg` | `packages/pkg/` | 通用 HTTP 请求处理器、Token 管理、Kratos 错误解析 |

## 开发约束

### 提交规范

格式：`type(scope): description`。type：`feat`/`fix`/`refactor`/`docs`/`test`/`chore`。scope 建议：`pkg`、`repo`。

### 包设计原则

- 只放**跨业务仓库复用**的前端基础能力
- 保持 API 小而稳定：导出的类型/函数是多个应用的共享契约
- 不要放页面状态、业务 store、路由逻辑、特定服务的 toast/文案

### 与业务仓库的关系

- 业务仓库通过 `@servora/web-pkg` 依赖本仓库
- 本地开发：顶层 `pnpm-workspace.yaml`（在 `servora-kit/`）联调，`workspace:*` 自动 link 本地源码
- CI/生产：通过 npm 公共 registry 安装（`npm publish --provenance`）

### 发布流程

```bash
cd packages/pkg
# 1. 修改代码 → 提交
# 2. 更新 package.json 中的 version
# 3. 打 tag（格式 pkg/v<version>）
git tag pkg/v0.0.2
git push origin main --tags
# GitHub Actions 自动构建并 npm publish --provenance
```

业务仓库更新：将 `"@servora/web-pkg": "workspace:*"` 改为 `"@servora/web-pkg": "^0.0.2"`（CI 独立构建时需要），本地开发仍通过 workspace link 使用本地源码。

## 目录结构

```
servora-web/
├── .github/
│   └── workflows/
│       └── publish-pkg.yml # tag pkg/v* 触发自动发布到 npm
├── packages/
│   └── pkg/                # @servora/web-pkg
│       ├── src/
│       │   ├── request.ts  # createRequestHandler, ApiError, TokenStore
│       │   └── errors.ts   # parseKratosError, isKratosReason, kratosMessage
│       ├── dist/            # 构建产物（gitignored，npm publish 时生成）
│       ├── package.json
│       ├── tsconfig.json       # 类型检查（noEmit）
│       └── tsconfig.build.json # 构建配置（ESM + d.ts + source map）
├── pnpm-workspace.yaml
└── package.json
```

## packages/pkg 修改约定

- 优先把这里当作 **proto client 适配层**，而不是业务逻辑目录
- 新增能力前先判断它是否能被多个前端应用复用
- 错误处理需兼容 Kratos 返回格式：`{ code, reason, message, metadata? }`
- 修改 Token 刷新、请求头注入、`onError` 触发时机时，必须检查是否影响现有调用方

## 禁止事项

- 不要放某个服务专属的 toast / 文案 / 页面跳转策略
- 不要在这里复制业务仓库 `api/gen/ts/` 里的生成类型
- 不要把页面组件、UI 库放到这里（UI 共享另行处理）
