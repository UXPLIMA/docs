---
title: Leaderboards
order: 1336
description: Rankings of lands, towns and nations, shown in a menu, in placeholders and on holograms.
icon: trophy
---

Leaderboards rank lands, towns and nations by chunks, members, bank balance, level and war results. Players
open a board with `/claim top`, `/town top` or `/nation top`; staff can place boards in the world as holograms.
Boards are recomputed on a timer and read from a cache, so they can be a few minutes old.

Module `leaderboards` · disabled by default · `modules/leaderboards/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim top [metric]` | Opens the land leaderboard. | `uxmclaims.command.leaderboards.use` |
| `/town top [metric]` | Opens the town leaderboard. Only while [Towns](towns.md) runs. | `uxmclaims.command.leaderboards.use` |
| `/nation top [metric]` | Opens the nation leaderboard. Only while [Nations](nations.md) runs. | `uxmclaims.command.leaderboards.use` |
| `/uxm top recompute` | Computes every board now. | `uxmclaims.admin.leaderboards` |
| `/uxm top hologram create <name> <land\|town\|nation> <metric> [rows]` | Places a board hologram two blocks above where you stand. | `uxmclaims.admin.leaderboards` |
| `/uxm top hologram delete <name>` | Removes a board hologram. | `uxmclaims.admin.leaderboards` |
| `/uxm top hologram list` | Lists the board holograms and where they stand. | `uxmclaims.admin.leaderboards` |

Without a metric a board opens on `default-metric` for its kind. Hologram names are 1 to 32 lowercase letters,
digits, `_` or `-`. Holograms need the [Holograms](holograms.md) module.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.leaderboards.use` | everyone | `/claim top`, `/town top`, `/nation top`. |
| `uxmclaims.admin.leaderboards` | op | `/uxm top recompute` and the hologram commands. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `recompute-minutes` | `5` | Minutes between two recomputations of every board. At least `1`. |
| `menu-size` | `45` | Rows the menu shows. |
| `skip-admin-lands` | `true` | Leaves lands owned by the server off the land boards. |
| `skip-zero` | `true` | Leaves holders with a value of 0 off the boards. |
| `default-metric.land` | `chunks` | The metric the land board opens with. |
| `default-metric.town` | `residents` | The metric the town board opens with. |
| `default-metric.nation` | `towns` | The metric the nation board opens with. |
| `hologram-rows` | `10` | Rows a new hologram shows when the create command names no count. |

## Metrics

| Kind | Metrics | Needs |
|---|---|---|
| land | `chunks`, `members` | |
| land | `balance` | [Bank](bank.md) |
| town | `residents`, `chunks` | [Towns](towns.md) |
| town | `balance`, `level` | [Bank](bank.md), [Levels](levels.md) |
| nation | `towns`, `residents`, `chunks` | [Nations](nations.md) |
| nation | `balance`, `level` | [Bank](bank.md), [Levels](levels.md) |
| every kind | `kd`, `war-kills`, `war-wins` | [War](war.md) |

`kd` is the kills of the holder's side divided by the kills against it, over all its wars.

## Placeholders

`<kind>` is `land`, `town` or `nation`; `<metric>` is one of the metrics above; `<n>` is a row number from 1.

| Placeholder | Renders |
|---|---|
| `%uxmclaims_top_<kind>_<metric>_<n>%` | The name at row n. `_<n>_name` does the same. |
| `%uxmclaims_top_<kind>_<metric>_<n>_value%` | The value at row n. |
| `%uxmclaims_top_<kind>_<metric>_<n>_position%` | The position at row n. |
| `%uxmclaims_top_<kind>_<metric>_position%` | The position of the viewer's own land, town or nation. `_rank` does the same. |
| `%uxmclaims_top_<kind>_<metric>_mine%` | The value of the viewer's own land, town or nation. |

A row that does not exist yet renders as empty text. Example: `%uxmclaims_top_town_balance_1_value%`.

## Notes

- The first computation runs about 20 seconds after start-up, so boards are empty right after a restart until then.
- The war boards rank lands, towns and nations by their wars, not players by their own kills.
- There is no sign board and no chat list form of `/claim top` yet; the command always opens the menu.
- Holograms are rewritten after each recomputation.

Related: [Holograms](holograms.md), [Towns](towns.md), [Nations](nations.md), [Bank](bank.md), [Levels](levels.md), [War](war.md)
