# HotKey Web（app 端）

[![CI](https://github.com/StephenQiu30/hotkey-web/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/StephenQiu30/hotkey-web/actions/workflows/ci.yml)

HotKey 平台的 **app 端（Web 客户端工作台）**，为内容创作者提供内容归档阅读、工作台可视化、实时热点监控等界面。本仓库只包含前端应用；后端服务位于独立的 [`hotkey-server`](https://github.com/StephenQiu30/hotkey-server) 仓库，app 端通过消费其发布的 OpenAPI 契约与后端交互。

## 技术栈

Next.js App Router · React · TypeScript · Tailwind CSS · Radix UI · zustand · axios · recharts · GSAP

## 目录结构

```
hotkey-web/
├── src/
│   ├── app/            # Next.js App Router（页面文件）
│   ├── components/     # 业务组件与 UI 组合组件
│   ├── layouts/        # 工作台布局
│   ├── stores/         # zustand 状态管理
│   ├── lib/            # 请求、认证会话与通用工具
│   └── services/       # OpenAPI 自动生成客户端
├── public/             # 品牌与静态资源
├── test/               # 单元测试与统一测试初始化
└── ...
```

## 本机开发

前置条件：Node.js（建议使用仓库锁定的版本）。本地需要一个可访问的 `hotkey-server` 后端实例（见其仓库 README 启动）。

```bash
git clone https://github.com/StephenQiu30/hotkey-web.git
cd hotkey-web
npm ci
cp .env.example .env
npm run dev
```

默认启动在 `http://127.0.0.1:3000`，通过 Next.js 服务端 rewrites 访问后端。环境变量见 [.env.example](.env.example)。

## OpenAPI 协作

- 发布契约来自 `hotkey-server` 的 `docs/openapi/swagger.json`。
- 后端契约变更后：在后端生成 OpenAPI，再运行 `npm run openapi:generate` 生成客户端。
- 提交前运行 `npm run openapi:check`，确认发布契约与客户端无漂移。
- 业务代码只调用 `src/services/hotkey/hotkey-server/` 中的生成函数，不手写后端 DTO 或接口路径。

## 质量检查

```bash
npm run openapi:check
npm run typecheck
npm run test:unit
npm audit --omit=dev --audit-level=high
npm run build
```

贡献和安全报告请参阅 [CONTRIBUTING.md](CONTRIBUTING.md) 与 [SECURITY.md](SECURITY.md)。
