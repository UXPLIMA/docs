---
title: Types and levels
order: 401
description: Every key of a type file and of a level, requirements and rewards.
icon: file-text
---

One type per file under `types/`; the file name is the id. Copy a file, change the numbers and the name, and the server
has one more type on the next reload. `types/collector.conf` is commented in full.

| Key | Meaning |
|---|---|
| `job` | `miner`, `lumberjack`, `farmer`, `fisher`, `slayer`, `collector`, `feeder`, `seller`, `spawner` or `healer`; a job another plugin registered is written quoted, `"their-plugin:harvester"` |
| `icon` | The material the menus draw |
| `action-interval` | Ticks between two actions, before fuel takes its share off |
| `display-name` | A line of the messages (`"@content.types.miner.display-name"`), a block per language, or one name |
| `targets` | What it acts on; empty means everything not blacklisted |
| `blacklist` | What it never acts on |
| `experience` | Optional: `enabled`, `per-action`, `per-material` over the server's block, key by key |
| `levels` | The ladder |
| `loot` | Fisher only: the sea, by weight |
| `effects` | Healer only: the action lines it runs |

Both lists are read by whatever the job acts on: a miner names blocks, a slayer mob types, a fisher loot lines, a
collector what it picks up, a seller what it refuses to sell.

The set of jobs is closed: a job this jar does not have fails at load with the name you wrote. A namespaced job nobody
registered still loads: its minions keep their level, stock and money, say so on their panel, and work again when the
plugin comes back.

## A level

| Key | Meaning |
|---|---|
| `level` | From 1 upward with no gap |
| `action-interval` | This level's interval, in ticks |
| `range` | How far the work reaches, in blocks |
| `storage` | Slots in the internal container |
| `smelt` | Smelt the output before storing it |
| `sell-multiplier` | What a sale of this minion's output is multiplied by |
| `tax` | The fraction the server keeps of a sale, below one |
| `held-item` | What the minion is drawn holding |
| `armour` | The suit colour: `{ red, green, blue }`, 0 to 255 |
| `fuel-per-action` | How much of the fuel budget one action costs |
| `multiply` | `chance` (0 to 1) and `extra`: a hit produces that many more copies of the action's output; one roll per action |
| `requirements` | What the owner must have to buy this level |
| `rewards` | What runs when they buy it |

A ladder with a gap, or a level that improves nothing over the one before, is refused at load and named in the
console. A sale pays the price times `sell-multiplier` times one minus `tax`.

## Requirements

```hocon
requirements = [
  { money   = ">= 3600",        message = "@minion.upgrade.money" }
  { item    = "HOPPER >= 64",   message = "@minion.upgrade.item" }
  { actions = ">= 250",         message = "@minion.upgrade.actions" }
]
```

| Kind | Compares |
|---|---|
| `money` | The balance: `"[currency] op amount"` |
| `item` | What the player holds: `"<item> op amount"` |
| `placeholder` | Any comparison: `"%statistic_time_played% >= 500"` |
| `permission` | A node the player must hold |
| `actions` | This minion's own action counter |

Each carries its own `message`, so a player is told which one they miss.

## Rewards

The lines that run when the level is bought, in order. The take verbs (`[take-money]`, `[take-item]`) are the charge,
and every one is checked before any is taken.

Related: [The ten jobs](jobs.md), [Upgrades](upgrades.md)
