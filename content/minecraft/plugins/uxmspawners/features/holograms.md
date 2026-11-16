---
title: Holograms
order: 142
description: The text above a spawner, what it shows, how it looks, a look per spawner type, and what it needs to run.
icon: text-cursor
---

A hologram above each spawner shows its name, level and owner. uxmSpawners draws holograms itself,
as packets: nothing is saved in the world and no hologram plugin is needed. They need the
[PacketEvents](https://modrinth.com/plugin/packetevents) plugin. Without it the plugin starts with a
warning and holograms stay off.

## hologram.yml

```yaml
enabled: true

see-through-blocks: false
shadow: true
text-opacity: 255
background-color: "#644C4F4B"
brightness: -1
view-distance: 64
height: 1.5
scale: 1, 1, 1
translation: 0, 0, 0
billboard: CENTER
text-alignment: CENTER

content:
  - "<spawner> <#5F6778>· <#A99BF0>Lv. <level>"
  - "<#9AA3B5>Owner <#DCE1EA><owner>"
  - "<#7AD7C9>▸ <#DCE1EA>Right-click <#9AA3B5>to open"
```

| Setting | Meaning |
|---|---|
| `enabled` | `false` removes every hologram and hides the **Hologram** button |
| `see-through-blocks` | Readable through walls |
| `shadow` | Shadow under the letters |
| `text-opacity` | `26` to `255`, `255` fully opaque |
| `background-color` | `0` for none, `#RRGGBB`, `#AARRGGBB` with the opacity first, or an ARGB number |
| `brightness` | `0` to `15` keeps the text that bright at night. `-1` uses the world's light |
| `view-distance` | How far players see it, in blocks: 8 to 256 |
| `height` | Blocks from the bottom of the spawner to the bottom of the text: `1.5` is half a block above it |
| `scale` | Size along x, y and z, `1, 1, 1` normal, `2, 2, 2` twice as big |
| `translation` | Offset in blocks along x, y and z, from where `height` puts it |
| `billboard` | `CENTER` faces the player, `VERTICAL` turns left and right only, `HORIZONTAL` tilts up and down only, `FIXED` never turns |
| `text-alignment` | `CENTER`, `LEFT` or `RIGHT` |

Changes apply with `/usp reload`, `enabled` included. Installing PacketEvents needs a restart.

## The text

`content` holds the lines, in MiniMessage. They are the same for every language.

| Placeholder | Shows |
|---|---|
| `<spawner>` | The spawner's name, as on its item |
| `<level>` | Its level |
| `<owner>` | The owner's name |
| `<xp_storage_bar>`, `<xp_percentage>` | How full the XP Storage is, as a bar and in percent |
| `<xp_color>` | The colour of how full the XP Storage is, for the text after it |

The item storage has no bar of its own here: its limit is per item, so it has no single "full". The
bar's length, character and colours are `storage-bar` in the language file.

## A look per spawner type

A spawner file's `hologram` section sets any of the settings above, `content` included, differently
for that type's spawners. `enabled` and `view-distance` stay `hologram.yml`'s.

```yaml
hologram:
  height: 2
  background-color: 0
  content:
    - "<spawner> <#5F6778>· <#A99BF0>Lv. <level>"
```

A spawner whose file is missing or turned off looks like `default.yml`'s. See
[Spawner Types](../config/spawner-types.md#hologram-its-own-look).

## Per spawner

The **Hologram** button in the Settings menu turns a spawner's hologram off and on. New spawners
start with it on. Only the owner can press it.

## Cost

A hologram is made when a player first comes near and taken down when nobody sees it. It is redrawn
only when what it shows has changed, and only for players near enough to see it. Changes from
production are gathered for a second before they are drawn. A server with thousands of spawners pays
for the ones players are standing near.

Related: [Language and Menus](../config/languages.md), [Integrations](../integrations.md)
