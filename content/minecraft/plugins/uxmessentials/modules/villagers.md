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
| `infinite-trading.enabled` | `true` | Infinite trading: a villager's trades never lock out from use. |
| `restock.enabled` | `true` | Restock timer: instead of relying on the vanilla work-station cycle, a scheduled sweep restocks a villager's trades once its last restock is older than interval-seconds. |
| `restock.interval-seconds` | `600` | Restock timer: instead of relying on the vanilla work-station cycle, a scheduled sweep restocks a villager's trades once its last restock is older than interval-seconds. |
| `instant-restock.enabled` | `false` | Instant restock: the trade a player just used restocks immediately, with no cooldown. |
| `disable-trades.enabled` | `false` | Disable trades: with enabled = true, every villager refuses to open its trade GUI (the right-click is cancelled and the player is told). |
| `trade-manager.enabled` | `true` | Trade manager: with enabled = true, staff holding uxmessentials.villagers.manager run /villager manager to open the villager they are looking at (or the nearest within reach) and edit its trades: change a recipe's buy/sell items and amounts, add a recipe, remove one, or toggle that villager's disable flag. |
| `click-to-trade.enabled` | `true` | Click-to-trade: with enabled = true, a player holding uxmessentials.villagers.trade who right-clicks a villager opens its trade window directly, even where the vanilla gate would refuse it (a professionless villager, one already busy with another trader). |
| `protect.enabled` | `true` | Protection ("villager saver"): with enabled = true, a protected villager never dies to the deaths a settlement loses villagers to and is kept loaded so it never despawns. |
| `protect.all` | `false` | Shield every villager, not only those marked with /villager protect. |
| `protect.from-zombies` | `true` | Cancel a zombie infecting the villager into a zombie villager. |
| `protect.from-lightning` | `true` | Cancel a lightning strike's damage and the villager-to-witch transform it triggers. |
| `protect.from-damage` | `true` | Cancel other lethal damage: suffocation, fire, drowning, a mob's or player's blow. |
| `protect.no-despawn` | `true` | Keep the villager loaded (mark it persistent) so it never despawns when no player is nearby. |
| `bucket.enabled` | `false` | Villager-in-a-bucket: with enabled = true, a player holding uxmessentials.villagers.bucket sneak-right-clicks a villager to pick it up into a captured-villager item that stores the villager's profession, type, level, and trades. |
| `follow.enabled` | `true` | Follow: with enabled = true, a player holding uxmessentials.villagers.follow runs /villager follow to make the villager they are looking at (or the nearest within reach) pathfind after them. |
| `follow.speed` | `1.0` | Follow: with enabled = true, a player holding uxmessentials.villagers.follow runs /villager follow to make the villager they are looking at (or the nearest within reach) pathfind after them. |
| `follow.range` | `16` | Follow: with enabled = true, a player holding uxmessentials.villagers.follow runs /villager follow to make the villager they are looking at (or the nearest within reach) pathfind after them. |
| `leash.enabled` | `false` | Leash: with enabled = true, a player holding uxmessentials.villagers.leash right-clicks a villager with a lead in hand to leash it; something vanilla never allows. |
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
