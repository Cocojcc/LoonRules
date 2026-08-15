# Loon 自建规则

和 GitHub 上那些 `xxx.list` 一样：文件里只写匹配条件，**不写策略**。策略在 Loon 订阅时用 `policy=` 指定。

```text
请求 → 命中某条规则 → 用这条订阅绑定的策略 → 策略再选节点
```

## 分类

| 文件 | 建议策略 | 干什么 |
|---|---|---|
| `rules/reject.list` | REJECT | 广告 / 追踪 |
| `rules/lan.list` | DIRECT | 局域网、回环 |
| `rules/personal.list` | DIRECT | 你自己的公司、内网、节点 IP |
| `rules/ai.list` | Available 或单独 AI 组 | ChatGPT / Claude / Grok |
| `rules/streaming.list` | Streaming | Netflix / Disney+ / YouTube |
| `rules/telegram.list` | Available | Telegram |
| `rules/twitter.list` | Available | X / Twitter |
| `rules/google.list` | Available | Google |
| `rules/github.list` | Available | GitHub |
| `rules/apple.list` | DIRECT | 苹果（要走代理就改 policy） |
| `rules/proxy.list` | Available | 其它必须翻墙的 |
| `rules/direct.list` | DIRECT | 国内站点补充 |

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

订阅后，Loon 配置里原来那几条本地 `DOMAIN-SUFFIX,baidu.com,DIRECT` 可以删，已经迁到 `personal.list`。

## 和现有订阅的关系

你现在还挂着 blackmatrix7 的大 Proxy 列表、国内 GEOIP。这套自建列表是**覆盖层**：自己的判断写这里，大名单继续垫底。
