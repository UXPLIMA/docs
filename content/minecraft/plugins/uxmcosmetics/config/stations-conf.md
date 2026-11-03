---
title: stations.conf
order: 604
description: The box stations, written by the plugin, and every key of one.
icon: map-pin
---

The plugin writes this file from `/uxmcosmetics station add`, `set` and `remove`. You may edit it by hand and run
`/uxmcosmetics reload`.

| Key | Meaning |
|---|---|
| `id` | The station's name, unique |
| `box` | A box id, or `all` |
| `world`, `x`, `y`, `z` | The block |
| `radius` | How far from the block a click counts |
| `effect.particle`, `pattern`, `radius`, `height`, `period-ms` | The figure: `HALO`, `ORBIT`, `HELIX` or `TRAIL` |
| `effect.sound` | For the user; empty for silence |
| `effect.use-particle` | A burst on use; empty for none |
| `hologram.height`, `lines` | The sign; `[]` for none. Lines are MiniMessage with theme roles; `{box}` is the box's name |

The sign's look is `station.hologram.appearance` in [config.conf](config-conf.md#station), one look for every station.

Related: [Box stations](../features/stations.md)
