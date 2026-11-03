---
title: Operator commands
order: 303
description: Every /uxmcrates subcommand, its permission, its arguments and what it answers.
icon: shield
---

`/uxmcrates`, aliases `/cratesadmin` and `/uxmcr`. Every node is `op` by default.

## Keys

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates key` | `uxmcrates.admin.key` | Prints the usage of the key branch |
| `/uxmcrates key give <player> <key> <amount>` | `uxmcrates.admin.key.give` | Adds to a player's balance, online or away |
| `/uxmcrates key giveitem <player> <key> <amount>` | `uxmcrates.admin.key.giveitem` | Gives the physical key item. What does not fit waits in the player's pending rewards |
| `/uxmcrates key take <player> <key> <amount>` | `uxmcrates.admin.key.take` | Takes from a player's balance; refuses with `key.not-enough` past what they hold |
| `/uxmcrates key set <player> <key> <amount>` | `uxmcrates.admin.key.set` | Writes a player's balance |
| `/uxmcrates key giveall <key> <amount>` | `uxmcrates.admin.key.giveall` | Adds to the balance of every online player, and says how many |
| `/uxmcrates key giveoffline <player> <key> <amount>` | `uxmcrates.admin.key.giveoffline` | The same balance grant as `give`, under its own node for a web store |
| `/uxmcrates key ledger <player>` | `uxmcrates.admin.key.ledger` | Every grant, spend, transfer and reset of a player, newest first, `ledger.page-size` rows |

A balance grant works for a player who is away: they find it when they join. A web store needs no delivery plugin.

The ledger row reads the date in `date-format`, the movement (`granted`, `spent`, `transferred`, `set to a number`),
the amount, the key and the source.

## Crates in the world

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates place <crate>` | `uxmcrates.admin.place` | Makes the block you look at the crate. Player only |
| `/uxmcrates tool <crate>` | `uxmcrates.admin.place` | Gives the [link tool](../features/crates.md#the-link-tool) for a crate. Player only |
| `/uxmcrates remove` | `uxmcrates.admin.remove` | Takes the crate off the block you look at. Player only |
| `/uxmcrates forceopen <player> <crate>` | `uxmcrates.admin.forceopen` | Opens a crate for an online player with no key, no cost and no cooldown. The crate's permission, requirements and world rule still hold |

`place` refuses with `place.no-block` when you look at no block and `place.protected` inside another player's claim,
unless `claims.place-inside-others-claims` is on.

## Envoys

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates envoy start <envoy>` | `uxmcrates.admin.envoy` | Starts an envoy now |
| `/uxmcrates envoy stop <envoy>` | `uxmcrates.admin.envoy` | Ends a running envoy and puts every block back |
| `/uxmcrates envoy list` | `uxmcrates.admin.envoy` | Every envoy, `running` or `waiting`, and when it next runs, or `started by hand` |

## Resets and rerolls

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates reset` | `uxmcrates.admin.reset` | Prints the usage of the reset branch |
| `/uxmcrates reset cooldown <player> <crate>` | `uxmcrates.admin.reset.cooldown` | Clears a player's wait on a crate, a one-time crate included |
| `/uxmcrates reset player-limit <player> <crate>` | `uxmcrates.admin.reset.player-limit` | Clears a player's win counts on every reward of a crate |
| `/uxmcrates reset global-limit <crate>` | `uxmcrates.admin.reset.global-limit` | Clears the server's win counts on every reward of a crate |
| `/uxmcrates reroll` | `uxmcrates.admin.reroll` | Prints the usage of the reroll branch |
| `/uxmcrates reroll give <player> <amount>` | `uxmcrates.admin.reroll.give` | Grants rerolls, kept until spent |

## The log

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates log <field> <value>` | `uxmcrates.admin.log` | Reads the opening log, newest first |

`<field>` is `player`, `crate` or `reward`. A player gets the [log window](../menus/log.md), `log.page-size` rows a
page; the console gets lines with the date, the player, the reward and the chance it had.

## The plugin

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates` | `uxmcrates.admin.version` | Says to add `help` |
| `/uxmcrates editor` | `uxmcrates.admin.editor` | Opens the [editor](../menus/editor.md). Player only. Answers `editor.off` when `editor.enabled` is off |
| `/uxmcrates migrate <plugin>` | `uxmcrates.admin.migrate` | Reads `excellentcrates`, `crazycrates` or `specializedcrates` into files. See [Migration](../integrations/migration.md) |
| `/uxmcrates doctor` | `uxmcrates.admin.doctor` | The health checks, one line each |
| `/uxmcrates reload` | `uxmcrates.admin.reload` | Reads every file again and says how long it took |
| `/uxmcrates version` | `uxmcrates.admin.version` | The running version |

## Language

| Command | Permission | What it does |
|---|---|---|
| `/uxmcrates lang` | `uxmcrates.language` | The language you read, and the choices |
| `/uxmcrates lang set <language>` | `uxmcrates.language` | Chooses a language |
| `/uxmcrates lang reset` | `uxmcrates.language` | Follows the client's language again |
| `/uxmcrates lang set-player <player> <language>` | `uxmcrates.admin.language` | Sets another player's language; a player who is away gets it at their next join |
| `/uxmcrates lang reset-player <player>` | `uxmcrates.admin.language` | Clears another player's choice |

`uxmcrates.language` is held by everyone, so every player sees `/uxmcrates lang` and nothing else of the root.

## Reload

A reload reads the crates, keys, rewards, animations, recipes, envoys, messages, windows and menu sounds again. It is
applied as a whole or not at all: a file that does not parse leaves the running configuration as it was and the
answer is `admin.reload-failed`. The storage block and command names need a restart.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
