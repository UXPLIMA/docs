---
title: Velocity & Redis
order: 1590
description: Cross-server sync keeps player data in step across a network of backend
  servers — so a home set on survival-1, a /pay made on survival-2, and a vault filled
  on the lobby all agree the moment a player switches servers. It is off by default
  (network.enabled = false); a single server needs none of it and runs purely local.
---

---

## How It Works

The shared database is the single source of truth. Sync is a lightweight
**cache-invalidation bus**: when one backend changes something, it publishes a small
notification — "this player's homes/balance/vault changed, drop your cached copy" — and the
other backends re-read the fresh row from the database. A message is never a full record written
blindly, so the economy's double-spend guard (an atomic, guarded `UPDATE` at the database) holds
across the whole cluster.

Two things make this work, and both are required:

1. A **shared networked database** — MySQL/MariaDB or PostgreSQL — that every backend points at.
   SQLite cannot be shared; see [MySQL / MariaDB](../database/mysql.md).
2. A **transport** to carry the notifications between backends: a Velocity proxy broker, a Redis
   bus, or both.

```
        ┌─────────────┐                 ┌──────────────┐
        │  Velocity   │   or  ────────▶  │    Redis     │
        │  (broker)   │                  │  (pub/sub)   │
        └──────┬──────┘                  └──────┬───────┘
     ┌─────────┼─────────┐             (backends publish/subscribe)
┌────┴───┐ ┌───┴────┐ ┌──┴─────┐
│survival│ │survival│ │ lobby  │  ── all read/write ──▶  ┌──────────────┐
│  -1    │ │  -2    │ │  -1    │                         │ shared MySQL │
└────────┘ └────────┘ └────────┘                         │  / Postgres  │
                                                          └──────────────┘
```

---

## What Syncs

The following stay consistent across every backend:

| Synced | |
|--------|--|
| Homes | Warps |
| Player warps | Economy balances |
| Vaults | Moderation (bans + mutes) |
| Holograms | NPCs |
| Vote party / counter | Messaging ignore lists |

Each backend also sends a presence heartbeat so `/uxmess doctor` can report how many peers are
online.

---

## The `network` Block

Cross-server settings live in the `network` block of `plugins/uxmEssentials/config.conf`. These
are **restart-only** — a `/uxmess reload` does not rewire the bus.

```hocon
network {
  enabled = false               # opt every backend into the shared bus
  server-id = "server-1"        # MUST be unique per backend
  bus-channel = "uxmessentials:bus_v1"   # plugin-messaging channel; must match the proxy broker
  heartbeat-seconds = 30        # how often this backend announces itself
  transport = "velocity"        # velocity | redis | both

  redis {                       # only used when transport is redis | both
    host = "127.0.0.1"
    port = 6379
    password = ""               # leave empty to skip AUTH
    channel = "uxmessentials:bus"   # every backend must use the same channel
    db = 0                      # logical database index
  }
}
```

| Key | Meaning |
|-----|---------|
| `enabled` | Turns the shared bus on |
| `server-id` | This backend's name on the bus — **must differ on every backend** |
| `bus-channel` | Plugin-messaging channel for the Velocity path; must match the proxy broker |
| `heartbeat-seconds` | How often this backend announces its presence (feeds the doctor peer count) |
| `transport` | How notifications travel: `velocity`, `redis`, or `both` |
| `redis.*` | Redis connection; the `channel` must be identical on every backend |

---

## Choosing a Transport

| Transport | You need | Best when |
|-----------|----------|-----------|
| `velocity` (default) | The `uxmessentials-velocity` jar on your Velocity proxy | You already run Velocity and want no extra service |
| `redis` | A Redis server and the `uxmEssentials-redis` companion jar on each backend | You want proxy-independent sync (works behind any proxy) |
| `both` | Both of the above | A mixed network fanning out over both carriers |

If the transport is unavailable, the plugin degrades to local-only with a single warning — the
individual server keeps working, it just does not sync.

