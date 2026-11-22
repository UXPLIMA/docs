---
title: WorldGuard
order: 1343
description: Keeps claims out of WorldGuard regions, with a region flag that allows or denies claiming per region.
icon: shield
---

The worldguard module stops players from claiming a chunk that overlaps a WorldGuard region, as 3.x did. It
also adds the `uxmclaims-claim` region flag, so you can open a single region to claims or close the whole
world with one flag on `__global__`.

Module `worldguard` · enabled by default · `modules/worldguard/config.yml` · needs WorldGuard

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.worldguard` | op | Claim inside WorldGuard regions, whatever their flag. The node is the value of `bypass-permission`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `deny-in-any-region` | `true` | Refuse a chunk that overlaps any region (3.x behaviour). `__global__` never counts as a region here. |
| `respect-flag` | `true` | Read the region flag. `allow` lets claims into a region even with `deny-in-any-region`; `deny` keeps them out even with it off. |
| `flag-name` | `uxmclaims-claim` | Name of the WorldGuard flag. Changing it needs a restart. |
| `ignored-regions` | `[]` | Region ids that never keep claims out, whatever their flag. |
| `bypass-permission` | `uxmclaims.admin.worldguard` | Players with this permission claim inside regions. Empty switches the bypass off. |

## How a chunk is checked

The module tests the whole chunk, from the bottom of the world to the top, against every region it overlaps
(3.x looked only at the corners at Y=64). Regions in `ignored-regions` and `__global__` are skipped, then:

| Region flag | Result |
|---|---|
| `deny` on any overlapping region | Refused, even when another overlapping region says `allow`. |
| `allow` | This region does not block. |
| not set | Blocks when `deny-in-any-region` is `true`. |

When no overlapping region blocks or allows, `deny` on `__global__` refuses the chunk. This closes a whole
world to claims while regions flagged `allow` stay open:

```
/rg flag __global__ -w world uxmclaims-claim deny
/rg flag town_square -w world uxmclaims-claim allow
```

A refused player sees `claim-denied` from the module's `lang/en/messages.yml`.

## Notes

- WorldGuard accepts new flags only while it loads. uxmClaims registers the flag early for that reason; if
  WorldGuard still started first, the log says so and only `deny-in-any-region` applies until the next restart.
- A flag of the same name that belongs to another plugin and is not an allow/deny flag is ignored, with a
  warning.
- The check covers claiming. [Random Teleport](rtp.md) does not avoid WorldGuard regions yet.
- This module has not been tried on a live WorldGuard server yet.

Related: [Lands](lands.md), [Integrations](../integrations.md)
