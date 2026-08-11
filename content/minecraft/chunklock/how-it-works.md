---
title: How It Works
order: 1680
description: 'This page explains the mechanics: how chunks lock, how players unlock
  them, and how the two boundary layers behave.'
---

---

## Starting Out

When an island is created, only a small area around the **center chunk** is open.
The `start.radius-chunks` option decides how much:

| `radius-chunks` | Open area at the start |
|-----------------|------------------------|
| `0` | Just the center chunk (all 4 sides locked) |
| `1` | A 3×3 block of chunks |

Everything beyond that is **locked**.

---

## Rings

A chunk's distance from the island center is its **ring** (Chebyshev distance):

```
2 2 2 2 2
2 1 1 1 2
2 1 0 1 2     0 = center
2 1 1 1 2
2 2 2 2 2
```

The **cost** to unlock a chunk depends on its ring: farther rings cost rarer items.

---

## Unlocking a Chunk

To open a locked chunk:

1. **Look at** the locked chunk (you'll see a hologram telling you what it costs).
2. **Drop (Q)** the required item while looking at it.
3. Dropped items **accumulate** until the cost is satisfied.
4. When the cost is met, the chunk **unlocks**: the barrier wall comes down and a
   sound plays.

The cost is read from the **first band** in `unlock.items` whose `max-ring` is at
least the chunk's ring:

```yaml
unlock:
  items:
    - max-ring: 2
      require: [ DIRT:5 ]
    - max-ring: 4
      require: [ COBBLESTONE:16, OAK_LOG:8 ]
    - max-ring: 8
      require: [ IRON_INGOT:8, COAL:16 ]
    - max-ring: 16
      require: [ DIAMOND:4, GOLD_INGOT:8 ]
    - max-ring: 999999
      require: [ NETHERITE_INGOT:1, DIAMOND_BLOCK:2 ]
```

So chunks near the center cost dirt; the outer frontier costs netherite.

---

## The Two Boundary Layers

Chunklock deliberately splits confinement and the upgrade border into **separate**
systems.

### 1. Chunk Lock (always on)

Entering a locked chunk is stopped by **movement blocking**:

- Works at **every height**: you can't jump over it from Y 200 like a barrier.
- Follows the **exact shape** of your open area, so opening one direction never
  opens the opposite side by accident.
- Bypass with the `chunklock.bypass` permission (for staff).

A **barrier wall** is built on the edge of a locked chunk as the *visible* marker,
but the real stop is the movement blocking, not the barrier.

<Callout type="info" title="Terrain is preserved">

Only **passable** blocks (air, grass, etc.) on the frontier become barrier, so
natural terrain isn't destroyed. When a chunk unlocks, those barriers turn back
to air (self-healing).

</Callout>

### 2. Island-Upgrade Border (optional)

A square **WorldBorder** showing the island's protection size, which grows with the
uxmSkyblock **size upgrade**. Purely informative: toggle it with
`island-border.enabled`.

---

## Holograms

Each locked frontier chunk shows a floating marker telling the player what to drop.
It's configurable (title, requirement line, footer) under `hologram:` in the config.

---

## What Comes From the Core

Membership, roles, bans, warps, visiting, the bank, and upgrades are **all
uxmSkyblock features**: Chunklock doesn't reimplement them. A Chunklock island is
a normal core island with a lock layer on top, so everything in the
[uxmSkyblock wiki](../uxmskyblock/getting-started/welcome.md) applies.
