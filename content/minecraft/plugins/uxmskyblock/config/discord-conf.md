---
title: discord.conf
order: 622
description: Discord webhooks for milestones, boards, alliances and staff actions.
icon: webhook
---

Discord: island milestones, leaderboard changes, alliances and staff actions, posted to webhooks.

The file is `plugins/uxmSkyblock/modules/discord.conf`. Every default below is the shipped value.

## `discord`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `false` | false sends nothing to Discord. |
| `bot-username` | `UXPLIMA Skyblock` | The name and picture the messages are posted under. |
| `avatar-url` | empty |  |
| `rate-limit-per-second` | `2.0` | The most messages sent each second, across every webhook below. |

## `discord.webhooks`

The webhook each kind of message goes to. An empty one sends nothing of that kind.

| Key | Default | What it does |
|---|---|---|
| `milestones` | empty |  |
| `leaderboards` | empty |  |
| `alliances` | empty |  |
| `admin-audit` | empty |  |

## `discord.embeds`

What each embed says. A `<name>` is replaced with the value the embed carries. A key left out keeps the text shown here. Colours are #RRGGBB.

## `discord.embeds.milestone`

Carries `<island>`, `<leader>`, `<level>`, and `<count>` in members-field.

| Key | Default | What it does |
|---|---|---|
| `title` | `🌟 Island Level Milestone Reached!` |  |
| `description` | `Island **<island>** has achieved Level **<level>**!` |  |
| `leader-field` | `Leader` |  |
| `level-field` | `Island Level` |  |
| `members-field` | `Members (<count>)` |  |
| `footer` | `UXPLIMA Skyblock Milestones` |  |
| `color` | `#FFE66D` |  |

## `discord.embeds.leaderboard`

The title carries `<metric>`. Each line carries `<place>`, `<island>` and `<score>`. `<place>` is a medal for the first three and other-place, which carries `<rank>`, for the rest.

| Key | Default | What it does |
|---|---|---|
| `title` | `🏆 Top Islands Leaderboard: <metric>` |  |
| `line` | `<place> **<island>**: <score>` |  |
| `first-place` | `🥇` |  |
| `second-place` | `🥈` |  |
| `third-place` | `🥉` |  |
| `other-place` | `'#<rank>'` |  |
| `empty` | `No leaderboard entries recorded.` |  |
| `footer` | `UXPLIMA Skyblock Leaderboard` |  |
| `color` | `#FFA07A` |  |

## `discord.embeds.alliance`

Carries `<alliance>`, `<action>`, `<actor>` and `<target>`.

| Key | Default | What it does |
|---|---|---|
| `title` | `⚔️ Diplomatic Event: <action>` |  |
| `description` | `Alliance update concerning **<alliance>**.` |  |
| `alliance-field` | `Alliance` |  |
| `actor-field` | `Initiated By` |  |
| `target-field` | `Target` |  |
| `footer` | `UXPLIMA Skyblock Diplomacy` |  |
| `color` | `#48CAE4` |  |

## `discord.embeds.audit`

The title carries `<severity>` and `<event>`. The colour follows the severity.

| Key | Default | What it does |
|---|---|---|
| `title` | `🛡️ Staff Audit Alert [<severity>]: <event>` |  |
| `footer` | `UXPLIMA Skyblock Administrative Audit` |  |
| `high-color` | `#FF6B8B` |  |
| `medium-color` | `#FFA07A` |  |
| `low-color` | `#8A93A1` |  |
