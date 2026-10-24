---
title: config.conf (Globals)
order: 1150
description: Storage, locale, lookup, claims, links and every other setting in the root config file.
icon: file-cog
---

`plugins/uxmEssentials/config.conf` holds the **global** settings only: the database
connection, the default language, cross-server sync, native server links, the update
checker, and web-map markers. Everything that belongs to a feature lives in that
feature's own folder (see [Per-Module Config](per-module.md)), so this file stays
small.

## How to Edit

1. Open `plugins/uxmEssentials/config.conf` in a plain-text editor.
2. Change the values you need, keeping braces `{ }` and quotes balanced.
3. Save.
4. Reload: most of these are re-read on `/uxmess reload`; the `network` block is
   **restart-only** (noted below).

**HOCON, not YAML.** This file is HOCON. Indentation is cosmetic, but braces and quotes are not.
Comments start with `#`, keys are kebab-case, strings are quoted (`type =
"sqlite"`), numbers and booleans are bare (`port = 3306`, `enabled = true`).

## `storage`

Where uxmEssentials keeps its data. The default is an embedded SQLite file: zero
setup, no external service.

```hocon
storage {
  type = "sqlite"               # sqlite | mysql | mariadb | postgresql. Any other word stops the plugin
  file = "uxmessentials.db"     # SQLite only: the database file under this folder
  host = "localhost"
  port = 0                      # 0 is the database's own: 3306 for mysql, 5432 for postgresql
  database = "uxmessentials"
  username = "uxmessentials"
  password = ""
  pool-size = 8                 # network backends only; SQLite is fixed single-writer
  connection-timeout-ms = 5000
  table-prefix = "uxmessentials_"   # chosen before the first start
  local-only = false            # true keeps this one server on the SQLite file
}
```

#### type

**What it does:** Selects the database engine.

| Value | Effect |
|---|---|
| `sqlite` | Embedded file database (default). No server needed. |
| `mysql` | MySQL / MariaDB. |
| `mariadb` | MariaDB, the same driver as `mysql`. |
| `postgresql` | PostgreSQL. `postgres` is read the same. |

**Default:** `sqlite`. Choose a network backend only when you run more than one server
against one dataset (or want central backups). It is a topology decision, not a
capability one: every backend is a first-class, fully tested path.

<Callout type="info" title="Older files say backend and read-pool-size">
A `config.conf` written by an older version names these two keys `backend` and
`read-pool-size`. Both still work, and an update does not add `type` or `pool-size` beside them,
so your server keeps its database. When a file has both names, `type` and `pool-size` win.
</Callout>

#### file

**What it does:** The SQLite database file name, relative to the plugin folder. Ignored
by the network backends.

#### host / port / database / username / password

**What they do:** The connection details for a network backend. A `port` of `0`, or no
`port` at all, means the database's own: `3306` for MySQL and `5432` for PostgreSQL. These are ignored when `type = "sqlite"`.

#### table-prefix

**What it does:** Written in front of every table, so two plugins, or two copies of this one,
can share a database. Letters, digits and underscores, at most 15.
**Default:** `uxmessentials_` on a new install. A server that ran before the prefix existed
has no such line and keeps its tables under their bare names; an update never adds the line.
Choose it before the first start: a prefix written over an install that ran without one stops
the plugin with a message rather than start on an empty set of tables.

#### local-only

**What it does:** `true` keeps this one server on its SQLite file, whatever `type` says.
**Default:** `false`. It is for the server of a network that has no business in the shared
data, a creative world or a build server: the `storage` block stays the same on every server
and that one sets this line. Leave the cross-server bus off on it as well.

#### pool-size / connection-timeout-ms

**What they do:** `pool-size` sizes the read connection pool for network backends
(SQLite is fixed to a single writer). `connection-timeout-ms` is how long to wait for a
connection before giving up.

**Recommendation:** Leave SQLite on for a single server. See
[SQLite](../database/sqlite.md), [MySQL / MariaDB](../database/mysql.md), and
[PostgreSQL](../database/postgresql.md) for full setup.

## `messages`

The fallback language.

```hocon
messages {
  default-locale = "en"     # used when the client locale has no catalog and /lang is unset
}
```

#### default-locale

**What it does:** The language a player gets when their Minecraft client locale has no
matching catalog and they have not chosen one with `/lang`.

**Default:** `en`. Twelve catalogs ship: `en`, `tr`, `de`, `ru`, `es`, `pt`, `fr`, `pl`,
`uk`, `zh`, `ja` and `ko`. See [Messages & Languages](messages.md) to add or edit locales.

## `lookup`

How many known player names are held in memory so that a command can resolve a player
by name whatever the case it is typed in.

```hocon
lookup {
  name-index-size = 50000
}
```

#### name-index-size

**What it does:** Caps the in-memory index of "which account last joined under this
name". The most recently seen names are kept; a name that has fallen out of the window
still resolves when its case matches exactly.

