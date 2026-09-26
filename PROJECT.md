# HotKey App 项目与技术选型

更新日期：2026-09-21。本文件是 `hotkey-app` 的技术选型入口。工程规范见 [AGENTS.md](AGENTS.md)，当前交接见 [HANDOVER.md](HANDOVER.md)。

## 1. 定位与固定技术

`hotkey-app` 是 HotKey 独立客户端仓库，由原 `hotkey-web` 本地重命名而来。**固定技术为 Flutter + Dart，依赖通过 Flutter/Dart pub 管理。** Web 工作台由 `hotkey-server/frontend/` 使用 Next.js 实现，本仓库不保留 Next.js/React/TypeScript Web 工程。

| 项目 | 固定约定 |
|---|---|
| 应用框架与语言 | Flutter + Dart |
| 依赖管理 | `pubspec.yaml` 声明依赖，应用提交 `pubspec.lock` |
| 业务 API | 消费 `hotkey-server` 发布的 FastAPI OpenAPI 契约 |
| 数据边界 | 客户端不直连 PostgreSQL、Redis、Kafka 或持有服务端密钥 |
| 质量 | Dart 格式与分析、Flutter 单元/Widget 测试、目标平台构建、设备集成验证 |

Flutter/Dart 兼容版本在初始化时锁定。目标平台、应用标识、状态管理、路由、HTTP 库和 Dart OpenAPI 生成器尚未冻结，按首个实际功能切片决定；不预装一套未经决定的 Flutter 插件。

## 2. 目录与契约

当前实际存在根 PROJECT、HANDOVER、AGENTS、贡献/安全规范及协作配置；Flutter 应用尚未初始化。

以下为初始化后的目录约定，只按真实切片创建：

```text
hotkey-app/
├── lib/
│   ├── main.dart          # 启动入口
│   ├── app/               # 应用装配、主题与导航
│   ├── features/          # 业务功能
│   └── api/               # OpenAPI 生成客户端
├── test/                  # 单元与 Widget 测试
├── integration_test/      # 设备集成测试
├── pubspec.yaml
└── pubspec.lock
```

平台目录由 Flutter 官方工具按已确定目标生成，不手工伪造平台工程。唯一契约读取同版本服务端运行时 `/openapi.json`，源为 FastAPI 路由与 Pydantic；需要离线归档时仅使用同提交 CI 导出的产物，不手工维护 OpenAPI 文件；禁止从旧 Web 客户端复制 DTO。生成器确定后锁定配置和版本，生成代码可复现，业务调用通过单一客户端适配入口。

客户端只保存必要的会话与展示数据，秘密使用目标平台安全存储；API 地址属于可见配置，不将打包配置误当成秘密。移动端鉴权、刷新与退出按服务端契约实现，不能直接照搬 Web 的同源 Cookie/CSRF 假设。

## 3. 开发与验证

公共异常、HTTP 状态、错误码、资源/分页/任务响应遵循 [Server 046 Design](https://github.com/StephenQiu30/hotkey-server/blob/main/docs/design/046-全局异常与响应契约设计.md)。Server 046 S03 通过后才开始本仓库实现；范围与设计准备可先开展。App 业务接入另须完成 APP-02 的生成客户端和设备契约验证，Web 通过不代表 App 通过，服务端门禁也不反向等待 Flutter 初始化。

单一客户端入口使用生成的 ErrorView，正确读取 details 和响应头/body request_id；分别处理 HTTP、网络、超时、取消、非 JSON。失败任务的成功查询仍按 HTTP 200 消费业务状态；取消请求不等于已取消任务。字段/页面/操作反馈由功能层决定，不按 message 匹配、不自动重试写操作；204 与文件不进入 JSON 包装。

优先实现核心监控、事件、证据流程，覆盖正常、空、加载、失败和无权限状态，并考虑生命周期、网络中断与无障碍。产品需求来自 server 的统一 PRD；App 功能和设备验收需独立记录，不继承 Web 浏览器结果。

Flutter 初始化后建立以下检查入口；当前没有 pubspec，不宣称这些命令已经通过：

- `flutter pub get`
- `dart format --output=none --set-exit-if-changed lib test`（只传实际存在的目录）
- `flutter analyze`
- `flutter test`
- 已选平台的构建及 `integration_test` 设备验证
- OpenAPI 生成漂移检查（生成工具确定后补充命令）

## 4. 重命名与交付

本地目录由 `hotkey-web` 改名为 `hotkey-app`，保留 `.git`、分支和提交历史。当前 `origin` 为 `https://github.com/StephenQiu30/hotkey-app.git`；客户端仍未初始化。历史重命名记录见 [HANDOVER.md](HANDOVER.md)。

两个项目分别在各自仓库根维护 PROJECT 与 HANDOVER。这里不再重复 server 的基础设施选型，以避免两份后端规范漂移；服务端接口变化必须同步生成客户端及验证。

依据：[Flutter 官方项目创建说明](https://docs.flutter.dev/reference/create-new-app)。本文固定技术与目录方向，不代替应用实现或设备验收。

## 5. 产品交付编排

统一需求和跨仓库排期见 [Server BACKLOG](https://github.com/StephenQiu30/hotkey-server/blob/main/BACKLOG.md) 的 App 交付队列。App 独立安排目标平台、鉴权、监控/事件/证据及设备验证；Web 的 M5 验收不代表 App 完成，App 未初始化也不应让已冻结的 Web 首版无限等待。目标平台和首批功能由 App 范围切片冻结，当前不扩为全部平台或全功能对齐。
