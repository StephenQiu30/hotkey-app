# AGENTS.md — hotkey-web（app 端）

本文件为 Codex 与 Claude Code 提供在 hotkey-web 仓库中工作的持久指导。hotkey-web 是 HotKey 平台的 **app 端（Web 客户端工作台）**，独立仓库，通过消费 `hotkey-server` 发布的 OpenAPI 契约与后端交互。

当前工作树仅保留规范文件。以下技术、目录和验证要求用于后续实现，不表示代码、依赖或命令已存在。

## 项目概述

### 技术栈

- Next.js App Router + React + TypeScript
- Tailwind CSS + CSS Variables（设计令牌）
- Radix UI + 自有组合组件（`src/components/ui/`）
- lucide-react（图标）
- axios（HTTP 客户端）
- zustand（状态管理）
- recharts（图表）
- GSAP（动效）
- react-markdown + remark-gfm（证据文档）
- @umijs/openapi（OpenAPI 客户端生成）

### 目录结构

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
├── package.json        # 依赖与脚本
├── tsconfig.json       # TypeScript 配置
├── next.config.ts      # API 代理 rewrites
└── openapi2ts.config.ts  # @umijs/openapi 生成配置
```

## 架构

### API 客户端生成

- 只消费 `hotkey-server` 发布的 OpenAPI 契约（`docs/openapi/openapi.json`）。
- 使用 `@umijs/openapi` 工具，生成路径为 `src/services/hotkey/hotkey-server/`。
- **绝不手写后端 DTO、接口路径或重复服务层**；后端契约变更时，先在后端生成 OpenAPI，再运行 `npm run openapi:generate` 与 `npm run openapi:check`。

### 环境与代理

- `HOTKEY_API_ORIGIN` 只供 Next.js 服务端 rewrites 使用，不以 `NEXT_PUBLIC_*` 暴露后端密钥或内部地址。
- 后端地址、密钥、Token 一律不提交；`.env` 只保留本地可丢弃配置。

## 前端规则

- 页面位于 `src/app/`，业务组件位于 `src/components/`，布局位于 `src/layouts/`，状态位于 `src/stores/`，通用请求与工具位于 `src/lib/`。
- 所有 `*.test.ts`、`*.test.tsx` 与测试初始化位于 `test/`；不得在 `src/` 内创建测试文件。
- 单文件通常保持在 200–500 行；超出时按职责拆分，不创建无职责包装层。
- 复用现有设计令牌和 UI 组合组件，保持键盘可操作、可见焦点、语义化标签、合理对比度与 `prefers-reduced-motion` 支持。
- 同时覆盖桌面与移动布局，以及正常、空、加载、错误和权限不足状态。
- 行为变更遵循测试先行：先保存可复现失败，再做最小实现，最后重构并运行相关回归。纯文档和机械迁移可说明为何不新增行为测试。
- 只在出现第二个真实实现或明确替换需求时提取抽象；避免空目录、占位层、重复 DTO、重复配置和第二套事实源。

## Git、评审与交付

- 提交只包含当前任务文件；提交前检查工作区、生成物、冲突标记和敏感信息。
- Git 提交标题统一使用 Conventional Commits：`<type>(<scope>): <subject>`。`scope` 必填，使用稳定的小写英文模块名（如 `app`、`docs`、`ci`、`repo`）；`subject` 必须使用简体中文动宾短语，冒号后保留一个空格，标题不超过 72 个字符。
- 允许的 `type` 为 `feat`、`fix`、`test`、`refactor`、`docs`、`chore`、`perf`、`build`、`ci` 与 `revert`；禁止使用 `impl`、无 scope 前缀、英文主题或 `feat():xxx` 这类空 scope/缺少空格的变体。示例：`feat(app): 新增监控空状态`。
- 提交正文和脚注统一使用简体中文；正文按“变更摘要”“变更原因”“验证”三个段落记录实际内容、原因与已通过命令，禁止使用无信息量的“更新代码”“修复问题”。
- 不兼容变更使用 `<type>(<scope>)!:`，并在脚注以 `BREAKING CHANGE: <中文迁移说明>` 记录影响和迁移方式。
- 一个提交只完成一个可独立验收的任务；生成物必须与源文件同提交，纯格式化、顺手重构和无关文档不得混入功能提交。
- 行为变更的提交顺序保持测试、最小实现、重构/文档可审查；不得通过放宽断言掩盖失败。
- Pull Request 说明用户影响、实现边界、测试命令与结果、OpenAPI/配置/部署影响和残余风险。
- 未经用户明确要求，不创建提交、不推送、不创建或合并 Pull Request。

## 验证要求

当前规范变更检查文件一致性与 `git diff --check`。恢复实现后，必须提供并执行依赖安装、OpenAPI 漂移、类型、单元测试与构建检查；交付时说明实际结果与未覆盖风险。
