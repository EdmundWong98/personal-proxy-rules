# 规则检查记录

检查日期为 2026-11-10，检查基线为 `be648da`。范围覆盖 13 个代理平台列表、5 个直连列表、1 个广告列表和订阅索引。

检查包括规则语法、CIDR 网络边界、重复项、索引文件路径、典型域名匹配，以及官方文档和维护中社区规则的内容差异。历史域名的停用状态需要结合服务迁移和持续观测确认。DNS 无解析、HTTP 403、首页重定向均不足以单独认定规则失效。

## 平台检查结果

| 平台 | 原规则数 | 当前规则数 | 处理结果 |
| --- | ---: | ---: | --- |
| ChatGPT | 11 | 442 | 补充 chat.com、Sora、oaistatsig、WorkOS 登录依赖和 LiveKit 域名，加入官方语音清单中的 402 条 IP 规则 |
| Claude | 4 | 26 | 补充 claude.com、claudeusercontent.com、MCP 内容域名等，保留官方仍有效的两个旧 IP 段，新增 153.61.224.0/22 |
| Copilot | 5 | 28 | 同时覆盖 GitHub Copilot 和 Microsoft Copilot，补充 githubcopilot.com 全部子域、GitHub 登录依赖及新 Copilot 域名 |
| Gemini | 8 | 49 | 采用 Google DeepMind 分类中的域名，覆盖 AI Studio、Gemini API、Code Assist、NotebookLM、Jules、Flow、Antigravity 等 Google AI 服务 |
| Google | 722 | 724 | 同步基础分类变化，删除上游已移除的 Qualys 相关条目，补充新域名和 Google 自有顶级域；googleplex.com 仍被另一维护来源收录，予以保留 |
| Facebook | 570 | 582 | 补充 fb.gg、fb.watch、fbwat.ch、tfbnw.net 及 Meta、Threads 的新增域名 |
| Instagram | 4 | 10 | 补充 ig.me、igcdn.com、igsonar.com、igtv.com、threads.com、threads.net |
| HBO | 48 | 90 | 补充 max.com、地区入口及内容 CDN，移除两个区域级 AWS API Gateway 后缀 |
| Netflix | 1157 | 160 | 原有核心域名已覆盖官方清单，替换陈旧的 1119 条 IP 规则为当前 Netflix 分类的 122 条 IP 规则，移除整个 us-west-2.amazonaws.com 后缀，补充网站 API 关键词 |
| Telegram | 38 | 47 | 对齐官方 14 条 IPv4/IPv6 前缀，补充 91.105.192.0/23、185.76.151.0/24 以及 fragment.com 等域名 |
| Twitch | 21 | 49 | 补充 twitchcon.com 及 27 个明确列出的 CloudFront 主机 |
| Twitter / X | 32 | 34 | 补充 grok.com、twitter.map.fastly.net；原列表已经包含 x.com |
| YouTube | 190 | 190 | 核对后核心域名已覆盖，规则正文与 blackmatrix7 当前版本一致 |

规则数量按有效非注释行计数。删除某个来源中的历史条目代表本次采用了更新的分类依据，不能据此推断所有被删除的 IP 或域名均已停止服务。

## 分流范围调整

