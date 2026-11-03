---
title: config.conf
order: 601
description: Every block and key of config.conf, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | The fallback language |
| `follow-client` | true | Read the client's language when there is a file for it |
| `force` | `""` | One language for everybody |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"jobs.db"` | The SQLite file, inside `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` | The database server |
| `table-prefix` | `"uxmjobs_"` | Before every table |
| `pool-size` | 4 | Connections |
| `sync-seconds` | 2 | How often a level or booster changed on another server is read |
| `leaderboard-size` | 100 | Rows a board reads |
| `leaderboard-refresh-seconds` | 120 | How often boards are read again |
| `flush-seconds` | 30 | How often decay, income windows and payouts reach the disk |
| `local-only` | false | This server keeps its own rows on the SQLite file |
| `copy-from-file` | false | Copy the SQLite rows into an empty network database |

On a network the income caps, decay counters and ledger are shared state: a network that splits them splits the
protections. See [Database](../database/overview.md).

## slots, permissions, employment

| Key | Default | Meaning |
|---|---|---|
| `slots.default` | 3 | Jobs a player may hold |
| `slots.node` | `"uxmjobs.slots."` | `uxmjobs.slots.5` holds five; the highest wins |
| `permissions.refresh-seconds` | 20 | How often slots and zone access are read from nodes |
| `employment.rejoin-cooldown-seconds` | 3600 | Wait before rejoining a job left |
| `employment.keep-on-return` | 0.5 | Share of archived experience a return keeps, compounded per departure |
| `employment.confirm-before-leaving` | true | `/jobs leave` asks in a dialog first |

## ledger and decay

| Key | Default |
|---|---|
| `ledger.enabled`, `retention-days`, `prune-interval-minutes` | true, 14, 60 |
| `decay.enabled`, `threshold`, `step`, `floor` | true, 64, 0.01, 0.25 |
| `decay.recovery-minutes`, `region-chunks`, `retention-days` | 10, 4, 7 |

See [Protections](../features/protections.md).

## income-cap

`window-minutes` 60; `money` cap 25000 softness 1.0; `experience` cap 20000 softness 2.0; `points` cap 2000 softness
1.0. See [Income cap and tax](../features/income-cap-and-tax.md).

## progress-alerts

`at { <percent> { actions = [...] } }`, empty by default. See [Display](../features/display.md#milestones).

## tax and server-account

| Key | Default |
|---|---|
| `tax.enabled`, `rate`, `node`, `bypass` | false, 0.05, `"uxmjobs.tax."`, `"uxmjobs.tax.bypass"` |
| `server-account.enabled`, `holder`, `pay-what-is-left` | false, `""`, true |

## claims

`enabled` true, `foreign-claim-multiplier` 0.0, `own-claim-multiplier` 1.0, `combine` `"any-land"`, `providers {}`.
See [Protections](../features/protections.md#claims).

## zones

| Key | Default |
|---|---|
| `enabled` | true |
| `wand-material` | `"golden_hoe"` |
| `regeneration-interval-seconds` | 30 |
| `regeneration-batch` | 200 |
| `frame.enabled`, `refresh-seconds`, `step`, `maximum-points`, `colour`, `size` | true, 1.0, 1.0, 800, `"#4ecca3"`, 1.0 |

## leaderboard

`chat-lines` 10, `empty-head` `"entity/player/wide/steve"`, `periods` `[alltime, monthly, weekly, daily]`, `time-zone`
`""`, `week-starts` `monday`, `keep-windows` 12. See [Leaderboards](../features/leaderboards.md).

## signs

`enabled` true, `marker` `"[uxmjobs]"`, `refresh-seconds` 60. See [Signs](../features/signs.md).

## report

`enabled` true, `retention-days` 7, `window-minutes` 60, `alert-multiple` 10.0, `alert-minimum-sample` 5.

## explore and placeholder-actions

`explore.rows-per-player` 5000, `explore.retention-days` 30; `placeholder-actions.minimum-seconds` 5,
`placeholder-actions.maximum-step` 100. See [Action types](../features/action-types.md).

## editor and menu

`editor.enabled` true (a restart). `menu.sounds`: `click` (`block.note_block.pling` 0.6 1.5), `page`
(`item.book.page_turn` 0.7 1.0), `denied` (`block.note_block.bass` 0.6 0.9).

Related: [Configuration](index.md)
