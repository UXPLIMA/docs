---
title: Ranks
order: 9190
description: A rankup ladder with requirements, costs, prestige and an optional automatic promotion scan.
---

Ranks are the server's progression ladder: a chain a player climbs with `/rankup`, an optional `/prestige` reset
at the top, and an optional scan that promotes eligible players by itself. The plugin tracks each player's rank
in its own database, so the ladder does not depend on a permission plugin, and a rank's actions can set a
LuckPerms group, run any command, or do nothing at all.

Module `ranks` · enabled by default · `modules/ranks/config.conf`, `ranks.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/prestige` | Reset to the first rank for a prestige level once you reach the top rank. | `uxmessentials.ranks.prestige` |
| `/ranks` | Set a player's rank directly. | `uxmessentials.ranks.admin` |
| `/rankup` | Advance to the next rank when you meet its requirements. | `uxmessentials.ranks.rankup` |
| `/setrank` | Set a player's rank directly. | `uxmessentials.ranks.admin` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.ranks` | op | Hot-reload / inspect the ranks module (rank ladders, prestige and rank-up costs). |
| `uxmessentials.ranks.admin` | op | /ranks setrank \<player> \<rank> to set a player's rank directly. |
| `uxmessentials.ranks.gui` | everyone | /ranks to open the ladder panel (config-gated; registered only when the GUI is enabled). |
| `uxmessentials.ranks.prestige` | everyone | /prestige to reset to the first rank for a prestige level once you reach the top rank. |
| `uxmessentials.ranks.rankup` | everyone | /rankup to advance to the next rank when you meet its requirements. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `gui.enabled` | `true` |  |
| `prestige.enabled` | `true` |  |
| `prestige.max-level` | `0` | The highest prestige level a player can reach. 0 means no cap (prestige forever). At the cap, /prestige is refused. |
| `prestige.cost` | `0` | How much is charged through the economy when a player prestiges (0 for a free prestige). |
| `prestige.requirements` | `[]` | What the player must satisfy to prestige, on top of being at the top rank. Same grammar as a rank's requirements (see ranks.conf): "money \<amount>", "playtime \<seconds>", "permission \<node>", etc. An unmet requirement blocks the prestige; an unknown type is simply not enforced. |
| `prestige.actions` | `[]` | What runs when a player prestiges, in order. Same grammar as a rank's actions (see ranks.conf): "console ...", "player ...", "message ...", "sound ...", and so on. Use \{player} for the player's name. |
| `prestige.reward-multiplier` | `1.0` | The reward multiplier granted per prestige level, applied linearly: a value of 1.0 grants no bonus, 1.5 gives a 1.5x multiplier at prestige 1, 2.0x at prestige 2, and so on. It is surfaced to the player on prestige (and exposed as a placeholder) so your rewards can scale with prestige level. |
| `autorank.enabled` | `false` |  |
| `autorank.interval-seconds` | `300` | How often, in seconds, the scan runs. Clamped to a minimum of 1. Keep it comfortably large: every scan re-checks every online player's next-rank requirements. 300 (five minutes) is a sensible default. |
| `autorank.charge-cost` | `true` | Whether an automatic promotion charges the rank's cost, exactly as /rankup would. Leave it true so autorank respects your ladder's prices; set it false to let eligible players climb for free (requirements still apply). |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_prestige%` | How many times the player has prestiged. |
| `%uxmessentials_rank%` | The player's current rank. |
| `%uxmessentials_rank_next%` | The rank above the player's, or max when they are at the top of the ladder. |
| `%uxmessentials_rank_next_cost%` | What the next rankup charges, or the dash at the top of the ladder. |
| `%uxmessentials_rank_position%` | Which rung of the ladder the player stands on, counting from one. |
| `%uxmessentials_rank_progress%` | How far up the ladder the player stands, as a whole percentage. |
| `%uxmessentials_rank_total%` | How many rungs the ladder holds. |
{/* /generated */}

## Notes

- **The module is enabled but inert until you author `ranks.conf`.** Each top-level section is one rank, the
  section name is its id, and `order` decides the sequence, lowest first.
- **A rank carries four things:** an `order`, a `display-name`, a `cost` and lists of `requirements` and
  `actions`. Requirements and actions share their grammar with prestige.
- **`/rankup` checks, then charges, then advances, then acts.** A failed requirement refuses with nothing
  charged, a short balance refuses with nothing moved, and the actions only run once the new rank is durable.
  With no economy provider, or a zero cost, the charge step is skipped.
- **Prestige needs a player at the very top.** It resets them to the first rank in exchange for a permanent
  prestige level, capped by `max-level` (0 means forever). Disabled, `/prestige` is never registered.
- **Autorank promotes one rank per scan.** It is off by default; switched on it reuses the `/rankup` pipeline on
  `interval-seconds`, so requirements still apply and `charge-cost` decides whether the cost does.

Related: [Economy](economy.md), [Kits](kits.md), [Communication](communication.md)
