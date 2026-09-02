# LoonRules

可直接用于 Loon 的分流规则集合。规则文件只写匹配条件，策略由 Loon 配置中的 `policy=` 指定；规则已按 Loon 语法整理并去重。

> 本项目只提供规则文件，不包含代理节点或订阅服务。

## 规则文件

| 文件 | 用途 | 建议策略 |
| --- | --- | --- |
| `rules/reject.list` | 广告、追踪与恶意请求 | `REJECT` |
| `rules/direct.list` | 国内服务、局域网及指定直连地址 | `DIRECT` |
| `rules/ai.list` | AI 服务 | `Available` 或指定策略组 |
| `rules/proxy.list` | 需要代理的服务 | `Available` 或指定策略组 |
| `subscribe.conf` | 四条远程规则的示例配置 | — |

## 接入 Loon

1. 打开 [`subscribe.conf`](https://raw.githubusercontent.com/Cocojcc/LoonRules/master/subscribe.conf)。
2. 将 `[Remote Rule]` 下的四行复制到 Loon 配置的同名区块。
3. 如果你的策略组不叫 `Available`，将对应的 `policy=Available` 改成实际名称。
4. 保存并重载配置。

```ini
https://raw.githubusercontent.com/Cocojcc/LoonRules/master/rules/reject.list, policy=REJECT, tag=MyReject, enabled=true
https://raw.githubusercontent.com/Cocojcc/LoonRules/master/rules/direct.list, policy=DIRECT, tag=MyDirect, enabled=true
https://raw.githubusercontent.com/Cocojcc/LoonRules/master/rules/ai.list, policy=Available, tag=MyAI, enabled=true
https://raw.githubusercontent.com/Cocojcc/LoonRules/master/rules/proxy.list, policy=Available, tag=MyProxy, enabled=true
```

## 优先级

Loon 按远程规则的顺序匹配，先命中先生效。默认顺序为 `reject → direct → ai → proxy`；新增更具体的规则时，应放在可能覆盖它的通用规则之前。

## 规则格式

```ini
DOMAIN-SUFFIX,example.com
IP-CIDR,1.1.1.1/32,no-resolve
GEOIP,CN
```
