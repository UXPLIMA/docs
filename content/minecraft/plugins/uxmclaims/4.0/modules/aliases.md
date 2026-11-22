---
title: Aliases
order: 1345
description: Your own short commands from aliases.yml, and the Towny and Lands command names as optional profiles.
icon: corner-down-right
---

The aliases module adds command names of your own, such as `/accept` for `/claim invite accept`, from the 3.x
`aliases.yml` file. Two optional profiles add the command names players know from Towny or Lands, so a server
moving over keeps their habits.

Module `aliases` · enabled by default · `modules/aliases/config.yml`

## Commands

The shipped `aliases.yml`:

| Command | What it does | Permission |
|---|---|---|
| `/accept [land]` | Runs `/claim invite accept`. | as the target command |
| `/deny [land]` | Runs `/claim invite reject`. | as the target command |
| `/unclaim [land]` | Runs `/claim delete`. | as the target command |
| `/untrust <player>` | Runs `/claim kick`. | as the target command |

Arguments after an alias are passed on to the target, and tab completion follows the target. Each alias also
works as `/uxmclaims:<alias>`.

## Settings

`config.yml`:

| Key | Default | What it does |
|---|---|---|
| `profiles.towny` | `false` | Towny command names, see the table below. |
| `profiles.lands` | `false` | Lands command names, see the table below. |

`aliases.yml`:

| Key | Default | What it does |
|---|---|---|
| `aliases.<label>` | four entries, see above | `<label>: "target command"`. The target is written without the slash. |

## Profiles

| Profile | Command | Leads to | Only with |
|---|---|---|---|
| `towny` | `/res`, `/resident` | `/town resident` | [Towns](towns.md) |
| `towny` | `/ta`, `/townyadmin` | `/uxm` | |
| `towny` | `/towny map` | `/claim map text` | |
| `towny` | `/nation add` (hidden) | `/nation invite` | [Nations](nations.md) |
| `lands` | `/land`, `/lands` | `/claim` | |
| `lands` | `/nations` | `/nation` | [Nations](nations.md) |

`/town new`, `/town add` and `/nation new` already belong to the towns and nations modules. `/land` and
`/lands` exist only through the Lands profile. Any other first argument after `/towny` answers that the
command is not available on this server.

## Notes

- A name another plugin already uses, or an uxmClaims command, is never taken. The log names each skipped
  alias with `W-ALIAS-CLASH`.
- While Towny or Lands is installed, its profile adds no command names.
- A changed target works after `/uxm reload`. A new alias, or a profile switched on or off, needs a restart. A
  removed alias answers that it was removed until the next restart.
- An alias whose target belongs to a module that is off answers that the command is not available.

Related: [Invites](invites.md), [Towns](towns.md), [Nations](nations.md), [Upgrading from 3.x](../upgrading/from-3x.md)
