---
title: Messages
order: 506
description: How a line is written, the prefixes and the blocks of keys.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default language.

| Prefix | Draws |
|---|---|
| `<tag:'SHOP'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |

| Block | Said when |
|---|---|
| `admin.*` | The operator commands, the doctor, logs, modifiers |
| `error.*` | Refusals |
| `content.*` | The shipped sections' words |
| `hologram.*` | The shop hologram lines |
| `shop.*` | Trades: the `effects` lines |
| `sell.*`, `value.*` | Those commands |
| `myshop.*`, `report.*` | Player shops and the profit report |
| `log.*` | `/uxmshop logs` |
| `state.*` | How a line trades |
| `menu.*` | Every window's titles, tiles and buttons |
| `language.*`, `command.*` | Languages, the command layer |
| `currency.*` | How each shipped currency is named |

Related: [Languages](languages.md), [theme.conf](theme.md)