---

## Setup Outline

First, for **any** transport: provision a shared MySQL/MariaDB or PostgreSQL database, point
every backend's [`storage`](../database/mysql.md) block at it, and start the backends one at a
time on first launch so they do not race the initial Flyway migration.

**Velocity path**

1. Drop the `uxmessentials-velocity` jar onto your Velocity proxy and the main `uxmEssentials`
   jar on every backend.
2. On each backend set `enabled = true`, `transport = "velocity"`, and a **unique** `server-id`.
3. Leave `bus-channel` as shipped (it must match the broker).
4. Restart the proxy, then the backends.

**Redis path**

1. Install the `uxmEssentials-redis` companion jar on every backend, and run a Redis server all
   backends can reach.
2. On each backend set `enabled = true`, `transport = "redis"`, a **unique** `server-id`, and
   point `network.redis` at the shared Redis with an identical `channel`.
3. No proxy is required. Restart the backends.

Verify either path with `/uxmess doctor`, which shows the active transport, its health, and the
number of peers seen.

<Callout type="warning" title="Every backend must match">

All backends must point at the **same shared database** and use **identical economy /
currency configuration**. A backend on its own SQLite file, or one with a different set of
currencies, will not sync correctly — balances and other data can diverge or be rejected.
Give every backend a **unique** `server-id`; two backends sharing one corrupts sync routing.

</Callout>

---

## Proxy-Side Command Control

The backend [`commandcontrol`](../features/commandcontrol.md) module gates the commands a
**backend** owns. It cannot touch the proxy's own commands: `/server`, `/glist`, `/send`,
`/find` and any proxy plugin's commands never reach a backend at all.

The Velocity jar carries its own command-control section for exactly that layer. It is
configured in the proxy's `config.conf`, mirrors the backend config shape, and reuses the
same rule engine, so the proxy and the backends decide allow, deny and hide identically.

```hocon
command-control {
  enabled = false          # opt in explicitly
  mode = blacklist

  plugin-hide {
    enabled = true
    hidden-commands = [ "server", "glist", "send", "find", "plugins", "pl",
                        "velocity", "ver", "about", "bungee", "greporter" ]
    deny-list-commands = true
  }

  commands { default = [] }
  use-unknown-command-message = true
  tab-completion { enabled = true }
  block-namespace-bypass = true

  command-spam {
    enabled = false
    max-per-window = 40
    window-seconds = 2
    action = block         # KICK | BLOCK | WARN
  }
}
```

Groups resolve through **LuckPerms-Velocity** when it is installed; without it, the
`default` list applies to everyone. Three nodes govern it:

| Node | Effect |
|---|---|
| `uxmessentials.commandcontrol.bypass` | See and run every proxy command |
| `uxmessentials.commandcontrol.viewproxycommands` | See the hidden proxy-native commands |
| `uxmessentials.commandcontrol.spam.bypass` | Exempt from the proxy spam guard |

<Callout type="note" title="Two layers, configured separately">

Turning this on at the proxy does **not** configure your backends, and vice versa. A
network that wants both hidden runs command-control in two places: the proxy section
here for proxy-native commands, and the `commandcontrol` module on each backend for
that backend's commands.

</Callout>

<Callout type="warning" title="`plugin-channel-hide` is not enforced at the proxy">

Velocity's API does not expose the client-facing channel-advertisement packets, so
that setting is read and validated but never applied, and a warning is logged if you
set it. Use the backend module's plugin-channel hider instead.

</Callout>

---

## Next Steps

- [MySQL / MariaDB Setup](../database/mysql.md) — the shared database every backend requires.
- [PostgreSQL Setup](../database/postgresql.md) — the alternative shared backend.
- [config.conf (Globals)](../config/global-config.md) — the `network` block in full.
- [Economy](../features/economy.md) — how balances stay consistent (and double-spend-safe) across the cluster.
