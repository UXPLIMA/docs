---
title: SQLite (Easy)
order: 800
description: 'SQLite is the default storage backend. It needs zero setup: no database
  server, no credentials. Perfect for a single server.'
icon: database
---

---

## Enabling

It's already the default:

```yaml
storage:
  type: sqlite
```

A database file is created inside `plugins/uxmSkyblock/` on first start. Everything:
islands, members, roles, points, levels, bank, warps, bans, upgrades, border
colors, custom roles: is stored in it.

---

## When to Use SQLite

| Use SQLite if... |
|------------------|
| You run a **single** server |
| You want zero configuration |
| You don't need cross-server sync |

If you run **multiple backend servers** behind a proxy, you need shared storage
instead: switch to [MySQL](mysql.md).

---

## Backups

The database is a single file in `plugins/uxmSkyblock/`. To back it up, stop the
server (or ensure no writes) and copy that file somewhere safe.

<Callout type="warning" title="Can't share across servers">

SQLite is a local file: two servers can't safely use the same one. For a
network, use [MySQL](mysql.md) and the [proxy layer](../proxy/cross-server.md).

</Callout>
