# 项目知识库

## 项目名称

clash-provider-simple

## 项目地址

https://github.com/ranjugao/clash-provider-simple

## 上游规则来源

https://github.com/Loyalsoldier/clash-rules

上游项目提供 Clash Premium RULE-SET 规则集。本项目不原创规则内容，只做自动拉取、合并、去空行、去重和发布。

## 背景问题

在 iOS Clash Mi 中，如果配置了 10+ 个 rule-providers，VPN 启动阶段可能需要同步加载多个远程 provider。

风险包括：

- 多个 HTTP 请求同时发生
- 多次 DNS 解析
- 网络不稳定时阻塞启动
- iOS Network Extension 超时
- VPN 启动失败

## 核心思路

把规则动态更新从 Clash Mi 客户端转移到 GitHub Actions。

GitHub Actions 负责：

- 拉取上游规则
- 合并为 3 个 provider 文件
- 删除空行
- 使用 sort -u 去重
- 有变化才提交，避免空 commit

Clash Mi 只加载：

- proxy
- direct
- reject

## 合并关系

proxy:

- proxy.txt
- gfw.txt
- tld-not-cn.txt
- google.txt

direct:

- direct.txt
- cncidr.txt
- lancidr.txt
- apple.txt
- icloud.txt

reject:

- reject.txt
- private.txt

## Raw URLs

https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_proxy.txt
https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_direct.txt
https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_reject.txt

## Clash Mi 配置示例

```yaml
rule-providers:
  proxy:
    type: http
    behavior: domain
    url: https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_proxy.txt
    interval: 21600

  direct:
    type: http
    behavior: domain
    url: https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_direct.txt
    interval: 21600

  reject:
    type: http
    behavior: domain
    url: https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_reject.txt
    interval: 21600

rules:
  - RULE-SET,reject,REJECT
  - RULE-SET,direct,DIRECT
  - RULE-SET,proxy,PROXY
```

如果用户的代理策略组不叫 PROXY，需要提醒用户替换为实际策略组名。

## 更新机制

当前 workflow 支持：

- 每小时 :15 和 :45 检查上游 release 分支 SHA
- 上游 SHA 变化后等待 10 分钟再合并
- 每 6 小时强制刷新一次作为兜底
- 手动 workflow_dispatch 触发

## 写作语气

中文优先，语气清楚、克制、适合技术论坛分享。强调：

- 这是个人使用中遇到的问题和解决方案
- 尊重并致谢上游项目
- 不夸大效果，不保证适配所有客户端
- 提醒用户检查 behavior、策略组名称和客户端兼容性
