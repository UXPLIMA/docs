---
title: Upgrades
order: 650
description: 'Upgrades are how an island grows over time. Each upgrade has a series
  of tiers, and reaching a tier requires:'
---

1. A minimum **island level** (`required-level`)
2. A **money cost** (`required-money`, via Vault, only when economy is enabled)

Players buy them in the [Upgrades menu](../menus/upgrades.md) (`/is upgrade`).
Everything below is the **default** `upgrades.yml`; you can add or remove tiers
freely, see [upgrades.yml](../configuration/upgrades-yml.md).

---

## Team Limit

Maximum members on the island (including the owner).

| Tier | Members | Req. Level | Cost |
|------|---------|-----------|------|
| 1 | 4 | 0 | 0 |
| 2 | 6 | 5 | 5,000 |
| 3 | 8 | 12 | 15,000 |
| 4 | 12 | 25 | 40,000 |

---

## Island Size

The full island size in blocks across; the protected radius is half this.

| Tier | Size | Req. Level | Cost |
|------|------|-----------|------|
| 1 | 100 | 0 | 0 |
| 2 | 140 | 8 | 10,000 |
| 3 | 180 | 20 | 30,000 |
| 4 | 220 | 40 | 75,000 |

Buying this instantly refreshes the island border.

---

## Crop Double Chance

% chance to double a fully-grown crop when harvested.

| Tier | Chance | Req. Level | Cost |
|------|--------|-----------|------|
| 1 | 0% | 0 | 0 |
| 2 | 15% | 5 | 8,000 |
| 3 | 30% | 15 | 20,000 |
| 4 | 50% | 30 | 50,000 |

---

## Mob Drops

Multiplier applied to mob drops.

| Tier | Multiplier | Req. Level | Cost |
|------|-----------|-----------|------|
| 1 | ×1 | 0 | 0 |
| 2 | ×2 | 10 | 12,000 |
| 3 | ×3 | 25 | 35,000 |

---

## Spawner Rates

Delay multiplier: lower is faster.

| Tier | Speed | Req. Level | Cost |
|------|-------|-----------|------|
| 1 | ×1.0 (normal) | 0 | 0 |
| 2 | ×0.7 | 12 | 20,000 |
| 3 | ×0.4 (2.5× faster) | 30 | 60,000 |

---

## Generator

A weighted cobble generator, see the [Generators](generators.md) page for the
full tier-by-tier block tables, including the top-tier **obsidian** generator.

---

## Without an Economy

If Vault isn't installed (or `economy.enabled: false`), the **money cost is
ignored** and upgrades only check the **level** requirement. This makes the plugin
fully playable on servers with no economy at all.
