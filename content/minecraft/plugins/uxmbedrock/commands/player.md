---
title: Player commands
order: 302
description: Every command a Bedrock player types, with its arguments and node.
icon: user
---

| Command | Does | Node |
|---|---|---|
| `/bedrock` | Opens the [settings form](../menus/settings.md) | `uxmbedrock.settings` |
| `/bedrock menus <form\|chest>` | Other plugins' menus open as forms or as chests | `uxmbedrock.settings` |
| `/bedrock feature <feature> <on\|off>` | Switches one part of the polish for yourself. Only the features in `player-choices.allowed` can be switched | `uxmbedrock.settings` |
| `/bedrock animated <full\|light\|none>` | How much of the animated pack you download, from your next join | `uxmbedrock.settings` |
| `/bedrock lang` | Says the language you read and the ones this server has | `uxmbedrock.language` |
| `/bedrock lang set <language>` | Reads this plugin in one language, whatever your client is set to | `uxmbedrock.language` |
| `/bedrock lang reset` | Follows your client again | `uxmbedrock.language` |
| `/menus` | Switches between forms and chests | `uxmbedrock.menus` |
| `/menus <form\|chest>` | Sets forms or chests | `uxmbedrock.menus` |

All three nodes are given to every player by default.

## Features a player can switch

| Word | Part |
|---|---|
| `java-font` | Java's font |
| `exact-colours` | Exact colours |
| `faces` | Faces in text |
| `tab-list` | The Java player list |
| `java-chat` | Chat laid out as on Java |
| `scoreboard-art` | The sidebar's picture and see-through background |
| `displays` | Furniture and decorations |
| `menu-forms` | Menus as forms |
| `menu-art` | Menu pictures |
| `tooltips` | Item tooltip frames |
| `vanilla-hud` | The Java HUD |
| `hud-plugins` | The server's HUD |
| `animated-pack` | Animated pictures |

The shipped `player-choices.allowed` lists `java-font`, `java-chat`, `menu-art`, `vanilla-hud`, `hud-plugins` and
`animated-pack`. A pack part switched off reaches the player from their next join.
