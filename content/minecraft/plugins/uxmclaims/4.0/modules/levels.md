---
title: Levels
order: 1327
description: Town and nation level ladders with requirements, a price and rewards that raise limits.
icon: trending-up
---

Towns and nations climb a ladder of levels written in `levels.yml`. Each level asks for residents, chunks
or other amounts, may cost money from the bank, and gives rewards such as a higher chunk limit, outposts or
lower upkeep. `/town level` and `/nation level` show the ladder; `/town level up` reaches the next step.

Module `levels` · disabled by default · `modules/levels/config.yml` · requires [Towns](towns.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/town level` | Opens your town's level and the levels above it. | `uxmclaims.command.levels.use` |
| `/town level up` | Reaches your town's next level. | `uxmclaims.command.levels.use` and the town permission `levels:level_up` |
| `/nation level` | Opens your nation's level and the levels above it. Only while [Nations](nations.md) runs. | `uxmclaims.command.levels.use` |
| `/nation level up` | Reaches your nation's next level. Only while Nations runs. | `uxmclaims.command.levels.use` and the nation permission `levels:level_up` |
| `/uxm levels set <town\|nation> <name> [level]` | Sets the level of a town or nation; `none` or no level removes it. | `uxmclaims.admin.levels` |
| `/uxm levels check` | Checks every town and nation against the requirements now. | `uxmclaims.admin.levels` |

The mayor of a town and the leader of a nation may always level up. In `/uxm levels set`, a name with
spaces is read as typed when it is a known town or nation; otherwise the last word is the level.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.levels.use` | everyone | `/town level`, `/nation level`. |
| `uxmclaims.admin.levels` | op | `/uxm levels`, and levelling up any town or nation. |

| Role permission | Given to | Grants |
|---|---|---|
| `levels:level_up` | Owner | Reaching the next level of the town or nation. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `mode` | `PURCHASE` | `PURCHASE`: a town or nation that meets the next level's requirements buys it from its bank and keeps it. Levels that cost nothing are reached on their own. `AUTOMATIC` (or `AUTO`): the level follows the requirements up and down. |
| `check-minutes` | `5` | Minutes between two checks of every town and nation. `AUTOMATIC` levels are worked out again and free `PURCHASE` levels are given. `0` turns the check off. |
| `announce` | `true` | Tells the online residents of a town, or the citizens of a nation, when it reaches a level. |
| `stack-rewards` | `false` | `true`: the rewards of every level up to the reached one add up. `false`: only the reached level's rewards count, so each level lists its full rewards. |

## Writing a ladder

`plugins/uxmClaims/modules/levels/levels.yml` has a `town` and a `nation` section, each listing levels
lowest first. The section name is the level's id; never rename it once in use. The shipped ladders are
Settlement, Village, Town and City for towns, and Federation, Kingdom and Empire for nations.

```yaml
town:
  village:
    display-name: Village
    cost: 1000
    requirements:
      residents: 4
      chunks: 16
    rewards:
      chunks: 32
      outposts: 1
      bank-cap: 10000
    limits: {}
    unlock: [rentals]
    commands: []
```

| Key | What it does |
|---|---|
| `display-name` | The name players see, as plain text. |
| `cost` | Taken from the town or nation bank in `PURCHASE` mode. |
| `requirements` | At least these amounts: `residents`, `chunks`, `balance`, `age-days` (days since founding), `outposts` (towns), `towns` (nations), and ids other plugins register. |
| `rewards.chunks` | Added to the town chunk limit. |
| `rewards.outposts` | Added to the outpost limit. |
| `rewards.bank-cap` | Added to the bank's cap. |
| `rewards.upkeep-percent` | The share of the daily upkeep paid: `100` is full, `80` is 20% off. |
| `rewards.bonus-chunks-per-town` | Nations: chunks every member town gets. |
| `rewards.war-shield` | Hours after reaching the level in which no war can be declared on the holder. `PURCHASE` mode only. |
| `limits` | Added to any limit a module checks per town or nation, by limit name. |
| `unlock` | Names other modules and plugins ask about, such as `vault`, `rentals`, `public_spawn`. |
| `commands` | Console commands run once when the level is reached. `%holder%` is the id, `%name%` the name, `%level%` the level id, `%head%` the mayor or leader. |

Climbing several levels at once runs the commands of each level passed.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_levels_town%` | The display name of the player's town level; `%uxmclaims_levels_town_name%` is the same. |
| `%uxmclaims_levels_town_id%` | The id of that level. |
| `%uxmclaims_levels_town_rank%` | Its position on the ladder, `0` for none. |
| `%uxmclaims_levels_town_next%` | The display name of the next level. |
| `%uxmclaims_levels_nation%` | The same for the player's nation, also as `_name`, `_id`, `_rank` and `_next`. |

They are empty when the player has no town or nation, or it has no level yet.

## Notes

- **Buying needs the bank.** In `PURCHASE` mode a level with a cost is refused while [Bank](bank.md) does
  not run for that holder.
- **`AUTOMATIC` goes down too.** A town that loses residents or chunks drops a level at the next check,
  and loses that level's rewards.
- **Not every reward works yet.** There is no playtime requirement, no title or prefix reward, and no
  crop, spawner, mob or effect rewards. Entries in `unlock` are kept, but no module checks them yet.
- **Personal lands have no levels yet.** Only towns and nations climb a ladder, so the module stays off
  on a server without towns.

Related: [Towns](towns.md), [Nations](nations.md), [Bank](bank.md), [Taxes](taxes.md), [War](war.md)
