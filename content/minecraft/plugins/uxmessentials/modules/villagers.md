---
title: Villagers
order: 9310
description: Trades that never lock out, a restock timer, a trade editor, protection, pickup, follow and leash.
icon: user-round
---

The villagers module hands you the trading and settlement knobs a survival or economy server usually reaches for
several small plugins to get: trades that never lock out, a restock timer in place of the vanilla work-station
cycle, a staff trade editor, click-to-trade access, villager protection, pickup, follow and leashing. Each is an
independent switch in one file.

Module `villagers` · disabled by default · `modules/villagers/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/villager` | Manage the villager you are looking at (/villager manager, /villager protect, /villager follow). | `uxmessentials.villagers.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.villagers` | op | Hot-reload / inspect the villagers module (villager trade management). |
| `uxmessentials.villagers.bucket` | op | Sneak-right-click a villager to pick it up into a captured-villager item, and place it back later. |
| `uxmessentials.villagers.follow` | op | /villager follow: toggle whether the villager you are looking at pathfinds after you. |
| `uxmessentials.villagers.leash` | op | Right-click a villager with a lead to leash it, when leashing is enabled. |
| `uxmessentials.villagers.manager` | op | /villager manager: open and edit the trades of the villager you are looking at. |
| `uxmessentials.villagers.protect` | op | /villager protect: toggle whether the villager you are looking at is protected from death and despawn. |
| `uxmessentials.villagers.trade` | op | Open a villager's trade window directly on right-click, when click-to-trade is enabled. |
| `uxmessentials.villagers.use` | op | /villager: the root command's base node; its subcommands each gate further on their own node. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `infinite-trading.enabled` | `true` |  |
| `restock.enabled` | `true` |  |
| `restock.interval-seconds` | `600` |  |
| `instant-restock.enabled` | `false` |  |
| `disable-trades.enabled` | `false` |  |
| `trade-manager.enabled` | `true` |  |
| `click-to-trade.enabled` | `true` |  |
| `protect.enabled` | `true` |  |
| `protect.all` | `false` | Shield every villager, not only those marked with /villager protect. |
| `protect.from-zombies` | `true` | Cancel a zombie infecting the villager into a zombie villager. |
| `protect.from-lightning` | `true` | Cancel a lightning strike's damage and the villager-to-witch transform it triggers. |
| `protect.from-damage` | `true` | Cancel other lethal damage: suffocation, fire, drowning, a mob's or player's blow. |
| `protect.no-despawn` | `true` | Keep the villager loaded (mark it persistent) so it never despawns when no player is nearby. |
| `bucket.enabled` | `false` |  |
| `follow.enabled` | `true` |  |
| `follow.speed` | `1.0` |  |
| `follow.range` | `16` |  |
| `leash.enabled` | `false` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_villagers_following%` | How many villagers are walking after the player right now. |
| `%uxmessentials_villagers_has_follower%` | Whether any villager is following the player (yes/no). |
{/* /generated */}

## Notes

- **The module ships off because every feature in it rewrites villager trading,** and a server that wants
  vanilla villagers should get them. Switched on, the six tools most servers want are already armed.
- **Infinite trading resets a villager's recipe uses after each trade,** so no trade ever greys out. Turn it off
  to restore the vanilla limits.
- **Click-to-trade opens a villager the vanilla gate would refuse,** a professionless or already-busy one, for
  holders of the trade node. A villager disabled globally or by its own flag still never opens.
- **Protection is per villager unless you set `all = true`.** `/villager protect` marks one, and a protected
  villager survives zombies, lightning and damage and is kept loaded so it cannot despawn.
- **The trade editor is staff-only** and edits the live villager, so a fix takes effect without a restart.

Related: [Economy](economy.md), [Survival](survival.md), [NPCs](npc.md)
