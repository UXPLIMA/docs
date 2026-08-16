---
title: Welcome
order: 470
icon: door-open
---

## What Is uxmSkyblock?

uxmSkyblock is a complete, **config-first** uxmSkyblock plugin for Paper/Spigot **1.21.4**.
It gives every player their own island floating in the void (or sitting on real
terrain), a team to build it with, levels to grind, upgrades to buy, and a
leaderboard to climb.

Almost everything a player ever sees: **commands, messages, menus, roles,
upgrades, block values, levels**: lives in YAML files. That means you can rename,
re-theme, or fully translate the plugin without touching a single line of code.

---

## What Can Players Do?

| Feature | What it means |
|---------|---------------|
| 🏝️ **Their own island** | `/is create` drops a fresh island for the player to build on |
| 👥 **Real teams** | Invite friends, give them roles, kick or ban them |
| 🎭 **Custom roles** | Owners create their own ranks and hand out exact permissions |
| 📈 **Levels & points** | Placing valuable blocks raises the island's level |
| ⬆️ **Upgrades** | Bigger teams, bigger islands, better generators, faster spawners |
| ⛏️ **Custom generators** | Weighted cobble generators that can roll ores or even obsidian |
| 🏦 **Island bank** | A shared balance the whole team can deposit into |
| 🌀 **Named warps** | Public warps reachable by name or through a menu |
| 🏆 **Leaderboard** | `/is top`, plus a floating hologram that follows you |
| 🛡️ **Protection flags** | PvP, mob spawning, explosions, fire, visitor access |

---

## Single Server or a Whole Network

uxmSkyblock runs perfectly on **one server** with zero setup (SQLite, no proxy).

When you outgrow that, it scales out: an optional **proxy layer** keeps islands in
sync across multiple backend servers behind Velocity/BungeeCord using a shared
MySQL database and Redis. See [Cross-Server Setup](../proxy/cross-server.md).

---

## Built to Be Extended

uxmSkyblock ships with a lightweight **module loader**. Drop an add-on jar into
`plugins/uxmSkyblock/modules/` and it gets full access to the island API: no forking,
no second standalone plugin.

The first module built on this is **[Chunklock](../../chunklock/overview.md)**, which
turns the world into a shared grid where players expand their island chunk by chunk.

---

## Who Is This Guide For?

| Person | What they'll learn |
|--------|--------------------|
| **Server Owners** | Install, configure, and tune everything in YAML |
| **Server Admins** | Manage islands, players, and troubleshoot |
| **Players** | Create an island, build a team, level up |
| **Developers** | Use the API and write modules |

---

## Where Should I Start?

- [🔧 Installation](installation.md): get the plugin running in 2 minutes
- [🏝️ Your First Island](first-island.md): create and grow an island step by step
- [🧠 Core Concepts](concepts.md): the ideas behind islands, points, and roles
