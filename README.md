# Personal Proxy Rules

EdmundWong98 的个人 Loon / LoonLite 代理规则仓库。

基于 [Loon0x00/LoonLiteRules](https://github.com/Loon0x00/LoonLiteRules) 克隆。最近一次全量静态检查和平台来源核对为 2026-11-10，详情见 [规则检查记录](AUDIT.md)。

## 规则来源

- 原始规则来自 [LoonLiteRules](https://github.com/Loon0x00/LoonLiteRules)。
- 原始规则与基础分类来自 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)。
- 新域名参考 [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)，Netflix IP 段参考 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)。
- ChatGPT 语音 IP、Claude API IP、Telegram IP 采用平台官方公开列表。各文件头记录具体来源。

## Raw 链接

- [ChatGPT.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/ChatGPT.list)
- [Twitch.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/Twitch.list)
- [规则索引 list.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/list.list)

仓库为 public，raw 地址支持匿名订阅。其他平台可将地址末尾替换为对应文件名。

`list.list` 是 Loon 远程规则配置条目集合，按客户端导入方式使用。单个平台 `.list` 可直接作为远程规则订阅，策略选择 `PROXY` 或你的实际代理策略组。

平台专用规则排在 Google、Facebook 等综合规则之前，国内域名与 IP 直连规则靠后。按客户端实际规则顺序匹配，避免前置通用规则覆盖平台策略。

ChatGPT 语音使用 UDP 3478，代理节点和客户端需要支持 UDP。规则检查覆盖格式、来源差异和典型域名匹配，账号地区、节点解锁、实际播放与登录效果需要在客户端验证。平台和 CDN 地址会变化，各列表为本次审核时的静态快照。
