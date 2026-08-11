---
title: Installation
order: 900
---

## Requirements

Before you start, make sure you have:

| Requirement | Minimum |
|-------------|---------|
| **Server** | Paper **26.1.2 or newer** (Folia also supported) |
| **Minecraft API** | 1.21 |
| **Java** | Java 25 |

<Callout type="warning" title="Paper 26.1.2 / Java 25 only">

uxmEssentials is built for **modern Paper on Java 25**. It is **Folia-ready**: the whole plugin schedules work through Folia-safe schedulers, so it runs
on both Paper and Folia. It is not built for older Bukkit/Spigot versions.

</Callout>

---

## Step 1: Download

Download the latest **`uxmEssentials`** jar (the main, all-in-one plugin).

This one jar is everything you need for a single server. The extra companion jars
below are **optional** and only matter for networks; you can ignore them to start.

---

## Step 2: Drop It In

Put the jar in your server's `plugins` folder:

```
your-server/
├── plugins/
│   ├── uxmEssentials.jar   ← put it here
│   └── (other plugins)
├── server.properties
└── paper.jar
```

---

## Step 3: Start the Server

Start (or restart) the server once. On first launch the plugin creates its data
folder, sets up its **embedded SQLite database**, and writes out every config file.

```
[uxmEssentials] Enabling uxmEssentials...
[uxmEssentials] Database migrations applied
[uxmEssentials] 34 of 34 feature modules enabled
[uxmEssentials] uxmEssentials enabled!
```

<Callout type="tip" title="No database setup needed">

Out of the box, uxmEssentials stores everything in a local **SQLite** file
(`plugins/uxmEssentials/data/`). There is nothing to install and nothing to
configure; it just works. You only need MySQL/MariaDB or PostgreSQL if you
run a multi-server network. See [Database](../database/sqlite.md).

</Callout>

---

## Step 4: Check the Files

After the first start, uxmEssentials generates its config tree:

```
plugins/
└── uxmEssentials/
    ├── config.conf        ← global settings (storage, messages, network...)
    ├── modules/           ← one folder per module, each with its own config.conf
    ├── messages/          ← player-facing text per language (MiniMessage)
    ├── commands/          ← rename, alias or disable any command
    ├── menus/             ← your own custom GUI menus
    └── data/              ← the SQLite database lives here
```

Everything is **HOCON `.conf`** (not YAML). Your edits survive restarts and
updates: new keys from an update are added, your values are kept, and deleting a
key falls back to its default. Read more in [Core Concepts](concepts.md).

---

## Step 5: Verify It Works

Log in as an operator and run:

| Command | What it tells you |
|---------|-------------------|
| `/uxmess status` | Lists all 34 modules and whether each is enabled |
| `/uxmess doctor` | Runs health checks (database, economy provider, soft-depends, threading) |

The admin root is `/uxmess`: aliases `/uxmessentials` and `/uxe` do the same thing.

If `/uxmess doctor` comes back clean, you're ready to go.

---

## Optional Companion Jars (Advanced)

These are for **networks and integrations** and are entirely optional. Skip them on
a single server.

| Jar | Install on | What it adds |
|-----|-----------|--------------|
| `uxmessentials-velocity` | Your Velocity proxy | Cross-server sync of homes, warps, economy and more |
| `uxmEssentials-redis` | Backend servers | Redis-based cross-server sync (an alternative to the Velocity path) |
| `uxmessentials-discord` | A backend server | A Discord bridge for audit/economy notifications and `/link` |

<Callout type="info" title="Cross-server is off by default">

Even with a companion jar installed, cross-server sync stays disabled until you
turn it on in `config.conf`. See [Velocity & Redis](../cross-server/overview.md).

</Callout>

---

## That's It! 🎉

The plugin is running with sensible defaults and all modules enabled. Players can
start using `/home`, `/warp`, `/balance`, `/kit`, `/tpa` and more right away.

---

## Common Problems

### "Plugin failed to load"

**Cause:** Your Java version is too old.
**Fix:** Install **Java 25**. Check with `java -version`.

### A feature isn't there

**Cause:** Its module may be disabled.
**Fix:** Run `/uxmess status`, then enable the module and reload it. See
[Modules & Reloading](../modules/overview.md).

---

## Next Steps

- [🧠 Core Concepts](concepts.md): the ideas that make everything click
- [🧩 The Module System](../modules/overview.md): turn features on and off
- [📟 Commands Overview](../commands/overview.md): what you can type in-game
- [📦 Migrating from EssentialsX](migration.md): bring your old data across
- [💾 Database](../database/sqlite.md): SQLite by default, MySQL/PostgreSQL for networks
