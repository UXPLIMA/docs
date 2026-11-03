---
title: config.conf
order: 501
description: Every block and key of config.conf, with its default.
icon: file-cog
---

## language

`default` `"en"`, `follow-client` true, `force` `""`.

## storage

| Key | Default |
|---|---|
| `type` | `"sqlite"` |
| `file` | `"minions.db"` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` |
| `table-prefix` | `"minions_"` |
| `pool-size` | 10 |
| `local-only` | false |
| `copy-from-file` | false |

There is no `sync-seconds`: a minion is worked by the server it stands on. See [Database](../database/overview.md).

## tick, render, placeholders

| Key | Default |
|---|---|
| `tick.interval-ticks`, `max-actions-per-pass` | 20, 0 |
| `render.view-distance`, `refresh-ticks`, `menu-refresh-ticks` | 48, 40, 20 |
| `render.animation`, `talk`, `talk-interval-ticks` | true, true, 600 |
| `render.talk-lines` | `@minion.talk.one` to `four` |
| `placeholders.refresh-ticks` | 40 |

See [Drawing](../features/drawing.md).

## fuel and experience

`fuel.enabled` true. `experience.enabled` false, `per-action` 0, `per-material {}`. See [Fuel](../features/fuel.md),
[Economy](../features/economy.md).

## destinations, prices, economy, banking

| Key | Default |
|---|---|
| `destinations.internal` | on, no node |
| `destinations.sell` | on, `uxmminions.destination.sell` |
| `destinations.chest` | on, no node, `max-distance` 5 |
| `prices.sources` | `["uxmshop", "file"]` |
| `economy.currency`, `treasury-wait-ms` | `"exp"`, 250 |
| `banking.enabled` | false |

See [Output](../features/output.md).

## loops, smelting, upgrades

`loops.fuel-from-minion-output`, `feeder-feeds-feeder`, `slayer-paid-for-spawner-mob`: all false.
`smelting.overrides {}`. `upgrades.consume-minion` false.

## limits

`per-scope` 25, `per-type` (spawner 3, seller 5, feeder 5), `per-chunk` 8, `bypass-permission`
`"uxmminions.limits.bypass"`, `whitelist` false, `worlds` `["world_the_end"]`. See [Limits](../features/limits.md).

## claims and teleport

`claims.respect-build-permission` true, `work-inside-claim-only` true, `leader-bypass` true, `on-claim-lost`
`"refund"`, `check-interval-seconds` 60, `disabled-providers` `[]`. `teleport.enabled` true, `cost` 250.0,
`cooldown-seconds` 5.

## ledger, audit, effects

`ledger.enabled` true, `retention-days` 30, `report-lines` 10. `audit.retention-days` 0 (keep everything).
`effects.refused`, `upgraded`, `sold`: one sound each. See [Ledger and audit](../features/ledger-and-audit.md).

## editor and menu

`editor.enabled` true. `menu.sounds`: `click` (`block.note_block.pling` 0.6 1.5), `page` (`item.book.page_turn` 0.7
1.0), `denied` (`block.note_block.bass` 0.6 0.9).

Related: [Configuration](index.md)
