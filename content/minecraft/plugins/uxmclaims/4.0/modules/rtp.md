---
title: Random Teleport
order: 1334
description: Sends a player to a random safe spot in the wilderness, away from every land.
icon: shuffle
---

`/rtp` picks a random spot inside a ring around the world's centre, keeps clear of every land and the buffer
around it, and lands the player on safe ground. A cooldown, a price and a warm-up apply, and a failed search
gives the money back.

Module `rtp` · disabled by default · `modules/rtp/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/rtp [world]`, `/claim rtp [world]` | Teleports you to a random wilderness spot, in your own world or the one you name. | `uxmclaims.command.rtp` (naming a world also needs `uxmclaims.command.rtp.world`) |
| `/uxm rtp <player> [world]` | Sends an online player to a random spot, with no cooldown, price or warm-up. | `uxmclaims.admin.rtp` |

Without a world name, `/rtp` uses your current world when it has random teleports on, else `default-world`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.rtp` | everyone | `/rtp` in your own world (or the default world). |
| `uxmclaims.command.rtp.world` | op | Naming another world: `/rtp <world>`. |
| `uxmclaims.bypass.rtp-cooldown` | op | No cooldown between random teleports. |
| `uxmclaims.admin.rtp` | op | `/uxm rtp <player> [world]`. |

| Cost key | Default | What it pays for |
|---|---|---|
| `rtp:teleport` | `0` | One random teleport. Set the price in `modules/economy/costs.yml`; needs the [Economy](economy.md) module. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `cooldown` | `5m` | Time between two random teleports of one player. |
| `warmup-seconds` | `3` | Seconds to stand still before the teleport; `0` for none. |
| `cancel-on-move` | `true` | Moving a block during the warm-up cancels the teleport. |
| `max-attempts` | `12` | How many spots to try before giving up. |
| `land-buffer-chunks` | `2` | Chunks kept free around every land. |
| `avoid-liquids` | `true` | Never land on water or lava. |
| `biome-blacklist` | oceans and rivers | Biomes never landed in, by name (`ocean`, `river`, ...). |
| `default-world` | `"world"` | The world `/rtp` uses when your own world has random teleports off; empty for none. |
| `worlds.<name>.enabled` | `world`: `true`, `world_nether`: `false` | Random teleports in that world. A world not listed has them off. |
| `worlds.<name>.min-radius` | `300` | Inner edge of the ring, in blocks from the centre. |
| `worlds.<name>.max-radius` | `5000` | Outer edge of the ring. |
| `worlds.<name>.use-world-spawn` | `true` | The centre is the world spawn. `false` uses `center-x` and `center-z`. |
| `worlds.<name>.center-x`, `center-z` | `0` | The centre when `use-world-spawn` is off. |

## Notes

- A player in combat (with the [Combat](combat.md) module) or in jail (with the [Jails](jails.md) module) cannot use `/rtp`.
- The price is taken after the warm-up. If no safe spot is found within `max-attempts`, or the teleport fails, the price is paid back.
- Cooldowns are kept in memory, so a restart clears them.
- There is one cooldown and one price for every world, and no WorldGuard region avoidance yet.
- On Spigot the candidate chunks load on the main thread; Paper and Folia load them without blocking. In the Nether the search starts below y 120.

Related: [Lands](lands.md), [Economy](economy.md), [Combat](combat.md), [Jails](jails.md)
