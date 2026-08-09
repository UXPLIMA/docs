---
title: Core Concepts
order: 500
description: A quick tour of the ideas that make uxmSkyblock work. Once these click,
  the rest of the wiki is just detail.
---

---

## Islands

An **island** is a region in the island world owned by one player. It has:

- An **owner** and a list of **members** (the team)
- A **protected area** centered on the island (the size is set by the size upgrade)
- A **level** and **points**
- A **bank** balance, **warps**, **flags**, and a **border color**

Each island lives at its own grid cell; the gap between islands is
`world.island-distance`.

---

## The Island World

The world is generated one of two ways, set by `world.generator`:

| Generator | Result |
|-----------|--------|
| `void` | Classic empty uxmSkyblock — a void world; islands are pasted from a schematic |
| `normal` | Real, naturally-generated terrain; islands sit on the ground (used by Chunklock) |

Each island also owns a private **Nether** and **End** plot in their own void
worlds, reached through real portals. See
[nether & end](../configuration/config-yml.md#nether-end).

---

## Points & Levels

Every block has a **point value** in [block-values.yml](../configuration/block-values-yml.md):

- **Place** a block → its points are **added** to the island
- **Break** a block → its points are **removed**

When total points cross a threshold in [levels.yml](../configuration/levels-yml.md),
the island **levels up**. Levels gate upgrades and decide your spot on the
leaderboard. See [Points & Levels](../progression/points-levels.md).

---

## Roles & Permissions

Every team member has a **role**. A role is a bundle of **permissions** —
`BLOCK_PLACE`, `CONTAINER`, `INVITE`, `BANK`, and so on.

- **Built-in roles** (visitor, farmer, member, architect, moderator, owner) are
  defined in [roles.yml](../configuration/roles-yml.md).
- **Custom roles** can be created per-island at runtime by the owner.
- The **owner** always has every permission.

See [Roles & Permissions](../protection/roles-permissions.md).

---

## Flags

**Flags** are island-wide toggles that control what happens inside the protected
area: PvP, mob spawning, creeper/TNT explosions, fire spread, visitor interaction,
and whether visitors are allowed at all. Owners toggle them in `/is settings`.
See [Island Flags](../protection/flags.md).

---

## Upgrades

**Upgrades** improve an island over time. Each has tiers with a **level
requirement** and (optionally) a **money cost**:

- **team-limit** — max members
- **size** — protected area size
- **crop-growth** — chance to double crops
- **mob-drops** — drop multiplier
- **spawner-rates** — spawner speed
- **generator** — what the cobble generator produces

See [Upgrades](../progression/upgrades.md).

---

## Storage

Island data is saved to **SQLite** (default, zero setup) or **MySQL** (for
networks). Switch with `storage.type`. See [Database](../database/sqlite.md).

---

## The Proxy Layer

Optional. When enabled, islands stay in sync across **multiple backend servers**
behind Velocity/BungeeCord, using a shared MySQL database and Redis. Each island
physically lives on one backend, and `/is home` / `/is visit` route players to the
right server. See [Cross-Server Setup](../proxy/cross-server.md).

---

## Modules

uxmSkyblock loads add-on jars from `plugins/uxmSkyblock/modules/`. A module shares
the core's classloader, database, and proxy messaging, and can register its own
commands and listeners. See [Module System](../modules/overview.md) and
[Chunklock](../../chunklock/overview.md).
