# Loon 自建规则

文件里只写匹配条件，**不写策略**。策略在 Loon 订阅时用 `policy=` 指定。

只有一条 VPN 订阅，翻墙全部进 `Available`，不再按 Apple / Google / 流媒体拆策略组。

## 分类

| 文件 | 策略 | 内容 |
|---|---|---|
| `rules/reject.list` | REJECT | 广告 / 追踪 |
| `rules/lan.list` | DIRECT | 局域网 |
| `rules/personal.list` | DIRECT | 公司、内网、节点 IP |
| `rules/direct.list` | DIRECT | 国内站点 + `GEOIP,CN` |
| `rules/ai.list` | Available | AI 域名，方便单独加，策略和 proxy 一样 |
| `rules/proxy.list` | Available | 所有翻墙：Google / Twitter / GitHub / Telegram / 流媒体 / Apple |

要加翻墙域名，直接写进 `proxy.list`（或 AI 写进 `ai.list`），不用新建分类。

## 一行怎么写

```ini
DOMAIN,www.google.com
DOMAIN-SUFFIX,google.com
DOMAIN-KEYWORD,google
IP-CIDR,1.1.1.1/32,no-resolve
GEOIP,CN
```

## 接到 Loon

仓库根目录：

```bash
python3 -m http.server 8765
```

把 `subscribe.conf` 里 `[Remote Rule]` 贴进 Loon，重载。
