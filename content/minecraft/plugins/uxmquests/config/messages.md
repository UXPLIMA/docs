---
title: Messages
order: 506
description: How a line is written, the prefixes and the blocks of keys.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). Ordinary words are converted into small capitals when the line is drawn. A key
that is missing falls back to the default language.

| Prefix | Draws |
|---|---|
| `<tag:'QUESTS'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Said when |
|---|---|
| `admin.*` | The operator commands, the doctor, the top of a pass |
| `board.*` | The board in chat |
| `start.*`, `claim.*`, `reroll.*` | Those commands |
| `pass.*` | Passes, tiers, seasons, the shipped pass lines |
| `group.*`, `quest.*` | The shipped groups and quests |
| `error.*`, `language.*`, `command.*` | Errors, languages, the command layer |
| `menu.*` | Every window's titles, tiles and buttons |
| `feedback.*` | The four moments |
| `task.*`, `task-any.*` | A task with no description |
| `vanilla.*` | Block, item and mob names |

Message tags filled inside these lines: `<quest>`, `<group>`, `<done>`, `<needed>`, `<percent>`, `<pass>`, `<tier>`,
`<tiers>`, `<xp>` and `<time>`.

Related: [Languages](languages.md), [theme.conf](theme.md)
