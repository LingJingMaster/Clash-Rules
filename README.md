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
