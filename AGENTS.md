# AGENTS.md — hotkey-app

本文件适用于整个仓库。`hotkey-app` 是独立 Flutter 客户端，由原 `hotkey-web` 本地重命名而来。技术基线见 [PROJECT.md](PROJECT.md)，交接见 [HANDOVER.md](HANDOVER.md)。Web 工作台统一在 `hotkey-server/frontend/`。

当前只有规范与协作配置，没有 Flutter 应用或依赖清单；下列目录和命令属于后续初始化约束，不能声称已经存在或验证通过。

## 固定技术与职责

- 固定 Flutter + Dart，依赖使用 Flutter/Dart pub；应用提交 `pubspec.lock`，初始化时锁定 Flutter/Dart 兼容版本。
- 后端业务与数据库由 `hotkey-server` 负责；App 消费其 FastAPI OpenAPI 契约，不直连 PostgreSQL、Redis、Kafka、MinIO 管理接口。
- 后端 DTO 和端点由同提交服务端运行时 `/openapi.json` 生成，离线输入只能是 CI 自动导出产物，不维护第二份契约；生成路径固定 `lib/api/`；具体 Dart 生成器和 HTTP 库在初始化切片确定并锁定，禁止复制旧 TypeScript 客户端。
- 不恢复 Next.js、React、TypeScript、pnpm 或 Axios 应用。用户已确定 Flutter，不重复询问框架；新增有实质影响的插件或外部服务才按对应设计处理。
- 目标平台、应用标识、状态管理和导航方案按当前切片冻结；不擅自创建所有平台，不预装无使用场景的框架。

## 目录与开发

- 实施前必须核验 [Server 046 前置计划](../hotkey-server/docs/plans/046-全局异常与响应契约前置计划.md) S03 通过证据，统一协议见同编号 Design；设计准备可先开展。App 业务接入还须完成 Server BACKLOG 的 APP-02 契约与设备验证。
- 单一传输入口消费生成 DTO，按 code/status 分支，读取 details 及请求 ID 回退；分别验证网络、超时、取消、非 JSON、204、文件、会话失效和失败任务查询。不复制服务端错误模型、不把所有异常包装成 HTTP 500、不在传输层自动重试写请求。

- `lib/main.dart` 只做启动；`lib/app/` 负责应用装配、主题和导航，`lib/features/` 按业务功能组织，`lib/api/` 保存生成客户端。
- 单元/Widget 测试放 `test/`，设备集成测试放 `integration_test/`；平台工程由 Flutter 官方工具生成。
- Dart 文件用 snake_case，类用 UpperCamelCase，成员用 lowerCamelCase；遵循 Dart 格式与分析规则。
- 修改前读对应需求、设计与测试；行为变化先保存失败验证，再最小实现和回归。纯文档、目录改名可说明不新增行为测试的原因。
- UI 覆盖正常、空、加载、错误和无权限状态；关注触控、键盘、语义化标签、字体缩放、对比度和目标平台交互。
- 生命周期、断网重连、取消及重试按接口语义处理；禁止因页面重建无界重复调用。
- 只在实际切片创建目录；不新增通用包装层、第二套 DTO 或重复服务事实源。

## 配置与安全

- 生产 API 使用 HTTPS；API 地址属于可见客户端配置，不能在 `--dart-define`、资源或源码中放服务端秘密。
- 会话凭据按目标平台安全存储处理，日志/截图/崩溃信息须脱敏；退出清理必要的会话与敏感缓存。
- 移动端身份、刷新、过期和退出由后端契约定义，不照搬 Web 同源 Cookie/CSRF 假设。
- 不提交 `.env`、签名私钥、keystore、账号会话、真实用户资料或本地产物。

## Git、评审与交付

- 提交只包含当前任务文件；提交前检查工作区、生成物、冲突标记和敏感信息。
- Git 提交标题统一使用 `type(scope):中文描述`，冒号后不加空格。`scope` 必填，使用稳定的小写英文模块名（如 `app`、`docs`、`ci`、`repo`）；描述必须使用简体中文动宾短语，标题不超过 72 个字符。
- 允许的 `type` 为 `feat`、`fix`、`test`、`refactor`、`docs`、`chore`、`perf`、`build`、`ci` 与 `revert`；禁止使用 `impl`、无 scope 前缀或英文描述。示例：`feat(app):新增监控空状态`。
- 提交正文和脚注统一使用简体中文；正文按“变更摘要”“变更原因”“验证”三个段落记录实际内容、原因与已通过命令，禁止使用无信息量的“更新代码”“修复问题”。
- 不兼容变更使用 `<type>(<scope>)!:`，并在脚注以 `BREAKING CHANGE: <中文迁移说明>` 记录影响和迁移方式。
- 一个提交只完成一个可独立验收的任务；生成物必须与源文件同提交，纯格式化、顺手重构和无关文档不得混入功能提交。
- 行为变更的提交顺序保持测试、最小实现、重构/文档可审查；不得通过放宽断言掩盖失败。
- Pull Request 说明用户影响、实现边界、测试命令与结果、OpenAPI/配置/平台构建影响和残余风险。
- 未经用户明确要求，不创建提交、不推送、不创建或合并 Pull Request。

## 验证要求

当前规范修改检查本地引用、技术一致性和 `git diff --check`。Flutter 初始化后执行依赖解析、Dart 格式、`flutter analyze`、`flutter test`、目标平台构建、OpenAPI 漂移和必要设备集成验证；命令必须基于实际文件与 SDK。真实设备/模拟器结果与单元测试分别记录，不用 Web 浏览器证据替代 App 验收。
