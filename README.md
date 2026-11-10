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

## 分类集合

| 分类 | 覆盖平台 | Raw 订阅 |
| --- | --- | --- |
| 海外流媒体 | YouTube、Twitch、Netflix、HBO / Max | [OverseasStreaming.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/OverseasStreaming.list) |
| 海外 AI | ChatGPT / Sora、Claude、GitHub / Microsoft Copilot、Gemini / Google AI，以及已有 Grok、Meta AI 域名 | [OverseasAI.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/OverseasAI.list) |
| 海外社交软件 | Facebook、Instagram / Threads、Twitter / X、Telegram | [OverseasSocial.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/proxy/OverseasSocial.list) |
| CN 地区直连 | 国内常用域名、微信匹配规则、CN GeoIP | [China.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/direct/China.list) |

集合基于仓库现有平台列表合并，逐条去重。Twitter 中的 `grok.com`，以及 Facebook 中的 `llama.com`、`meta.ai`、`metaaiusercontent.com`、`metaaivm.com`、`muse.ai` 归入 AI 集合。

分类导入条目见 [categories.list](https://raw.githubusercontent.com/EdmundWong98/personal-proxy-rules/main/categories.list)。海外三类可分别绑定到流媒体、AI、社交策略组，CN 集合绑定 `DIRECT`。采用 AI、流媒体、社交、CN 的顺序，Google 等综合代理规则按需插在 CN 之前。社交集合继承 Facebook 的宽泛域名和 ASN 规则，规则顺序会影响重叠请求的分类。

CN 集合提取 `ChinaDomian.list`、`WeChat.list` 中的域名、关键词和 User-Agent 规则，去重后以 `GEOIP,CN` 覆盖国内 IP。IP 归属依赖客户端 GeoIP 数据库。国内云厂商的海内外混合 IP 清单、旧测试 IP 清单及源列表中的固定 IP/ASN 条目由各自独立列表维护。国内域名或微信请求可以解析至境外服务，CN 集合会按域名或应用规则直连这些请求。

更新单平台列表后，在仓库根目录执行以下命令重新生成集合和分类索引。

```sh
python3 scripts/build_collections.py
```

集合包含来源列表中的辅助登录、遥测、共享 CDN 和 IP 规则。集合覆盖范围以表内平台为准，具体账号、节点与地区可用性按客户端实际结果验证。

仓库为 public，raw 地址支持匿名订阅。其他平台可将地址末尾替换为对应文件名。

`list.list` 是 Loon 远程规则配置条目集合，按客户端导入方式使用。单个平台 `.list` 可直接作为远程规则订阅，策略选择 `PROXY` 或你的实际代理策略组。

平台专用规则排在 Google、Facebook 等综合规则之前，国内域名与 IP 直连规则靠后。按客户端实际规则顺序匹配，避免前置通用规则覆盖平台策略。

ChatGPT 语音使用 UDP 3478，代理节点和客户端需要支持 UDP。规则检查覆盖格式、来源差异和典型域名匹配，账号地区、节点解锁、实际播放与登录效果需要在客户端验证。平台和 CDN 地址会变化，各列表为本次审核时的静态快照。
