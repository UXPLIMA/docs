---
title: "Guide"
order: 10
description: "How uxmMinions works, how to set it up and what each file holds."
---

uxmMinions lets players place small workers that mine, chop, farm, fish, fight, collect, feed, sell, spawn or heal on their own. Every minion type is a file, every level of it is a record of numbers, and the output goes into the minion's own storage, a nearby chest, or straight to a sale.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmMinions/`, with ten example types under `types/`.
3. Check `economy`, `prices` and `limits` in `config.conf`, then run `/uxmminions reload`.
4. Hand out a minion with `/uxmminions give <player> <type> [level]`. The player places the item to put it in the world.

A shop plugin, an economy, PlaceholderAPI and the claim plugins are optional.

## Files

| File | What it holds |
|---|---|
| `config.conf` | storage, tick, render, placeholders, fuel switch, experience, destinations, prices, economy, loops, smelting, upgrades, limits, claims, teleport, ledger, audit, effects, editor |
| `hologram.conf`, `tool.conf` | the hologram and the tools, each under its own block name; a block an earlier release kept in `config.conf` moves here on the first start |
| `types/<id>.conf` | one minion type per file; the file name is the id |
| `fuel.conf` | the fuel grades |
| `prices.conf` | the plugin's own price table for the seller |
| `skins.conf` | the looks a player may choose |
| `editor/new-type.conf`, `menus/*.conf`, `commands.conf`, `messages/`, `theme.conf` | editor template, windows, commands, words, colours |

## A type

`types/collector.conf` is commented in full. In short:

| Key | Meaning |
|---|---|
| `job` | `miner`, `lumberjack`, `farmer`, `fisher`, `slayer`, `collector`, `feeder`, `seller`, `spawner` or `healer`; a job another plugin registers is written quoted, `"their-plugin:job"` |
| `icon`, `display-name` | the menu material, and the name: a line of the messages files (`"@content.types.miner.display-name"`), a block per language, or one name |
| `action-interval` | ticks between two actions |
| `targets`, `blacklist` | what the type acts on (blocks, mobs, loot lines, items, whatever the job reads); an empty `targets` means everything not blacklisted |
| `experience` | optional overrides of the server's experience block for this type |
| `levels` | the ladder (below) |

Each level has:

