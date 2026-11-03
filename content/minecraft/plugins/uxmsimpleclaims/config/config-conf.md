---
title: config.conf
order: 601
description: Every block of config.conf with its keys and defaults.
icon: file-cog
---

## language

`default` (`en`), `follow-client` (true), `force` (`""`).

## storage

| Key | Default |
|---|---|
| `type` | `sqlite` (or `mysql`, `mariadb`, `postgresql`) |
| `file` | `claims.db`, in `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` |
| `table-prefix` | `uxmsimpleclaims_` |
| `pool-size` | 4 |
| `sync-seconds` | 2 |
| `local-only` | false |
| `copy-from-file` | false |

## network

`server-name` (`survival`), `redis` (`""`), `cross-server-teleport` (true). See [Networks](../features/networks.md).

## worlds

`default-mode` (`normal`), `modes` (nether and end `normal`), `heights`. See [Worlds](../features/worlds.md).

## claims

`min-distance` (1), `max-radius` (5), `default-icon`, `default-colour`, `default-category`, `categories`, `name-min`
(3), `name-max` (24), `text-max` (64). See [Claiming](../features/claiming.md).

## members

`direct-add` (false), `invite-minutes` (10).

## economy

`currency` (`vault`), `treasury-wait-ms` (250), `price` (`curve`, `free-chunks`, `base`, `step`, `factor`, `maximum`,
`tiers`), `refund-percent` (50), `max-visit-fee` (1000), `max-sale-price` (0). See
[Prices and limits](../features/prices-and-limits.md).

## Tax, expiry and inactivity

`tax` (`enabled`, `per-chunk`, `period-days`, `grace-hours`), `expiry` (`time-limited`, `start-days`,
`daily-per-chunk`, `warn-days`, `max-days`), `inactivity` (`enabled`, `days`, `action`). See [Upkeep](../features/upkeep.md).

## rent

`grace-hours` (24), `max-period-days` (30).

## flags

`claim-defaults`, `locked` (`[]`), `wilderness`. See [Flags and rules](../features/flags-and-rules.md).

## Display and teleport

`display` (`title`, `action-bar`, `chat`, `boss-bar`, `greeting`, `border`, `border-seconds`), `teleport.policy`
(`consent`). See [Entering and teleports](../features/entering.md).

## retention

`snapshots-days` (30), `audit-days` (90), `inbox-days` (30).

## limits

`default` and `vip`, each `claims`, `chunks-per-claim`, `total-chunks`, `members`, `roles`, `sub-areas`.

## integrations

| Key | Default |
|---|---|
| `web-maps` | true |
| `web-map-seconds` | 60 |
| `luckperms-contexts` | true |
| `placeholders` | true |
| `worldguard` | true |
| `worldguard-ignore` | `["__global__"]` |
| `webhook-url` | `""` |
| `webhook-events` | `claim`, `unclaim`, `transfer`, `sale` |

## leaderboard

`refresh-seconds` (120), `size` (10).

## menu

`sounds.click` (`ui.button.click 0.6 1.2`).

Related: [Features](../features/index.md)
