# HotKey App · 知微见澜

HotKey 的独立 Flutter 客户端仓库。HotKey 计划围绕关键词监控公开或获授权内容，并提供可追溯的舆情报告；正在开发的后端与 Web 工作台位于 [hotkey-server](https://github.com/StephenQiu30/hotkey-server)。

## 项目状态

**Flutter 应用尚未初始化。**本仓库目前只有技术约束、设计参考和协作配置，没有 `pubspec.yaml`、`lib/`、平台工程或可运行安装包。因此目前没有 App 安装、启动或设备测试步骤。服务端的 Web 页面和 API 验证也不代表移动端功能已经完成。

已确定客户端使用 Flutter + Dart，从 HotKey 服务端运行时 OpenAPI 生成客户端，不复制 Web 工程或手写第二套服务端数据模型。目标平台、应用标识、鉴权方式与首批功能需要在初始化切片中确定。范围与依赖见 [项目说明](PROJECT.md)，当前交接见 [HANDOVER](HANDOVER.md)。

## 参与项目

欢迎先从文档、范围讨论和接口契约审查参与。创建应用或提交功能前，请阅读 [贡献指南](CONTRIBUTING.md) 与 [工程规范](AGENTS.md)，并与服务端当前 [进度看板](https://github.com/StephenQiu30/hotkey-server/blob/main/BACKLOG.md) 对齐。安全问题按 [安全策略](SECURITY.md) 私密报告。

本项目采用 [MIT 许可证](LICENSE)。
