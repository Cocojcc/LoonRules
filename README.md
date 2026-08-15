# Loon 自建规则

文件里只写匹配条件，**不写策略**。策略在 Loon 订阅时用 `policy=` 指定。

只有一条 VPN 订阅，翻墙全部进 `Available`。

## 分类

| 文件 | 策略 | 内容 |
|---|---|---|
| `rules/reject.list` | REJECT | 广告 / 追踪 |
| `rules/direct.list` | DIRECT | 个人/公司、局域网、国内、GEOIP CN |
| `rules/ai.list` | Available | AI 域名，策略和 proxy 一样 |
| `rules/proxy.list` | Available | 所有翻墙 |

直连写 `direct.list`，翻墙写 `proxy.list`。

## 一行怎么写

```ini
DOMAIN-SUFFIX,google.com
IP-CIDR,1.1.1.1/32,no-resolve
GEOIP,CN
```

## 接到 Loon

把 `subscribe.conf` 里 `[Remote Rule]` 贴进 Loon，重载。

```
https://raw.githubusercontent.com/Cocojcc/LoonRules/master/rules/
```
