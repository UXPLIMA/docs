---
title: Holograms
order: 42
description: The text above a spawner, what it shows, which hologram plugin draws it and how FancyHolograms displays it.
icon: text-cursor
---

A hologram above each spawner shows its type, level, owner and how full its storage is. uxmSpawners
does not draw holograms itself; it uses whichever supported plugin is installed:

| Plugin | Picked |
|---|---|
| FancyHolograms 2 or 3 | First |
| DecentHolograms | If FancyHolograms is not installed |
| CMI | If neither is |

`modules/hologram.yml`:

```yaml
hologram:
  enabled: true
  hologram-height: 1.5
```

`hologram-height` is how many blocks above the spawner the text floats. With `enabled: true` and none
of the three installed, the console warns at startup and no hologram appears; set it to `false`.
Turning it off with a reload removes every hologram.

## The text

The lines are in the language file, under `lang.hologram`:

```yaml
hologram:
  - "<dark_gray>(<white>{level}x<dark_gray>) <white>{spawner}"
  - ""
  - "<white>Owner <gray>➡ <white>{owner}"
  - "<white>Storage <gray>➡ <white>{storage_bar}"
  - ""
  - "<gray><italic>Right-click to access menu."
```

| Placeholder | Shows |
|---|---|
| `{spawner}` | The spawner's name, as on its item |
| `{level}` | Its level |
| `{owner}` | The owner's name |
| `{storage_bar}` | Ten characters coloured by how full the storage is, from `lang.storage-bar` |

The bar counts every item type together: two types at a limit of 2000 each fill it at 4000.

The hologram is redrawn when the spawner is placed, levelled up or toggled, and from time to time as
it produces, so the bar can lag a little behind the storage.

## Per spawner

The **Hologram** button in the menu turns a spawner's hologram off and on. New spawners start with
it on. The owner and friends can press it.

## FancyHolograms display

With FancyHolograms, the `fancyholograms` section shapes how the text looks:

| Setting | Values |
|---|---|
| `background` | `transparent`, `default`, a colour name such as `black`, or `#RRGGBB` / `#AARRGGBB` |
| `background-opacity` | `0.0` to `1.0`, or `0` to `255`. `-1` keeps the colour's own |
| `billboard` | `center` faces the player, `horizontal` turns sideways only, `vertical` up and down only, `fixed` never turns |
| `text-alignment` | `center`, `left` or `right` |
| `text-shadow` | Shadow under the letters |
| `see-through` | Readable through walls |
| `scale` | Size along `x`, `y` and `z`, `1.0` normal |
| `translation` | Offset in blocks along `x`, `y` and `z` |
| `brightness` | `block` and `sky` light from 0 to 15, so the text stays bright at night. `-1` uses the world's light |
| `shadow-radius`, `shadow-strength` | The shadow on the ground. A radius of `0.0` turns it off |
| `visibility-distance` | Blocks from which it is visible. `-1` uses FancyHolograms' default |
| `update-text-interval` | Ticks between text refreshes by FancyHolograms. `-1` leaves it to uxmSpawners |

DecentHolograms and CMI use their own defaults for how text looks.

Related: [Language and Menus](../config/languages.md), [Integrations](../integrations.md)
