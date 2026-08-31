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

## 那些 IP 是干什么的

绝大多数网站用**域名**匹配就够了。IP 规则只留给「请求里根本没有域名」的情况：

| 留下的 IP | 为什么 |
|---|---|
| 你的 3 个节点/服务器地址 | 代理自己连自己，必须直连 |
| `10/8` `172.16/12` `192.168/16` 等 | 局域网、回环、Fake-IP |
| `GEOIP,CN` **一条** | 中国大陆 IP 全覆盖，替代原来 1.6 万条 CN 网段 |
| Telegram 那几段 | 客户端经常直接连 IP，光写域名拦不住 |

已删掉的：

- 从 ChinaMax / Loyalsoldier 拷来的海量中国 IP——和 `GEOIP,CN` 重复
- AWS / GCP 大段（比如 `13.32.0.0/15`）——太宽，国内也有服务跑在上面
- 广告 IP——变得快，还容易误伤公共 DNS
- `DOMAIN-SUFFIX,jp/hk/us/...` 整国后缀——太粗

广告域名本身就不需要「能访问」，那是用来拦的，不是死链。

名单来源（2026-08-15，热门且仍在更新，已转成 Loon 语法去重）：

- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) Global / ChinaMax / Advertising
- [Loyalsoldier/clash-rules](https://github.com/Loyalsoldier/clash-rules) proxy / direct / reject / gfw / cncidr（日更）
- [ACL4SSR/ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) GFW / China / BanAD / OpenAi
- [TG-Twilight/AWAvenue-Ads-Rule](https://github.com/TG-Twilight/AWAvenue-Ads-Rule) 秋风广告
- [fmz200/wool_scripts](https://github.com/fmz200/wool_scripts) rejectAd / AI
- [Johnshall/Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever) sr_ad_only.conf（release 快照，按 CC BY-SA 4.0 标注并去重合并到 reject.list）
- [Loon0x00/LoonLiteRules](https://github.com/Loon0x00/LoonLiteRules) GEOIP,CN

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
