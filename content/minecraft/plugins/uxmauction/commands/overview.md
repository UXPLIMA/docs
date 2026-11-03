---
title: Overview
order: 301
description: Both commands at a glance, renaming them, flags, and who sees what.
icon: list
---

| Command | Aliases | Subcommands |
|---|---|---|
| `/auction` | `/ah` | `mailbox`, `mine`, `bids`, `orders`, `history`, `watch`, `search`, `profile`, `sell`, `auction`, `reprice`, `buy`, `bid`, `order`, `fill`, `cancel`, `claim`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/uxmauction` | `/auctionadmin`, `/ahadmin` | `cancel`, `return`, `purge`, `log`, `restore`, `stats`, `lookup`, `lock`, `doctor`, `reload`, `version`, `help` |

Names and aliases are the defaults from [commands.conf](../config/commands-conf.md); a change needs a restart.

## Flags

| Flag | Takes | On |
|---|---|---|
| `-t <time>` | A length: `30s`, `5m`, `6h`, `2d` | `sell`, `auction`, `order` |
| `-c <currency>` | A currency id from `currencies.conf` | `sell`, `auction`, `order` |
| `-r` | | `sell`: retail, buyers may take part of the stack |
| `-m <material\|enchanted\|exact>` | How closely a listing must match | `order` |
| `-x` | | `order`: manual, no listing fills it by itself |

Without `-t` a listing stands for `house.listing.default-duration`; without `-c` the price is in the currency with the
lowest `order`.

## Who sees what

Every player node is given to everyone by default, so a player whose rank had one taken away still sees the subcommand
and is told they may not use it. The `uxmauction.admin` nodes are operators only and hide their subcommands from
everybody else.

Related: [Player commands](player.md), [Admin commands](admin.md)
