---
title: general-settings.yml
order: 52
description: The tournament rules: removed items, ties, warnings, creative mode, chat and the log cleaner.
icon: sliders-horizontal
---

`general-settings.yml` holds the rules every tournament follows. On a network it is the same on
every server: edit it on the management server and run `/sr reload` there. Leave the
`revision:` line at the top alone.

| Key | Default | What it does | More on |
|---|---|---|---|
| `freeze-removed-item-scores` | `true` | Points earned with an item keep counting after the item is removed from a running event | [Events and runs](../features/events-and-runs.md#changing-an-event) |
| `sounds.deposit` | `ENTITY_EXPERIENCE_ORB_PICKUP` | Played after a deposit | [Depositing](../features/depositing.md) |
| `sounds.bulk-deposit` | `ENTITY_PLAYER_LEVELUP` | Played after a bulk deposit | [Depositing](../features/depositing.md) |
| `ranking.tiebreak` | `SHARED` | What decides a tie: `SHARED`, `MOST_ITEMS` or `EARLIEST` | [Ranking](../features/ranking.md) |
| `ranking.shared-style` | `DENSE` | Numbering after a shared rank: `DENSE` or `COMPETITION` | [Ranking](../features/ranking.md) |
| `ranking.shared-max` | `0` | The most players who can share one rank. `0` means no limit | [Ranking](../features/ranking.md) |
| `end-warnings` | `1h`, `10m` | When players are warned before a run ends. An empty list turns warnings off | [Events and runs](../features/events-and-runs.md#ending) |
| `creative-deposit` | `DENY` | `ALLOW` or `DENY` deposits from players in creative mode | [Depositing](../features/depositing.md) |
| `deposited-deliverables-glow` | `true` | Items a player has deposited glow in the menus | [Depositing](../features/depositing.md) |
| `chat-quit-triggers` | `!exit`, `cancel` and one word per language | Words that cancel a chat prompt | |
| `log-cleaner` | on, 30 days, 5000 | Trims the in-game logs | [History and logs](../features/history-and-logs.md#logs) |

Sound names are Minecraft sound names, such as `ENTITY_PLAYER_LEVELUP`. A name that does not exist
is reported in the console and the default deposit sound plays instead.

`end-warnings` uses the same time format as event lengths: `1h`, `10m`, `90s`. Each warning is sent
once per run.

Related: [config.yml](config-yml.md), [Event files](event-files.md)