- Netflix 的 `us-west-2.amazonaws.com` 会匹配整个 AWS 区域的子域，旧 IP 表还覆盖大量共享 AWS 地址。本次采用 Netflix 专属域名、API 特征和维护中的 Netflix IP 分类。以 IP 直连的非 Netflix AWS 请求将交由后续规则处理。Netflix 官方说明其网站使用 AWS、视频由 Open Connect 交付，客户端仍需分别验证网页和播放。[官方说明](https://openconnect.netflix.com/en/peering/)
- HBO 的 `execute-api.ap-southeast-1.amazonaws.com` 和 `execute-api.us-east-1.amazonaws.com` 会匹配对应区域其他客户的 API Gateway。本次移除这两个泛匹配项，保留明确的平台域名和 CDN。如果客户端日志仍出现 HBO 使用的独立 API Gateway 主机，应按具体主机补充。[HBO 域名来源](https://github.com/v2fly/domain-list-community/blob/2b7ae3222f79e58e323a6b1c30c6618821068963/data/hbo)
- Telegram 将旧的 `91.108.0.0/16`、`5.28.192.0/18` 等历史 CIDR 替换为官方公开前缀。原有 ASN 分类规则继续生效，最终覆盖还依赖客户端 ASN 数据库。[官方 IP 清单](https://core.telegram.org/resources/cidr.txt)
- 订阅顺序调整为平台专用代理规则、公司综合规则、国内直连规则。所有代理条目的默认策略仍为 `PROXY`，自定义地区策略时需检查导入后的实际顺序。

## 直连与广告规则

- `ChinaDomian.list` 删除 `smtcdns.net` 的重复项。`185.188.32.1/28` 含主机位，当前 TeamViewer 来源已删除该条，本次一并移除。[TeamViewer 来源](https://github.com/blackmatrix7/ios_rule_script/blob/0b6897dc17e0c513d25449c935a9694818e32f0c/rule/Loon/TeamViewer/TeamViewer.list)
- `WeChat.list` 从 332 条更新为上游当前 33 条，增加 `btrace.qq.com`，同步移除旧版 233 个关键词及 67 个 IP 条目。原有 ASN 132203 规则继续生效。[WeChat 来源](https://github.com/blackmatrix7/ios_rule_script/blob/0b6897dc17e0c513d25449c935a9694818e32f0c/rule/Loon/WeChat/WeChat.list)
- `cn.list` 改为远程规则集使用的 `GEOIP,CN`，`DIRECT` 策略由订阅索引指定。实际国家归属取决于客户端 GeoIP 数据库。
- `chinaIPTest.list` 删除 2015 条重复 IPv6 规则，保留原文件换行格式。该文件含 104772 条规则，当前订阅索引未引用它，IP 地理归属尚未逐条对照实时注册信息。
- `ChinaCompanyIp.list` 的 221 条 CIDR 格式通过。它按国内云厂商收录，包含海外机房网段，使用 DIRECT 时会产生相应跨境直连行为。
- `ChinaDomian.list` 为原仓库的自定义混合分类，除上述明确问题外，尚未逐条核验域名归属和业务存续。
- `AdvertisingLite.list` 实际只有 405 条规则，旧头部错误标注为 37970 条，其中声明的 23739 条 DOMAIN 和 13824 条 DOMAIN-SUFFIX 均无正文。本次修正头部计数，405 条现有规则格式通过，当前订阅索引未启用它。广告分类准确率需要结合实际请求记录判断。

## 来源快照

| 来源 | 本次读取版本 |
| --- | --- |
| blackmatrix7/ios_rule_script | `0b6897dc17e0c513d25449c935a9694818e32f0c` |
| v2fly/domain-list-community | `2b7ae3222f79e58e323a6b1c30c6618821068963` |
| MetaCubeX/meta-rules-dat，meta 分支 | `e731d6be701afb91548fe0b76380d5022f7f4c02` |
| OpenAI 语音 IP 清单 | 文件 creationTime 为 `2026-10-27T22:28:21.782457+00:00` |

ChatGPT 登录、资源与语音依赖采用 OpenAI 官方说明，Claude 的入站网段采用官方 IP 文档。GitHub Copilot 的鉴权与服务域名采用官方网络参考。[OpenAI 网络说明](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps)、[语音 IP](https://openai.com/chatgpt-voice.json)、[Claude 网络说明](https://code.claude.com/docs/en/network-config)、[Claude IP](https://platform.claude.com/docs/en/api/ip-addresses)、[Copilot 网络参考](https://docs.github.com/en/copilot/reference/copilot-allowlist-reference)

## 验证范围与使用限制

静态检查通过，共 19 个规则文件、108469 条有效规则、17 个订阅目标。已验证规则类型、参数、CIDR 地址族与网络边界、重复项和头部计数。68 个正向与 7 个反向域名用例通过，覆盖本次主要新增入口、API 和 Netflix、HBO 已移除的共享云泛匹配项，同时检查了订阅顺序。

规则包含原有 `USER-AGENT` 和 `IP-ASN` 类型，实际识别能力受 Loon/LoonLite 版本、HTTPS 可见性和客户端数据库影响。ChatGPT 语音 IP 会变化，UDP 3478 还需要节点与客户端支持。GitHub Copilot 的企业专有 GHE 域名及任意第三方 MCP 服务由实际部署决定。

V2Fly 中的动态正则域名未直接转成 Loon 规则，例如 OpenAI Azure WebPubSub 主机。已保留原规则中明确的 Azure 主机，新增精确域名按已确认来源落地。实际遇到动态主机漏分流时，需要结合客户端请求日志补充并核对客户端支持的匹配类型。

本次完成源数据核对与静态验证，实际账号登录、语音、视频播放、地域解锁及全部历史域名的存续状态仍需在你的代理客户端验证。
