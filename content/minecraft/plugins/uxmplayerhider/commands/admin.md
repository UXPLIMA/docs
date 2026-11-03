---
title: Operator commands
order: 203
description: Every /uxmplayerhider subcommand, with what it does and the permission it needs.
icon: shield
---

Every operator command is under `/uxmplayerhider`, aliases `/uxmph` and `/hideadmin`.

| Command | Permission | What it does |
|---|---|---|
| `/uxmplayerhider reload` | `uxmplayerhider.admin.reload` | Reads `config.conf`, the messages and the theme again. See [Configuration](../config/index.md#reloading) |
| `/uxmplayerhider doctor` | `uxmplayerhider.admin.doctor` | Checks the storage and PlaceholderAPI, one line each |
| `/uxmplayerhider version` | `uxmplayerhider.admin.version` | Prints the version this server runs |
| `/uxmplayerhider` | `uxmplayerhider.admin.version` | Points to `/uxmplayerhider help` |

The language of another player is set under `/hide`: see [Player commands](player.md).

## doctor

```
How this plugin is
ok  storage: sqlite file playerhider.db, prefix uxmplayerhider_, schema v1
ok  placeholders: published
Everything answered.
```

| Check | ok | warn or fail |
|---|---|---|
| `storage` | The backend, the file or `host:port/database`, the prefix and the schema version | fail: the database did not answer; no choice is remembered between sessions |
| `placeholders` | PlaceholderAPI is there and the placeholders answer | warn: PlaceholderAPI is not on the server; everything else works |

The checks run off the main thread, since one of them opens a connection; the header shows at once and the lines
follow.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
