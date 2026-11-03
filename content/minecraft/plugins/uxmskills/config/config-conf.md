---
title: config.conf
order: 501
description: Every block of config.conf with its keys and defaults.
icon: file-cog
---

| Block | Keys and defaults | Page |
|---|---|---|
| `language` | `default` `en`, `follow-client` true, `force` `""` | [Languages](languages.md) |
| `storage` | `type` `sqlite`, `file` `skills.db`, `table-prefix` `uxmskills_`, `pool-size` 4, `sync-seconds` 2, `local-only`, `copy-from-file` | [Database](../database/overview.md) |
| `curve` | `preset` `slow-late`, `base` 90 | [Curves](../features/curves.md) |
| `categories` | `ores`, `logs`, `saplings`, `pickaxes`, `shovels`, `axes`, `hoes`, `swords` | [Sources](../features/sources.md#categories) |
| `experience` | `global-multiplier` 1.0, `per-skill`, `maximum-multiplier` 10.0, `cancelled-events-pay` false, `movement-pulse-ms` 1000 | [Experience](../features/experience.md) |
| `claims` | `own`, `others`, `wilderness` 1.0, `providers`, `combine` `any-land` | [Experience](../features/experience.md#land) |
| `leaderboard` | `size` 100, `chat-lines` 10, `refresh-seconds` 120, `empty-head`, `periods`, `time-zone`, `week-starts`, `keep-windows` 12 | [Leaderboards](../features/leaderboards.md) |
| `progress-bar` | `width` 20, `filled` a bar, `empty` a dot | [Progress](../features/progress.md) |
| `notifications` | `pooling-window-ms` 2000, `action-bar` true, `chat` false, `boss-bar` false, `boss-bar-linger-ms` 4000 | [Progress](../features/progress.md) |
| `level-up`, `capstone` | `actions` | [Progress](../features/progress.md) |
| `specialisation` | `default` 0, `nodes` | [Progress](../features/progress.md#specialisation) |
| `death` | `enabled` false, `whole-level` false, `keep-ratio` 0.5, `skills` | [Progress](../features/progress.md#death) |
| `activator` | `enabled` false, `material` `ENCHANTING_TABLE`, `name`, `vertical-faces` true | [Progress](../features/progress.md#the-activator) |
| `backups` | `on-shutdown` true, `interval-minutes` 360, `maximum-players` 0, `keep` 10 | [Data](../features/data.md) |
| `points` | `enabled` false, `per-skill-level` 1, `starting` 0, `refundable` true | [Abilities](../features/abilities.md#points) |
| `editor` | `enabled` true, `watch-seconds` 3 | [The editor](../features/editor.md) |
| `placeholders` | `refresh-ms` 1000: every online player's numbers are rebuilt on this beat | [Placeholders](../placeholders/reference.md) |
| `menu` | `sounds.click`, `page`, `denied` | |

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `en` | The language a player reads when nothing else picks one |
| `follow-client` | true | Read the client's language when a file for it exists |
| `force` | `""` | One language for everybody |

## storage

| Key | Default |
|---|---|
| `type` | `sqlite` (or `mysql`, `mariadb`, `postgresql`) |
| `file` | `skills.db`, in `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` |
| `table-prefix` | `uxmskills_` |
| `pool-size` | 4 |
| `sync-seconds` | 2 |
| `local-only`, `copy-from-file` | false, false |

Related: [Features](../features/index.md)
