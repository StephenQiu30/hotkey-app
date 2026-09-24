# 为 HotKey App 贡献

`hotkey-app` 使用 Flutter + Dart，是 HotKey 独立客户端。先阅读 [PROJECT.md](PROJECT.md)、[AGENTS.md](AGENTS.md) 和 [HANDOVER.md](HANDOVER.md)。Web 工作台在 `hotkey-server/frontend/`。

## 当前状态与开发约束

目前仅有规范与协作配置，Flutter 应用尚未初始化。新功能先明确需求、平台、设计与验收；后端新能力先在 server 对齐 OpenAPI，再生成 Dart 客户端。

使用 Flutter/Dart pub，提交应用 `pubspec.lock`。代码放 `lib/`，单元/Widget 测试放 `test/`，设备集成测试放 `integration_test/`。不手写另一套后端 DTO，不提交凭据、签名材料、用户数据或构建产物。

UI 变更说明目标平台、交互状态、字体缩放与可访问性影响。安全问题按 [安全策略](SECURITY.md) 私密报告。

## 提交前验证

纯规范变更执行 `git diff --check` 并检查引用。应用初始化后按范围执行 Dart 格式、`flutter analyze`、`flutter test`、契约生成检查、平台构建和设备集成测试。交付明确实际命令、结果与未覆盖平台，不把 SDK 缺失误称业务测试失败。

## Git 提交规范

每个提交只表达一个可审查目的，标题统一使用：

```text
<type>(<scope>):<中文描述>
```

- `scope` 必填，使用稳定的小写英文模块名，例如 `app`、`docs`、`ci` 或 `repo`。
- `type` 只使用 `feat`、`fix`、`test`、`refactor`、`docs`、`chore`、`perf`、`build`、`ci` 或 `revert`。
- `subject` 使用简体中文动宾短语；冒号后不加空格，标题不超过 72 个字符，不使用英文主题、`impl`、空 scope 或自定义前缀。
- 不兼容变更在冒号前增加 `!`，并在正文添加 `BREAKING CHANGE:` 与迁移说明。
- 行为变更按 `test` → `feat`/`fix` → `refactor`/`docs` 的顺序提交；提交正文使用中文，并以“变更摘要”“变更原因”“验证”记录实际内容和命令。

```text
feat(app):新增监控空状态
fix(app):恢复弹窗关闭后的触发器焦点
test(api):覆盖生成客户端请求映射
docs(repo):固定 Flutter 客户端规范
```
