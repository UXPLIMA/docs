---
title: Web Maps
order: 1341
description: Lands drawn on dynmap, BlueMap, Pl3xMap and squaremap, each owner kind on its own layer.
icon: map
---

The webmap module draws every land on the web map plugins you have installed: dynmap, BlueMap, Pl3xMap and
squaremap, all at the same time. A land's chunks merge into one outline, and lands go on separate layers for
players, towns, nations, outposts and admin lands, each with its own colours and popup.

Module `webmap` · enabled by default · `modules/webmap/config.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm webmap status` | Shows which web maps uxmClaims draws on. | `uxmclaims.admin.webmap` |
| `/uxm webmap refresh` | Redraws every land within a few seconds. | `uxmclaims.admin.webmap` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.webmap` | op | `/uxm webmap status` and `/uxm webmap refresh`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `update-interval-seconds` | `5` | How often changed lands are redrawn. Changes are collected and drawn together, off the main thread. |
| `shape` | `merged` | `merged`: neighbouring chunks of a land form one outline. `per-chunk`: one square per chunk (3.x look). |
| `popup-on-click` | `false` | Pl3xMap and squaremap show the popup on click instead of hover. dynmap and BlueMap always use click. |
| `date-format` | `yyyy-MM-dd` | How `<created>` is written in popups, a Java date pattern. |
| `server-owner-name` | `Server` | `<owner>` for admin lands. |
| `providers.dynmap.enabled` | `true` | Feed dynmap when it is installed. |
| `providers.bluemap.enabled` | `true` | Feed BlueMap when it is installed. |
| `providers.bluemap.extrude` | `true` | Draw lands as 3D columns over the whole world height. `false` draws flat shapes. |
| `providers.bluemap.flat-y` | `64` | Height of flat shapes when `extrude` is off. |
| `providers.pl3xmap.enabled` | `true` | Feed Pl3xMap when it is installed. |
| `providers.squaremap.enabled` | `true` | Feed squaremap when it is installed. |

Each layer under `layers` (`lands`, `towns`, `nations`, `outposts`, `admin`) has the same keys:

| Key | Default (`lands`) | What it does |
|---|---|---|
| `layers.<layer>.enabled` | `true` | Draw this layer. Turn it off when another module draws its own. |
| `layers.<layer>.label` | `Claims` | Name in the map's layer list. |
| `layers.<layer>.priority` | `10` | Order in the layer list; higher is drawn on top. |
| `layers.<layer>.hidden` | `false` | Start with the layer switched off in the map's layer list. |
| `layers.<layer>.marker-label` | `<name>` | Marker name, plain text with the popup placeholders. |
| `layers.<layer>.style.border-color` | `#33FF33` | Outline colour. |
| `layers.<layer>.style.border-opacity` | `0.8` | Outline opacity, 0 to 1. |
| `layers.<layer>.style.border-weight` | `2` | Outline width. |
| `layers.<layer>.style.fill-color` | `#33FF33` | Fill colour. |
| `layers.<layer>.style.fill-opacity` | `0.35` | Fill opacity, 0 to 1. |
| `layers.<layer>.tooltip.lines` | name, owner, chunks, members, created | Popup lines, as HTML. |

The shipped labels, priorities and colours per layer:

| Layer | Holds | Label | Priority | Colour |
|---|---|---|---|---|
| `lands` | Lands of players | Claims | 10 | `#33FF33` |
| `outposts` | Outpost lands | Outposts | 15 | `#F59E0B` |
| `towns` | Lands owned by a town | Towns | 20 | `#3B82F6` |
| `nations` | Lands owned by a nation | Nations | 30 | `#A855F7` |
| `admin` | Admin lands and lands owned by the server | Admin lands | 40 | `#EF4444` |

Popup lines and marker labels take `<name>`, `<owner>`, `<owner-type>`, `<kind>`, `<chunks>`, `<members>`,
`<created>` and `<world>`. Values are escaped, so a land name cannot inject HTML into the page.

## Notes

- The module looks for map plugins one second after the server has started, so a map plugin that loads after
  uxmClaims is still found. Changing `providers` needs a restart; `/uxm reload` redraws everything with the
  new layer settings.
- The [Towns](towns.md) and [Nations](nations.md) modules can draw their own `towns` and `nations` layers with
  the town's or nation's name and colours in the popup. They take the layer only after you set
  `layers.towns.enabled` or `layers.nations.enabled` to `false` here.
- `/uxm webmap status` names no map when none is installed or every installed one is switched off in
  `providers`.

Related: [Towns](towns.md), [Nations](nations.md), [Lands](lands.md), [Integrations](../integrations.md)
