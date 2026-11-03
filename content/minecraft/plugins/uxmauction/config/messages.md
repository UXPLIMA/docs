---
title: Messages
order: 606
description: How a line is written, the prefixes, and the blocks of keys.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default the code carries.

| Prefix | Draws |
|---|---|
| `<tag:'AUCTION'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Said when |
|---|---|
| `admin.*` | The operator commands, the doctor |
| The moments of `effects` | `@line` reads the line the house chose for each outcome |
| Refusals | One line each: a listing gone, not enough money, a currency not ready, a price out of the band |
| `menu.<window>.*` | Every window's words and tiles |
| `currency.*` | How the shipped currencies write an amount: `{amount} coins`, `{amount} coin`... |
| `command.*` | The command layer |
| `language.*` | `/auction lang` |

Related: [Languages](languages.md), [theme.conf](theme.md)
