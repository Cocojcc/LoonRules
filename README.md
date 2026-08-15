# Loon 自建规则

和 GitHub 上那些 `xxx.list` 一样：文件里只写匹配条件，**不写策略**。策略在 Loon 订阅时用 `policy=` 指定。

```text
请求 → 命中某条规则 → 用这条订阅绑定的策略 → 策略再选节点
```

## 分类

| 文件 | 建议策略 | 干什么 |
|---|---|---|
| 文件 | 建议策略 | 来源（你现在 Loon 规则页） |
|---|---|---|
| `rules/reject.list` | REJECT | fmz200 `rejectAd.list`（原订阅关着，内容已拷过来） |
| `rules/lan.list` | DIRECT | 新建，局域网 |
| `rules/personal.list` | DIRECT | 配置里 `[Rule]` 本地直连 |
| `rules/ai.list` | Available | fmz200 `AI.list`（sooyaaabo `AI.lsr` 源已 404） |
| `rules/proxy.list` | Available | blackmatrix7 `Proxy.list` + `Proxy_Domain.list`（6900 条） |
| `rules/direct.list` | DIRECT | `GEOIP,CN`（LoonLiteRules `cn.list`）+ 国内补充 |
| `rules/streaming.list` | Streaming | 新建细分，方便挂 JP/HK |
| `rules/telegram.list` | Available | 新建细分 |
| `rules/twitter.list` | Available | 配置里 `twimg.com` 等 + 细分 |
| `rules/google.list` | Available | 配置里 `gstatic.com` 等 + 细分 |
| `rules/github.list` | Available | 新建细分 |
| `rules/apple.list` | DIRECT | 配置里 `apple.com` + 细分 |

先拆这三类就够用：直连 / 代理 / 拒绝。上面多出来的，是因为它们经常要绑**不同策略组**（AI 锁美、流媒体锁日、苹果直连）。

以后真要再拆，常见还有：Microsoft、Pay、Scholar、Game、Developer（npm / docker / pypi）。没单独需求就先堆在 `proxy.list` / `direct.list`。

## 一行怎么写

```ini
DOMAIN,www.google.com
DOMAIN-SUFFIX,google.com
DOMAIN-KEYWORD,google
IP-CIDR,1.1.1.1/32,no-resolve
IP-CIDR6,2001:db8::/32,no-resolve
GEOIP,CN
USER-AGENT,MicroMessenger*
```

`#` 开头是注释。文件里不要写 `DIRECT` / `Available`，那个交给订阅。

## 接到 Loon

1. 本仓库根目录跑 `python3 -m http.server 8765`
2. 把 `subscribe.conf` 里 `[Remote Rule]` 那几行贴进 Loon 配置，放在 blackmatrix7 那些大列表**上面**
3. 重载配置

推到 GitHub 之后，把地址换成：

```text
https://raw.githubusercontent.com/<用户名>/<仓库>/main/rules/personal.list
```

大名单已经拷进本仓库。接上之后，Loon 里原来的远程规则（blackmatrix7 Proxy、cn.list、AI.lsr、rejectAd）可以关掉，避免两套重复匹配。

`uBlacklist.lsr` 源仓库 404，没拷到。