**Default:** `50000`. An entry costs roughly 120 bytes, so the default is about 6 MB on
a server that has actually seen that many players. This matters most on offline-mode
(cracked) servers, where the server itself cannot resolve a name whose case differs.
See [Offline Mode](../getting-started/offline-mode.md).

## `skins`

Whether a player name may be resolved to the skin that account wears.

```hocon
skins {
  mojang-lookup = true
}
```

#### mojang-lookup

**What it does:** Allows the server to ask Mojang for the skin behind a name, which is how
an NPC dressed with `/npc skin name:<name>` and a tablist entry dressed with
`player:<name>` get their texture. The lookup goes to Mojang directly rather than through
the server's own profile completion, so it works identically on online-mode and
offline-mode servers, and the whole server shares one hour-long cache.

**Default:** `true`. Turn it off on a server with no outbound network: skins given as a raw
texture value still render, only lookups by name stop resolving. See
[Offline Mode](../getting-started/offline-mode.md).

## `network`

Cross-server sync. **Off by default**: a single server needs none of it and runs
purely local. Turn it on to keep homes, warps, economy, vaults and more in step across a
network of backends.

```hocon
network {
  enabled = false               # opt every backend into the shared bus
  server-id = "server-1"        # MUST be unique per backend
  bus-channel = "uxmessentials:bus_v1"   # must match the proxy broker, leave as-is
  heartbeat-seconds = 30        # how often this backend announces itself
  transport = "velocity"        # velocity | redis | both

  redis {
    host = "127.0.0.1"
    port = 6379
    password = ""               # leave empty to skip AUTH
    channel = "uxmessentials:bus"
    db = 0
  }
}
```

#### enabled

**What it does:** Opts this backend into the shared sync bus. Leave `false` on a single
server.

#### server-id

**What it does:** A unique id for this backend.

<Callout type="danger" title="Must be unique per backend">

Two backends sharing a `server-id` corrupt sync routing. Give every server its own.

</Callout>

#### transport

**What it does:** How sync frames travel between backends.

| Value | Effect |
|---|---|
| `velocity` | Plugin messages relayed through a Velocity proxy (no extra service). |
| `redis` | Redis pub/sub: no proxy needed; every backend shares one Redis. |
| `both` | Fan out over both carriers for a mixed network. |

The `redis { }` block applies when `transport` is `redis` or `both`.

**Restart-only.** The `network` block is read at startup, not on `/uxmess reload`. Restart the server
after changing it. Full walkthrough: [Velocity & Redis](../cross-server/overview.md).

## `server-links`

Native 1.21+ pause-menu server links. Each entry is either a built-in `type` or a custom
`label`, plus a `url`. An empty list leaves the client's links untouched.

```hocon
server-links = [
  # { type = "WEBSITE", url = "https://example.com" }
  # { label = "Discord", url = "https://discord.gg/xxxx" }
]
```

Built-in `type` values include `REPORT_BUG`, `SUPPORT`, `STATUS`, `FEEDBACK`,
`COMMUNITY`, `WEBSITE`, `FORUMS`, `NEWS`, `ANNOUNCEMENTS`, and `COMMUNITY_GUIDELINES`.
A malformed or empty entry is skipped with a warning.

## `update-check`

An optional update checker. **Off by default** because it makes an outbound network
call: opt in explicitly.

```hocon
update-check {
  enabled = false
  source-url = "https://api.github.com/repos/UXPLIMA/uxm-essentials/releases/latest"
  notify-ops-on-join = true     # tell operators on join when a newer version exists
  interval-hours = 12           # re-check cadence; 0 = check once on enable only
}
```

## `map-markers`

Web-map markers for Dynmap, squaremap and BlueMap. Renders warps and spawns as live markers
on whichever supported map plugin is installed; with none installed it does nothing. More
than one installed is fine: each of them gets the same markers.

```hocon
map-markers {
  enabled = true
  warps  = true
  spawns = true
  homes  = false                # per-player homes on a public map are opt-in
  layer-name = "uxmEssentials"
  warp-icon = "portal"          # dynmap icon id / squaremap icon key
  spawn-icon = "world"
  home-icon = "house"
  tooltip = "<name>"            # marker label; <name> expands to the warp/spawn name
}
```

#### enabled / warps / spawns / homes

**What they do:** `enabled` is the master switch; `warps`, `spawns` and `homes` toggle
which categories are published. `homes` defaults **off**: a public map of everyone's
homes is a privacy choice you opt into.

#### layer-name / warp-icon / spawn-icon / home-icon / tooltip

**What they do:** The map layer name, the icon keys used per category, and the marker
tooltip template. `<name>` in `tooltip` expands to the warp or spawn name.

**BlueMap ignores the icon keys.** BlueMap addresses marker icons by asset URL, while `*-icon` holds the short keys Dynmap
and squaremap use (`portal`, `house`). Those keys mean nothing to BlueMap, so they are
deliberately not passed through and its markers use BlueMap's own default icon. Passing
them would give every marker a broken image instead.

Related: [Per-Module Config](per-module.md), [SQLite (Default)](../database/sqlite.md), [Velocity & Redis](../cross-server/overview.md), [Config Overview](overview.md)
