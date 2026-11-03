---
title: config.conf
order: 601
description: Every block of config.conf with its keys and defaults.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `en` | The language a player reads when nothing else picks one |
| `follow-client` | true | Read the client's language when a file for it exists |
| `force` | `""` | One language for everybody |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `sqlite` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `plots.db` | The SQLite file |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` | A server database; port 0 is the backend's own |
| `table-prefix` | `uxmplots_` | The prefix of every table |
| `pool-size` | 4 | Connections |
| `local-only` | false | This server keeps its rows on the SQLite file whatever `type` says |
| `copy-from-file` | false | Copy the SQLite rows into empty tables on the next start |

See [Database](../database/overview.md).

## plots

| Key | Default | Meaning |
|---|---|---|
| `name-max-length` | 32 | The longest `/plot name` |
| `auto-search-rings` | 16 | How far `/plot auto` looks |
| `auto-world` | `""` | Where `/plot auto` claims from outside a plot world; empty is the first |

## limits

`world-node` (`uxmplots.limit.world.`), `total-node` (`uxmplots.limit.total.`), `default-total` (0). See
[Claiming](../features/claiming.md#limits).

## work

`blocks-per-batch` (20000), `tps-floor` (17, never below 5), `held` (an effect list for the player who is waiting). See
[Shaping a plot](../features/shaping.md#the-work-queue).

## snapshots

`keep` (5), `keep-automatic` (5), `before-reset` (true), `scheduled.every-minutes` (0, off), `scheduled.per-sweep` (10).

## visits

`count-after-seconds` (15), `count-again-after-seconds` (3600), `sweep-every-seconds` (10).

## guestbook

`max-length` (120), `cooldown-seconds` (3600), `keep-per-plot` (50), `forbidden-words` (`[]`).

## tips

`least` (1), `most` (10000).

## sessions

`longest-minutes` (120), `most-guests` (8).

## expiry

`warn-after-days` (30), `tell-every-hours` (24), `expire-after-days` (90, 0 turns it off), `release-automatically`
(false).

## reports

`max-length` (120), `cooldown-seconds` (300), `forbidden-words`, `reward` (an effect list).

## leaderboard

`refresh-seconds` (60), `size` (10), `tips-currency` (`diamonds`), `honour-points` (`[5, 3, 2]`), `post-format`.

## follow

`watch-seconds` (300), `max-followed` (100), `on-published`, `on-placed`, `on-levelled`.

## archive

`viewing.plots` (`[]`, plot ids such as `creative:20;20`), `viewing.minutes` (10).

## personality

`ambience-particles` (3), `sign-radius-plots` (2), `border` (`particle = "END_ROD"`, `seconds = 8`,
`every-millis = 500`): what `/plot outline` draws.

## merge

`most-plots` (4; 1 turns merging off).

## crash-items

`rules` (seven switches, all false), `thresholds` (`book-characters` 12000, `book-pages` 50, `firework-explosions` 12,
`potion-effects` 8, `potion-level` 10, `attribute` 100), `on-taken` (`[]`). See
[Protection](../features/protection.md#crash-items).

## featured

`pinned-slots` (9), `sponsored-slots` (9), `sponsored-hours` (168), `sponsored-currency` (`""`), `sponsored-price`
(2500).

## slots

`most` (0), `currency` (`""`), `first` (500), `step` (500), `curve` (`linear`).

## menu

`sounds.click`, `sounds.page`, `sounds.denied`: `name`, `volume`, `pitch`.

## Effect lists

Every effect list (`held`, `reward`, `on-taken`, the follow lines, the world's `on-entering`) takes the uxmLib effect
lines: `[message] @key`, `[sound] NAME volume pitch`, `[give-money] <currency> <amount>`, `[command] ...`. `@key` reads a
line of the language files.

Related: [Features](../features/index.md)
