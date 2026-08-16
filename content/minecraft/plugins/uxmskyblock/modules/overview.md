---
title: Module System
order: 850
description: uxmSkyblock has its own module system, so you can bolt new mechanics
  onto islands without forking the plugin or shipping a second standalone plugin.
icon: book-open
---

---

## What a Module Is

A module is a small jar you drop into:

```
plugins/uxmSkyblock/modules/
```

It loads **on top of** the core and gets full access to the island API: islands,
members, roles, points, levels, upgrades, the economy hook, storage, and the proxy
layer. It is **not** a separate Bukkit plugin: the core loads each module jar with
its own classloader (parent = the core plugin's classloader).

---

## What Modules Get for Free

Because modules share the core's classloader, they also share its:

- **Database connection**: store data in the same SQLite/MySQL database
- **Proxy messaging**: stay in sync across a multi-backend network using the same
  Redis channel the core uses
- **Managers**: economy, block values, islands, roles, and more

That means an add-on can persist and sync its data **without reinventing storage or
cross-server messaging**.

---

## Anatomy of a Module

Each module jar carries a `module.yml` at its root:

```yaml
name: Chunklock
main: net.cengiz1.chunklock.ChunklockModule
version: 1.0.0
```

…and a main class implementing `UxmSkyblockModule` with three hooks:

| Hook | When | Use it to |
|------|------|-----------|
| `onLoad(ModuleContext)` | At load | Grab what you need from the core API |
| `onEnable()` | After load | Register listeners, commands, tasks |
| `onDisable()` | On reload/shutdown | Tear down cleanly |

The `ModuleContext` gives access to the core (`getCorePlugin()`), lets you register
listeners and commands (auto-removed on disable), and provides the module's own
`config.yml` and data folder.

See [Writing a Module](../developer/modules.md) for the developer details.

---

## Module-Only Servers

To run a server that hosts **only** a module (for example a pure Chunklock server),
set `island.enabled: false` in the core config. The `/island` command and island
world stay off, but the core managers: economy, block values, storage, proxy, and
the module loader: keep running so the module has everything it needs.

---

## The First Module: Chunklock

**[Chunklock](../../chunklock/overview.md)** is the reference module. It turns the
world into a shared grid where each island starts boxed in, and players expand
their territory chunk by chunk by dropping the items the next chunk costs. It reuses
the core island system end to end: membership, roles, bans, warps, visiting, and
upgrades all work exactly as on a normal island.
