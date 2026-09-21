# HotKey App 交接

更新日期：2026-09-18。先阅读 [PROJECT](PROJECT.md)、[AGENTS](AGENTS.md)、[贡献规范](CONTRIBUTING.md)。

## 1. 本次完成

- 本地仓库由 `hotkey-web/` 重命名为 `hotkey-app/`，保留完整 `.git` 与历史。
- 在仓库根新增 PROJECT.md 与 HANDOVER.md，固定 Flutter + Dart；Web 工作台归 `hotkey-server/frontend/`。
- 重写 Flutter 工程约束，同步贡献/安全文档、忽略规则、编辑器配置和问题/PR 模板，去除 Node/Next.js 工程的现行要求。

## 2. 当前状态

- 2026-09-21 已对齐 [Server 046 统一异常与响应设计](../hotkey-server/docs/design/046-全局异常与响应契约设计.md)；046 S03 是 App 实施前置，APP-02 另做设备契约验证。目前仅同步规范，未初始化或验证 Flutter 工程。

本轮起点为本地 `main`、HEAD `8901017`，起点工作区干净。重命名不会生成 Git 提交，当前文档与配置改动尚未提交或推送。

现有规范已经切换 Flutter；没有 `pubspec.yaml`、Flutter SDK 版本锁、`lib/`、平台工程或应用测试，本次不代表 Flutter 应用已创建或可运行。没有安装依赖、执行设备构建或运行测试。

`origin` 仍为原 GitHub `hotkey-web` 仓库，安全报告入口仍指向该实际远端。本轮未改名 GitHub 仓库或改写远端 URL，也未 fetch 或检查远端 CI。

## 3. 下一步

1. 冻结首批目标平台与应用标识，锁定 Flutter/Dart 版本，用官方工具在当前仓库初始化，保留现有规范。
2. 与 server 对齐鉴权、分页与错误契约；选定并锁定 Dart OpenAPI 生成器和传输库，在 `lib/api/` 生成客户端。
3. 按实际切片建立 `lib/app/`、`lib/features/`、测试和平台配置，避免空业务模块；客户端不得直连服务端基础设施。
4. 执行格式、分析、单元/Widget、构建与设备集成测试后记录证据；仅目录改名或 API 通畅不等于 App 验收。

## 4. 本轮验证与维护

已通过：本地 Markdown 引用、旧 Web 现行要求残留检查、`git diff --check`，以及 HEAD 保留核对。`git check-ignore` 确认 `pubspec.lock`、`lib/main.dart` 可跟踪，`.dart_tool`、构建产物和签名材料被忽略。Flutter 构建、设备和真实 API 联调未执行。

后续更新真实改动、运行命令、测试结果和未决项；技术变动回写 PROJECT。发布前需要单独准备签名、分发和平台验收证据，不将当前规范状态写成已发布版本。

## 2026-09-21 文档复核

已纠正 PROJECT 的失效 OpenAPI 快照引用，统一读取 server 运行时契约；App 初始化、身份接入、核心流程与设备验收已列入 Server BACKLOG 的独立队列。本轮只做文档修订；以上 2026-09-18 的起点、改名与检查属于历史记录，不能当作当前 Git 状态。当前核对 HEAD 为 `0c695ea`，Flutter 工程仍未初始化，未重跑设备或构建验证。
