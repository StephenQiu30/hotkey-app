# 为 HotKey Web（app 端）贡献

hotkey-web 是 HotKey 平台的 app 端（Web 客户端工作台），独立仓库。开始修改前，请阅读仓库根目录唯一的 `AGENTS.md`；参与协作时保持尊重并聚焦可验证事实。

## 从哪里开始

- 小型修复、测试、文案和文档改进可以直接提交 Pull Request。
- 新功能、跨页面交互或大型重构，请先创建 Feature Request 对齐问题、范围和验收标准。
- 需要后端新能力时，先在 `hotkey-server` 仓库对齐契约，再在本仓库消费生成的 OpenAPI 客户端。
- UI 改动请说明目标用户、桌面与移动视口、交互状态和可访问性影响。
- 安全问题不得公开披露，请按 [安全策略](SECURITY.md) 使用私密报告渠道。

## 当前状态

仓库仅保留规范文件。代码、依赖和运行配置恢复后，再补充真实可执行的开发与验证命令。

## 开发约束

- 使用 Next.js App Router、React、TypeScript、Tailwind CSS 和现有 UI 组合组件；测试位于 `test/`。
- API 类型与请求函数只由 `hotkey-server` 发布契约生成，不手写后端 DTO 或接口路径。
- 不提交 `.env`、Token、用户数据、数据库内容、构建产物或本地工具目录。
- 修改后端契约后，先在后端生成 OpenAPI，再执行 `npm run openapi:generate` 并审查生成差异。

## 提交前验证

当前规范变更执行 `git diff --check` 并检查引用。恢复实现后，按变更范围执行 OpenAPI 漂移、类型、单元测试、构建与依赖安全检查。Pull Request 必须说明用户影响、实现边界、真实验证结果与未覆盖风险。

## Git 提交规范

每个提交只表达一个可审查目的，标题统一使用：

```text
<type>(<scope>): <subject>
```

- `scope` 必填，使用稳定的小写英文模块名，例如 `app`、`docs`、`ci` 或 `repo`。
- `type` 只使用 `feat`、`fix`、`test`、`refactor`、`docs`、`chore`、`perf`、`build`、`ci` 或 `revert`。
- `subject` 使用简体中文动宾短语；冒号后保留一个空格，标题不超过 72 个字符，不使用英文主题、`impl`、空 scope 或自定义前缀。
- 不兼容变更在冒号前增加 `!`，并在正文添加 `BREAKING CHANGE:` 与迁移说明。
- 行为变更按 `test` → `feat`/`fix` → `refactor`/`docs` 的顺序提交；提交正文使用中文，并以“变更摘要”“变更原因”“验证”记录实际内容和命令。

```text
feat(app): 新增监控空状态
fix(app): 恢复弹窗关闭后的触发器焦点
test(services): 覆盖 OpenAPI 客户端请求映射
docs(repo): 按 app 端定位重建规范文件
```
