# Clash Rules

一份从 [Shadowrocket-Rules](https://github.com/LingJingMaster/Shadowrocket-Rules) 转换而来的 Mihomo（Clash.Meta）分流配置。保留原配置的服务分组、地区节点选择和规则优先级，添加自己的机场订阅后即可使用。

> 本项目面向 Mihomo 内核，不兼容已经停止维护的 Clash Premium 老内核。

## 默认策略

| 服务 | 默认策略 | 可选策略 |
|---|---|---|
| 🧱 DNS 防泄露 | REJECT | 节点选择、DIRECT |
| 📧 邮件服务 | 节点选择 | DIRECT、日本节点、香港节点 |
| 🔍 谷歌服务 | 日本节点 | 香港节点、节点选择、DIRECT |
| 🤖 AI 服务 | 美国节点 | 节点选择、DIRECT |
| 🍎 苹果推送 | 节点选择 | DIRECT |
| 🍏 苹果服务 | DIRECT | 节点选择 |
| 🏦 汇丰香港 | DIRECT | 香港节点、节点选择 |
| 🏦 香港银行 | DIRECT | 香港节点、节点选择 |
| 📈 券商服务 | 香港节点 | DIRECT、节点选择 |
| 🌍 非中国 | 节点选择 | DIRECT、日本节点 |
| 🐟 漏网之鱼 | 节点选择 | DIRECT、日本节点 |

## 快速开始

1. 下载仓库中的 [`Clash.yaml`](https://raw.githubusercontent.com/LingJingMaster/Clash-Rules/refs/heads/main/Clash.yaml)。
2. 在本地文本编辑器中找到 `proxy-providers.subscription.url`，替换为自己的订阅地址。
3. 将修改后的文件导入 Clash Verge Rev、Clash Meta for Android，或其他使用 Mihomo 内核的客户端。
4. 更新配置中的代理和规则集，确认地区节点组能识别你的节点名称。

请勿把包含私人订阅地址的配置提交到公开仓库。不同客户端的配置覆写方式不一致，首次测试建议直接编辑本地文件。

## 分流顺序

1. 微信 DNS 与局域网例外
2. DNS 防泄露（HTTPDNS）
3. 邮件、Google、AI、YouTube
4. 哔哩哔哩、Telegram、代码托管、微软服务
5. 香港银行、券商、Apple 推送与 Apple 服务
6. 国内服务、非中国服务
7. GEOIP CN 与最终兜底

## 规则来源与复用方式

- 可以被 Mihomo `classical` provider 直接解析的自有规则，继续使用 `Shadowrocket-Rules` 的 Raw 链接：`Mail`、`AI`、`HSBC_HK`、`HK_Broker`、`ApplePush`。
- `Google`、`Apple` 含 Mihomo 不支持的 `USER-AGENT` 规则，转换后的版本存放在本仓库 `rules/`。
- `HK_Banks_Direct` 含按 URL 路径匹配的规则，Mihomo 无法等价表达；本仓库将它转换为主机名规则。
- YouTube、BiliBili、LAN、Telegram、GitHub、GitLab、Atlassian、Microsoft、China、Global 等规则直接使用 [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) 提供的 Clash 原生版本。

## 转换差异

原有 1053 条自有规则中：

| 分类 | 数量 | 处理方式 |
|---|---:|---|
| 可直接使用 | 1026 | `DOMAIN`、`DOMAIN-SUFFIX`、`DOMAIN-KEYWORD`、`IP-CIDR` 保持不变 |
| 需要修改 | 1 | `URL-REGEX,^https?://www\.sc\.com/hk/` 扩大为 `DOMAIN,www.sc.com` |
| 无法等价转换 | 26 | 23 条 Apple 与 3 条 Google `USER-AGENT` 规则省略 |

主配置还进行了以下语法转换：

- `PROTOCOL,TCP` → `NETWORK,TCP`
- `FINAL` → `MATCH`
- Shadowrocket `RULE-SET` / `DOMAIN-SET` → Mihomo `rule-providers`
- `server:system` DNS 例外 → `fake-ip-filter` 与 `direct-nameserver`

Shadowrocket 的 URL Rewrite、MITM 和 `block-quic` 没有进行非等价模拟。完整逐条记录见 [`conversion-report.json`](./conversion-report.json)。

## 文件结构

```text
.
├── Clash.yaml
├── rules/
│   ├── Apple.yaml
│   ├── Google.yaml
│   └── HK_Banks_Direct.yaml
└── conversion-report.json
```

## License

MIT
