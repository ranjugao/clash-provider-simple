# Clash Provider Simple - ChatGPT App 配置包

这个目录提供一个可直接用于 ChatGPT Builder / 自定义 GPT 的配置包，目标是创建一个专门帮助维护 `clash-provider-simple` 的 ChatGPT 应用。

> 说明：官方 Apps SDK 可以构建更完整的 ChatGPT app，通常需要 MCP server 和前端组件。本配置包更适合先创建一个轻量的 ChatGPT/GPT 助手，用来总结需求、生成配置、维护 README、解释 GitHub Actions、产出论坛文案。

## 这个 app 要解决什么问题

用户在 iOS Clash Mi 中使用大量 `rule-providers` 时，可能会遇到 VPN 启动阶段加载过多 provider，导致 HTTP 请求、DNS 解析和 Network Extension 超时风险增加。

当前项目通过 GitHub Actions 把多个来自 `Loyalsoldier/clash-rules` 的规则文件自动合并为 3 个 provider：

- `proxy`
- `direct`
- `reject`

这样 Clash Mi 只需要加载 3 个远程 provider，同时保留自动更新能力。

## 创建方式

1. 打开 ChatGPT 的 GPT / App 创建入口。
2. 名称建议填写：`Clash Provider Simple 助手`。
3. 描述建议填写 `description.md` 的内容。
4. Instructions / 系统指令填写 `instructions.md` 的内容。
5. Conversation starters / 开场白填写 `conversation-starters.md` 的内容。
6. 如果支持 Knowledge，把 `knowledge.md` 作为知识文件上传。

## 建议能力

如果创建环境支持工具或连接器，建议开启：

- GitHub：用于查看仓库、检查 Actions、辅助生成 PR/提交说明。
- Web browsing：用于检查上游 `Loyalsoldier/clash-rules` 状态和官方文档。
- Code / 文件分析：用于检查 shell、YAML、README、Clash 配置。

## 不建议自动做的事

除非用户明确确认，不要自动执行这些高风险动作：

- 强推 `main`。
- 修改 GitHub 账号级权限或 signing key。
- 删除远端仓库或历史提交。
- 上传私钥、token 或任何凭据。

## 相关链接

- 项目仓库：https://github.com/ranjugao/clash-provider-simple
- 上游规则：https://github.com/Loyalsoldier/clash-rules
- OpenAI Apps SDK：https://developers.openai.com/
