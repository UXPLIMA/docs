---
title: Messages
order: 616
description: How a line is written, the prefixes and the blocks of keys.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). Ordinary words are converted into small capitals when the line is drawn. A key
that is missing falls back to the default the code carries; delete a line to take the default back.

| Prefix | Draws |
|---|---|
| `<tag:'PLOTS'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |

| Block | Said when |
|---|---|
| `admin.*` | The operator commands, the doctor, reports, inactive plots, pre-generation, the bench |
| `plot.*` | Every `/plot` branch, the plot sign lines, flag names |
| `language.*` | Languages |
| `menu.*` | Every window's titles, tiles and buttons |
| `contest.*` | Contests, teams, voting, results |
| `world.*` | The world editor |
| `refused.*` | Protection refusals |
| `shop.*`, `catalogue.*`, `currency.*` | Buying and the catalogue words |
| `command.*` | The command layer |
| `time.*` | Durations |
| `effect.*` | The shipped effect lines: `effect.report-right`, `effect.follow.*`, `effect.vote.*` |
| `gui.*` | Shared window words |
| `category.<id>` | A rating category's word |
| `feel-kind.*`, `sort-word.*`, `contest-phase.*`, `contest-mode.*`, `board-name.*` | Words for kinds, orders, phases, modes and boards |

Related: [Languages](languages.md), [theme.conf](theme.md)
