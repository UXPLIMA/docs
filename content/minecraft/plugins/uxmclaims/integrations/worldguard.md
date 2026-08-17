---
title: WorldGuard
order: 803
description: Stopping claims from overlapping protected regions.
icon: shield-alert
---

```yaml
generalSettings:
  worldGuardSupport: true
```

## What it does

When a player tries to claim a chunk, the plugin asks WorldGuard whether that chunk overlaps a region.
If it does, the claim is refused with `errorNotClaimableArea` — *"Here is not claimable area."*

It is one check in one direction: WorldGuard regions block claims. uxmClaims does not create regions
and does not modify existing ones.

## What this is for

The usual setup is a spawn region, an event arena, a shop district and a warzone — areas the server
owns and players should not be able to fence off.

Two ways to protect them, and they compose:

| Approach | Use when |
|---|---|
| A WorldGuard region | The area is a shape inside an otherwise claimable world |
| `generalSettings.disabledWorlds` | The whole world should never be claimed |

`disabledWorlds` is cheaper — no region lookup at all — so use it for whole worlds and reserve regions
for shapes.

## When to turn it off

`worldGuardSupport: false` on a server where regions and claims are meant to coexist. A plot world
wrapped in one large region, for example, would otherwise be entirely unclaimable.

## Notes

- **Existing claims are not re-checked.** Drawing a region over land somebody already claimed does not
  remove their claim. The check happens at claim time only.
- **The check covers the chunk, not the block.** A region touching any part of a 16×16 chunk blocks
  that whole chunk.
- **`disabledWorlds` is case sensitive.** `World_Nether` and `world_nether` are different strings, and
  a typo there silently leaves the world claimable.
