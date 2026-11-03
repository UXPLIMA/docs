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
| `file` | `quests.db` | The SQLite file, inside `data/` |
| `host`, `port`, `database`, `username`, `password` | `localhost`, 0, `minecraft`, `minecraft`, `""` | A server database; port 0 is 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `table-prefix` | `quests_` | Before every table name; letters, digits and underscores, at most 32 |
| `pool-size` | 10 | Connections; SQLite uses one |
| `sync-seconds` | 2 | How often this server reads what the others wrote for a player who is here; 0 turns it off |
| `join-delay-ticks` | 0 | How long after a join before the player is read |
| `local-only` | false | This server keeps its rows on the SQLite file whatever `type` says |
| `copy-from-file` | false | Copy the SQLite rows into empty tables on the next start |

See [Database](../database/overview.md).

## rewards

`milestones` (`[]`): overall milestones over a player's whole record; see [Rewards and milestones](../features/rewards.md#overall-milestones).

## battlepass

| Key | Default |
|---|---|
| `enabled` | false |
| `sources.tasks` | true |
| `sources.activity.enabled` | false |
| `sources.activity.any-block` | `xp = 1, chance = 0.05` |
| `sources.activity.any-mob` | `xp = 2, chance = 0.5` |
| `sources.activity.blocks` | `DIAMOND_ORE` 25, `ANCIENT_DEBRIS` 40 |
| `sources.activity.mobs` | `WITHER` 500, `ENDER_DRAGON` 1000 |
| `leaderboard-size` | 10 |
| `board.enabled`, `pass`, `world`, `x`, `y`, `z`, `refresh-seconds` | false, `season`, `world`, 0.5, 100.0, 0.5, 30 |

See [Pass experience and boosters](../features/pass-experience.md).

## anti-glitch

| Key | Default |
|---|---|
| `session` | true |
| `remembered-blocks` | 100000 |
| `coreprotect` | false |
| `bypass-permission` | `uxmquests.bypass` |
| `history-days` | 30 |

See [Anti-glitch](../features/anti-glitch.md).

## feedback

`toast-icon` (`BOOK`), `toast-frame` (`TASK`), and the lists `started`, `progressed`, `completed`, `paid`. See
[Feedback](../features/feedback.md).

## objectives

`sampling` (true), `sample-interval-ticks` (10), `placeholder-interval-ticks` (40), `look-distance` (5). See
[Task types](../features/task-types.md#sampled-types).

## ranks

`board-size` (`uxmquests.count.`), `rerolls` (`uxmquests.rerolls.`). See [Ranks](../features/ranks.md).

## menu

| Key | Meaning |
|---|---|
| `sounds.click`, `sounds.page`, `sounds.denied` | `name`, `volume`, `pitch` |
| `phase-display` | One material per quest phase, read as `%phase-material%`; see [Quests and board](../menus/quests.md#phase-materials) |
| `lore-appendix` | Lines every quest tile carries under its own, read as `%appendix%`; `@key` lines are translated |

Related: [Features](../features/index.md)
