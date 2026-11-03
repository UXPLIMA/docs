---
title: Your first glow
order: 204
description: Giving ranks their colours, letting a rank cycle, and changing the picker.
icon: footprints
---

## 1. Give the colours out

A new player may open the picker and sees every colour, and may use the ones you granted. One node per colour, so a
rank can carry two colours and another rank twelve:

```
lp group vip permission set uxmglow.colour.aqua true
lp group vip permission set uxmglow.colour.gold true
lp group mvp permission set uxmglow.colour.* true
lp group mvp permission set uxmglow.cycle true
```

There is no list of ranks anywhere in the plugin. It reads the node a player holds, and you decide which rank holds
which node. `uxmglow.colour.*` works through your permission plugin's wildcard; the plugin itself declares it without
children.

## 2. Try it as a player

`/glow` opens the picker: a free colour reads **Ready**, a locked one **Locked**. `/glow set aqua` skips the picker.
`/glow cycle red,blue,gold 400` steps through three colours every 400 milliseconds, `/glow off` stops, and
`/glow colours` lists what the player may use.

## 3. Change the picker

`menus/glow.conf` is the window: the rows, the slot every colour sits in, the material of each square, the sound a
click makes, and which lines of the language file each tooltip reads. Edit the file and run `/uxmglow reload`.

Delete the file and this server has no picker: `/glow` answers `menu.missing` in chat and every other command still
works. A window is a file, so an operator can take one away.

## 4. Show it on a scoreboard

With PlaceholderAPI, `%uxmglow_colour%` is the colour's name and `%uxmglow_tag%` its MiniMessage tag:
`%uxmglow_tag%%player_name%` writes the name in the glow's colour.

Related: [Player commands](../commands/player.md), [Picker](../menus/picker.md)