| Key | Meaning |
|---|---|
| `level` | from 1 upward with no gap |
| `action-interval` | this level's interval, in ticks |
| `range` | how far the work reaches, in blocks |
| `storage` | how many slots its container holds |
| `smelt` | smelt the output before storing it |
| `sell-multiplier`, `tax` | a sale pays the price times the multiplier times (1 minus the tax); tax is below one |
| `held-item`, `armour` | how it is drawn: the item it holds and the suit colour (`red`, `green`, `blue`, 0 to 255) |
| `fuel-per-action` | how much of the fuel budget one action costs |
| `multiply` | `chance` (0 to 1) and `extra`: a hit produces that many more copies of the action's output |
| `requirements`, `rewards` | what the owner needs to buy this level (money, item, placeholder, permission, the minion's action count), and what runs when they do |

A ladder with a gap, or a level that improves nothing over the one before, is refused at load.

## Where the output goes

`destinations` has three, each with `enabled` and an optional `permission`:

- `internal`: the minion's own storage. A full minion stops.
- `sell`: the output is sold and the money credited to the owner. Prices come from `prices.sources` in order (`uxmshop`, then `file`, meaning `prices.conf`). A material with no price stays in the minion and is never destroyed.
- `chest`: a container within `max-distance` blocks. Items are never lost if the chest is broken or unloaded.

`smelting.overrides` changes what a smelting level turns a material into, or `none` to leave it.

## Fuel

With `fuel.enabled`, a minion burns one fuel item at a time from `fuel.conf`. Each tier has an `item`, a `speed` (the percent cut off the action interval, at most 90) and `actions` (how many actions it lasts). A stopped, blocked, full or waiting minion spends nothing, and minions do not work while their owner is offline. Without fuel a minion works at its plain interval.

## Limits

| `limits` key | Meaning |
|---|---|
| `per-scope` | the total one scope may place; a scope is an island where a claim plugin has one, otherwise a player |
| `per-type` | a quota per type; an unnamed type takes the total |
| `per-chunk` | how many may stand in one chunk |
| `bypass-permission` | passes every cap |
| `whitelist`, `worlds` | `true` makes `worlds` the only worlds allowed, `false` makes them forbidden |

`/uxmminions slots <player> <amount>` grants extra slots on top of the total. Checks run in order: world, total, type, chunk.

## Economy loops and upgrades

`loops` keeps three money-printing loops closed by default: burning fuel that another minion made, feeders feeding feeders, and slayers paid for a spawner minion's mobs. Open one only on purpose.

`upgrades.consume-minion` (off by default) lets a player feed one minion to another of the same type and level to raise it, with `/minions consume`.

## Other settings

| Block | Meaning |
|---|---|
| `tick` | how often minions are checked, and an optional cap on actions per pass for very large servers |
| `economy.currency` | the balance teleport costs and money requirements use: `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury[:<currency>]` |
| `teleport` | `enabled`, `cost` and `cooldown-seconds` for teleporting to a minion |
| `ledger` | what minions paid their owners, kept for `retention-days`; `/minions report` reads it |
| `claims` | which claim plugins decide where a minion may stand and what it may touch |

## Commands

Player commands need no permission.

| Command | What it does |
|---|---|
| `/minions` or `/minions list` | your minions |
| `/minions report` | what your minions have paid you |
| `/minions trust <id> <player>`, `untrust <id> <player>` | let another player use one of your minions |
| `/minions name <id> <name>` | name a minion |
| `/minions consume <eaten> <into>` | feed one minion to another, when `upgrades.consume-minion` is on |

| Command | Permission | What it does |
|---|---|---|
| `/uxmminions give <player> <type> [level]` | `uxmminions.admin.give` | give a minion item |
| `/uxmminions find <id>` | `uxmminions.admin.find` | where one minion stands |
| `/uxmminions open <id>` | `uxmminions.admin.open` | open its panel |
| `/uxmminions minions <player>` | `uxmminions.admin.minions` | browse a player's minions |
| `/uxmminions remove <id>` | `uxmminions.admin.remove` | take a minion away |
| `/uxmminions restore <id>` | `uxmminions.admin.restore` | give a minion back to its owner as an item |
| `/uxmminions slots <player> <amount>` | `uxmminions.admin.slots` | grant extra slots |
| `/uxmminions ledger <player>` | `uxmminions.admin.ledger` | what a player's minions have earned |
| `/uxmminions migrate <jetsminions\|litminions> [source]` | `uxmminions.admin.migrate` | read another plugin's minion data; `source` overrides the default file |
| `/uxmminions editor` | `uxmminions.admin.editor` | the in-game editor |
| `/uxmminions reload`, `version`, `doctor` | `uxmminions.admin.reload`, `.version`, `.doctor` | the usual operator commands |

A migration skips blocks that already hold one of ours and types this server has no file for, so it is safe to run twice. The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmminions_placed%`, `%uxmminions_limit%`, `%uxmminions_remaining%` | placed, allowed (grants included), and the difference |
| `%uxmminions_placed_<type>%`, `%uxmminions_limit_<type>%` | the same for one type |
| `%uxmminions_earned%` | everything the ledger has paid the player |
| `%uxmminions_minion_<id>_type%`, `_level%`, `_name%`, `_world%`, `_facing%` | one minion's facts |
| `%uxmminions_minion_<id>_working%`, `_why%` | whether it works, and why not |
| `%uxmminions_minion_<id>_fuel%`, `_actions%`, `_stock%`, `_capacity%` | fuel left, actions done, items held, room |
| `%uxmminions_minion_<id>_destination%`, `_earnings%` | where its output goes, and what it has paid |

## Troubleshooting

- **A minion stands still.** Its panel and `%uxmminions_minion_<id>_why%` say why: full, no fuel, owner offline, blocked or a missing job.
- **The seller does not sell an item.** No price source knows it; add it to `prices.conf`.
- **A type fails to load.** Its levels have a gap, a level improves nothing, or `job` is unknown; the console names the file.
- **Players cannot place more.** A limit is reached; `/uxmminions slots` grants more.
