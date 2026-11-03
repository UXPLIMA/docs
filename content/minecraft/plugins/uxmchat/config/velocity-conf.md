---
title: Velocity config
order: 518
description: The proxy plugin's config.conf and secrets.conf.
icon: server
---

`uxmChat-Velocity-<version>.jar` in the proxy's `plugins/` writes `plugins/uxmchat/`:

| File | Holds |
|---|---|
| `config.conf` | `network.mode`, `host`, `port`, `redis-uri` |
| `secrets.conf` | One secret per server, written the first time the proxy sees it; `direct` only. Never put it in git |

```hocon
network {
  mode = "direct"
  host = "127.0.0.1"
  port = 25590
  redis-uri = "redis://127.0.0.1:6379"
}
```

The keys are described in [A network](../features/network.md#the-proxys-configconf).

Related: [A network](../features/network.md)
