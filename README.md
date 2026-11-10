# Personal Proxy Rules

EdmundWong98 的个人 Loon / LoonLite 代理规则仓库。

基于 [Loon0x00/LoonLiteRules](https://github.com/Loon0x00/LoonLiteRules) 克隆，保留原仓库历史与规则目录，新增 `proxy/Twitch.list`。

## 规则来源

- 原始规则来自 [LoonLiteRules](https://github.com/Loon0x00/LoonLiteRules)。
- 部分原始规则及 Twitch 规则来自 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)。
- Twitch 采用上游 Loon 规则中的 21 条规则，覆盖域名、关键词、IPv4 和 IPv6。

## Raw 链接

- [ChatGPT.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/ChatGPT.list)
- [Twitch.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/Twitch.list)
- [规则索引 list.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/list.list)

仓库为 private，以上 raw 地址需要有效鉴权，匿名访问通常返回 404。GitHub 文件页面 Raw 按钮生成的带 token 地址会过期，不适合长期订阅。`list.list` 中的个人 raw 地址也受同样限制。

支持自定义 HTTP 请求头的客户端可通过 GitHub Contents API 读取文件，设置 `Authorization: Bearer <TOKEN>` 和 `Accept: application/vnd.github.raw+json`。Token 至少需要本仓库 Contents 只读权限。

```text
https://api.github.com/repos/EdmundWong98/personal-proxy-rules/contents/proxy/ChatGPT.list?ref=main
https://api.github.com/repos/EdmundWong98/personal-proxy-rules/contents/proxy/Twitch.list?ref=main
```

客户端不支持鉴权请求头时，可在本地下载规则后导入。代理策略名称按客户端中的实际策略组配置。
