---
title: Entries
order: 401
description: The two lines and the icon of an entry, the list, and rotation.
icon: list
---

`motd.entries` is a list. Each entry is the two lines a client draws under the server name, and the picture beside them.

| Key | Meaning |
|---|---|
| `line-1` | The first line, MiniMessage |
| `line-2` | The second line |
| `icon` | Optional: a picture in the plugin folder, such as `icons/uxm-glow.png`. Left out, the entry takes `icons.normal` |

`motd.rotation` picks the entry for each ping:

| Rotation | Picks |
|---|---|
| `in-order` | The list from the top, starting again at the end |
| `random` | One at random for every ping |

## The shipped entries

| Line 1 | Line 2 | Icon |
|---|---|---|
| `UXM Network` across the wheel, `• 1.21 - 26.2` | `✦ Season 1 is now live • Join now` | `uxm-mark.png` |
| `UXM Network` across the wheel, `• Minigames and survival` | `★ New updates and rewards • play.uxm.net` | `uxm-glow.png` |

Both are centred. Replace them with your own.

## Tags in a line

| Tag | Value |
|---|---|
| `<online>` | The online count this ping shows |
| `<max>` | The maximum this ping shows |
| `<center>` | At the start: centres the line; see [Centring and colours](centring-and-colours.md) |
| `<g:'NAME':wheel>` | The name painted across the theme's wheel |
| `<plain>...</plain>` | Ordinary letters, not small capitals |

Related: [config.conf](../config/config-conf.md#motd), [Icons](icons.md)
