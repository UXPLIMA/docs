---
title: Operator commands
order: 303
description: Every word of /uxmbedrock, with what it says and the node it needs.
icon: shield
---

| Command | Does | Node |
|---|---|---|
| `/uxmbedrock` | Says where the rest of the command is written | `uxmbedrock.admin.version` |
| `/uxmbedrock help` | Lists every word you may use | |
| `/uxmbedrock reload` | Reads every file again; a failed reload keeps the last good settings | `uxmbedrock.admin.reload` |
| `/uxmbedrock version` | The version this server runs | `uxmbedrock.admin.version` |
| `/uxmbedrock doctor` | One line per thing that can be wrong: Floodgate, the Bedrock players online, the pack | `uxmbedrock.admin.doctor` |
| `/uxmbedrock status` | Floodgate and Geyser, where the pack goes, the network, the pack's size and build time or the reason it failed, every switch | `uxmbedrock.admin.status` |
| `/uxmbedrock pack build` | Builds the Bedrock pack now, whether a source changed or not | `uxmbedrock.admin.pack` |
| `/uxmbedrock pack report` | The first 15 lines of the last build's report; the rest is in `output/report.txt` | `uxmbedrock.admin.pack` |
| `/uxmbedrock pack sync` | Offers the build to the proxy again; it takes only the files it lacks | `uxmbedrock.admin.pack` |
| `/uxmbedrock items unknown` | The item models the last build found no vanilla item for | `uxmbedrock.admin.pack` |
| `/uxmbedrock skulls` | How many custom heads the servers have seen | `uxmbedrock.admin.pack` |
| `/uxmbedrock menus` | The rules of menus.conf and the default | `uxmbedrock.admin.menus` |
| `/uxmbedrock preview` | The link of the [preview page](../features/preview.md), while it runs | `uxmbedrock.admin.gui` |
| `/uxmbedrock gui measure` | Measures where the picture of the next menu you open sits, writes it to gui.conf and builds again | `uxmbedrock.admin.gui` |
| `/uxmbedrock gui list` | The menu pictures the pack draws and where gui.conf places each | `uxmbedrock.admin.gui` |
| `/uxmbedrock lang set-player <player> <language>` | Sets the language another player reads; one who is away reads it at their next join | `uxmbedrock.admin.language` |
| `/uxmbedrock lang reset-player <player>` | Clears the language another player chose | `uxmbedrock.admin.language` |

`uxmbedrock.admin.*` grants every node above; every one of them is given to operators by default.
