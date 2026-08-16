---
title: Proxy Setup
order: 820
description: 'The proxy layer keeps islands in sync across multiple backend servers
  behind Velocity/BungeeCord. It''s optional and off by default: enable it only when you
  run a network.'
icon: network
---

---

## How It Works

- All backends share **one MySQL database** ([MySQL](../database/mysql.md) required).
- Each island physically lives on **one** backend: the server it was created on
  (tracked by a `server` column).
- `/is home` and `/is visit <player>` **route** the player to the right server when
  the island lives elsewhere, then teleport them in.
- Island changes (members, roles, flags, level, bank, warps) are published over
  **Redis** so the other servers refresh that island from the database.
- New islands are created on the **least-full** server in `create-servers`
  (balanced distribution).

```
        ┌─────────────┐
        │  Velocity   │
        └──────┬──────┘
     ┌─────────┼─────────┐
┌────┴───┐ ┌───┴────┐ ┌──┴─────┐
│skyblock│ │skyblock│ │ spawn  │
│  -1    │ │  -2    │ │  -1    │
└────┬───┘ └───┬────┘ └────────┘
     └────┬────┘
   ┌──────┴──────┐  ┌────────┐
   │   MySQL     │  │ Redis  │
   │ (islands)   │  │(pub/sub)│
   └─────────────┘  └────────┘
```

---

## Configuration

```yaml
proxy:
  enabled: true
  server-name: "skyblock-1"        # unique per backend!
  spawn-server: "spawn-1"
  create-servers:
    - "skyblock-1"
    - "skyblock-2"
  pending-teleport-seconds: 30
  debug: false
  redis:
    host: "localhost"
    port: 6379
    username: ""
    password: ""
    timeout-seconds: 2
    channel: "skyblock:proxy"
```

| Key | Meaning |
|-----|---------|
| `enabled` | Turn the proxy layer on |
| `server-name` | This server's name in your Velocity/BungeeCord config: **must differ on every backend** |
| `spawn-server` | The lobby/spawn server for return routing |
| `create-servers` | Servers where new islands may be created (balanced). Empty = create locally |
| `pending-teleport-seconds` | How long a pending cross-server teleport/create stays valid |
| `debug` | Print detailed routing logs |
| `redis.*` | Redis connection; the **channel** must match on all servers |

---

## Setup Checklist

1. **Velocity/BungeeCord** running, with each backend registered.
2. All backends use [**MySQL**](../database/mysql.md) with the **same** host/database.
3. A **Redis** server all backends can reach, same `channel`.
4. On each backend set `proxy.enabled: true` and a **unique** `server-name`
   matching the proxy config.
5. Set `create-servers` to the backends that should host new islands.
6. Migrate existing islands once:
   ```sql
   UPDATE islands SET server = 'skyblock-1' WHERE server IS NULL;
   ```

---

## Checking It Works

Run `/is proxy` (needs `skyblock.admin`) to see whether the proxy is enabled, this
server's name, the create-server list, and which server a given island lives on.

---

## Troubleshooting

| Symptom | Likely cause |
|---------|--------------|
| `/is visit` doesn't switch servers | Island's `server` column is NULL, or server names don't match the proxy |
| Changes don't appear on other servers | Redis is down or the `channel` differs between servers |
| New islands all land on one server | `create-servers` only lists one server |
| Player not teleported after switch | The pending record expired: raise `pending-teleport-seconds` |

Turn on `proxy.debug: true` to log the routing decisions.

---

## Module Sync

Modules like [Chunklock](../../chunklock/overview.md) reuse this same proxy layer;
they ride the same database connection and Redis channel, so add-on data stays in
sync across the network for free.
