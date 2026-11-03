---
title: Player commands
order: 202
description: Every /glow subcommand, its permission, its arguments and what it answers.
icon: user
---

`/glow`, aliases `/glowcolor` and `/glowcolour`. Every subcommand also needs `uxmglow.use`.

| Command | What it does | Permission |
|---|---|---|
| `/glow` | Opens the picker | `uxmglow.use` |
| `/glow set <colour>` | Glows in one colour | the colour's own node |
| `/glow cycle <colours> [interval-ms]` | Steps through several colours | `uxmglow.cycle` and every colour named |
| `/glow off` | Stops glowing | `uxmglow.use` |
| `/glow colours` | Lists the colours this player may use | `uxmglow.use` |
| `/glow lang` | Which language they read, and what else there is | `uxmglow.language` |
| `/glow lang set <language>` | Reads this plugin in one language, whatever the client says | `uxmglow.language` |
| `/glow lang reset` | Follows the client again | `uxmglow.language` |
| `/glow lang set-player <player> <language>` | Sets another player's language; one who is away gets it at their next join | `uxmglow.admin.language` |
| `/glow lang reset-player <player>` | Clears another player's choice | `uxmglow.admin.language` |

## cycle

The colours are one comma-separated argument, `red,blue,gold`, so the interval that may follow stays unambiguous. The
interval is in milliseconds and optional: leave it out, or write `0`, and `cycle.default-interval-ms` is used.

| Check | Refusal |
|---|---|
| Every name is a colour | `error.colour-unknown` |
| Two colours at least | `error.cycle-too-few` |
| At most `cycle.maximum-colours` | `error.cycle-too-many` |
| The interval is at least `cycle.minimum-interval-ms` | `error.cycle-interval-too-short` |
| The player holds `uxmglow.cycle` | `error.cycle-not-permitted` |
| The player holds every colour named | `error.colour-not-permitted` |

Every colour of a cycle is checked and not only the first, so a player who holds red and blue but not gold cannot
reach gold by hiding it in the middle of a list.

## Answers

| Command | Answer |
|---|---|
| `set` | The `effects.set` moment: `glow.set` and a chime |
| `cycle` | The `effects.cycling` moment: `glow.cycling` and a chime |
| `off` | The `effects.cleared` moment; `glow.already-off` when not glowing |
| `colours` | `colours.available` with each colour in itself, or `colours.none` |

Related: [Overview](overview.md), [Colours and cycles](../features/colours-and-cycles.md)
