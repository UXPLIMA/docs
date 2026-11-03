---
title: config.conf
order: 501
description: Every block of config.conf with its keys and defaults.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `en` | The language a player reads when nothing else picks one |
| `follow-client` | true | Read the client's language when a file for it exists |
| `force` | `""` | One language for everybody; a player can then not change theirs |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `sqlite` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `shop.db` | The SQLite file, inside `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` | A server database; port 0 is the backend's own |
| `table-prefix` | `uxmshop_` | The prefix of every table |
| `pool-size` | 4 | Connections; SQLite uses one |
| `local-only` | false | This server keeps its rows on the SQLite file whatever `type` says |
| `copy-from-file` | false | Copy the SQLite rows into empty tables on the next start |

See [Database](../database/overview.md).

## shop

| Key | Default | Meaning |
|---|---|---|
| `default-currency` | `coins` | The currency of a section that names none, and of player shops |
| `tax.percent` | 0 | Taken out of what a seller is paid |
| `tax.bypass` | `uxmshop.bypass.tax` | The node that pays no tax |
| `sell-all-limit` | 2304 | Items one `/sell all` may move |
| `ranks` | `{}` | Rank blocks: `permission`, `buy`, `sell`; see [Prices](../features/prices.md#ranks) |

## player-shops

| Key | Default |
|---|---|
| `limit` | 3 |
| `limit-permission` | `uxmshop.shops` |
| `offers` | 27 |
| `tax.percent` | 0 |
| `container-memory` | `30s` |
| `hologram.*` | See [The shop hologram](../features/hologram.md) |

## blacklist

`materials` (`[]`), `bypass` (`uxmshop.bypass.blacklist`). See [Selling](../features/selling.md#blacklist).

## menu

`sounds.click`, `sounds.page`, `sounds.denied`: `name`, `volume`, `pitch`.

## log

`keep-days` (30; 0 keeps every row).

## editor

`enabled` (true): registers `/shopedit`. A change needs a restart.

## effects

`bought`, `sold`, `taxed`, `cannot-afford`, `out-of-stock`, `quota-reached`, `no-room`. See
[Trade effects and the log](../features/trade-effects.md).

Related: [Features](../features/index.md)
