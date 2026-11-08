---
title: Holograms
order: 42
description: The text above a spawner, what it shows, how it looks, and what it needs to run.
icon: text-cursor
---

A hologram above each spawner shows its type, level, owner and how full its storage is. uxmSpawners
draws holograms itself, as packets: nothing is saved in the world and no hologram plugin is needed.
They need the [PacketEvents](https://modrinth.com/plugin/packetevents) plugin. Without it the plugin
starts with a warning and holograms stay off.

## hologram.yml

```yaml
enabled: true

see-through-blocks: false
shadow: true
text-opacity: 255
background-color: "#644C4F4B"
brightness: -1
view-range: 1
height: 1.5
scale: 1, 1, 1
translation: 0, 0, 0
billboard: CENTER
text-alignment: CENTER

content:
  - "<spawner> <#8A93A6>· <#FFD166>Lv. <level>"
  - "<#7FD1FF>Owner <#FFFFFF><owner>"
  - "<storage_bar> <#9EF0B0><storage_percentage>%"
  - "<#FFB86B>Right-click to open"
```

| Setting | Meaning |
|---|---|
| `enabled` | `false` removes every hologram and hides the **Hologram** button |
| `see-through-blocks` | Readable through walls |
| `shadow` | Shadow under the letters |
| `text-opacity` | `26` to `255`, `255` fully opaque |
| `background-color` | `0` for none, `#RRGGBB`, `#AARRGGBB` with the opacity first, or an ARGB number |
| `brightness` | `0` to `15` keeps the text that bright at night. `-1` uses the world's light |
| `view-range` | How far players see it: `1` is 64 blocks |
| `height` | Blocks above the spawner |
| `scale` | Size along x, y and z, `1, 1, 1` normal |
| `translation` | Offset in blocks along x, y and z |
| `billboard` | `CENTER` faces the player, `HORIZONTAL` turns sideways only, `VERTICAL` up and down only, `FIXED` never turns |
| `text-alignment` | `CENTER`, `LEFT` or `RIGHT` |

Changes apply with `/usp reload`, `enabled` included. Installing PacketEvents needs a restart.

## The text

`content` holds the lines, in MiniMessage. They are the same for every language.

| Placeholder | Shows |
|---|---|
| `<spawner>` | The spawner's name, as on its item |
| `<level>` | Its level |
| `<owner>` | The owner's name |
| `<storage_bar>`, `<storage_percentage>` | How full the item storage is, as a bar and in percent |
| `<xp_storage_bar>`, `<xp_percentage>` | The same for XP |

The item bar counts every item type together: two types at a limit of 2000 each fill it at 4000.
Its length, character and colours are `storage-bar` in the language file.

## Per spawner

The **Hologram** button in the Settings menu turns a spawner's hologram off and on. New spawners
start with it on. Only the owner can press it.

## Cost

A hologram is redrawn only when what it shows has changed, and only for players near enough to see
it. Changes from production are gathered for a second before they are drawn, so the bar can lag that
much behind the storage. A server with thousands of spawners pays for the ones players are standing
near.

Related: [Language and Menus](../config/languages.md), [Integrations](../integrations.md)
