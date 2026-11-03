---
title: Networks
order: 411
description: Servers sharing claims, Redis, cross server teleports and webhooks.
icon: network
---

Servers that use the same MySQL, MariaDB or PostgreSQL database share their claims.

| Key | Default | Meaning |
|---|---|---|
| `network.server-name` | `survival` | This server's name; what a claim records as its home and what the proxy calls it. Every server needs a different one |
| `network.redis` | `""` | A Redis URL so a change reaches the others at once; empty polls every `storage.sync-seconds` (2) |
| `network.cross-server-teleport` | true | `/claim tp` to a claim on another server sends the player there through the proxy |

A Redis URL with a password belongs in an environment variable: `"${CLAIMS_REDIS}"`.

## Webhooks

```hocon
integrations {
  webhook-url    = ""
  webhook-events = ["claim", "unclaim", "transfer", "sale"]
}
```

A Discord webhook told about those events. Put the URL in an environment variable, `"${CLAIMS_WEBHOOK}"`; empty turns
it off.

Related: [Database](../database/overview.md)
