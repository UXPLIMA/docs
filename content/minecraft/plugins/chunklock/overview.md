---
title: Overview
order: 1660
description: A shared chunk grid where every island starts boxed in and unlocks outward.
icon: book-open
---

## What Is Chunklock?

Chunklock is the first **module** built on [uxmSkyblock](../uxmskyblock/getting-started/welcome.md).
It turns the world into a shared grid where every island starts **boxed in**, and
players expand their territory **one chunk at a time** by dropping the items each
chunk costs.

It is **not** a standalone plugin: it's a small jar you drop into
`plugins/uxmSkyblock/modules/`, and it reuses the entire core island system:
membership, roles, bans, warps, visiting, and upgrades all work exactly as they do
on a normal uxmSkyblock island.

---

## The Core Idea

1. A player runs `/ada oluştur` (`/is create`) and gets a uxmSkyblock island on
   **real terrain**.
2. Only a small area around the center starts **open**. The neighbouring chunks are
   **locked**.
3. To unlock a chunk, the player **looks at it and drops (Q)** the item it
   requires. Drops accumulate until the cost is met, then the chunk opens.
4. The cost scales by **ring**: chunks farther from the center cost rarer items.

---

## Two Layers

Chunklock keeps two concerns separate:

| Layer | What it does |
|-------|--------------|
| **Chunk lock** (always on) | Movement into a locked chunk is blocked at **every height**, exactly on the edge of the open shape. A visible **barrier** wall marks the edge. |
| **Island-upgrade border** (optional) | A square WorldBorder showing the island's protection size, growing with the size upgrade. Can be disabled. |

The lock is enforced by **movement blocking**, not the barrier, so it can't be
jumped over the top like a plain barrier wall, and unlike a square worldborder it
won't accidentally open the opposite side.

---

## Everything Else Comes From the Core

Because Chunklock rides on uxmSkyblock islands, you **don't** configure teams,
roles, bans, warps, or upgrades in Chunklock: those all live in the core. Chunklock
only adds the **per-chunk lock** layer on top.

---

## Where to Start

- [🔧 Installation](installation.md): drop in the module, set two core options
- [🧩 How It Works](how-it-works.md): rings, drops, barriers, borders
- [📟 Commands](commands.md): the `/chunklock` info commands
- [⚙️ Configuration](configuration.md): `config.yml` reference
