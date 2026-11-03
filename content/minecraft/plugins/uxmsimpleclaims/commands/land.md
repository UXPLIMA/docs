---
title: Land
order: 202
description: Claiming, giving up, merging, splitting, seeing and moving.
icon: map
---

| Command | Turkish | Does |
|---|---|---|
| `/claim` | | Claims the chunk you stand in, or opens the menu of the claim you stand in (its info, for a visitor) |
| `/claim here [name]` | `burasi` | Claims the chunk you stand in, with a name for a new claim |
| `/claim radius <radius>` | `yaricap` | Claims the square around you, up to `claims.max-radius` |
| `/claim auto [mode]` | `oto` | Claims as you walk; `unclaim` gives land up as you walk; run again to stop. Needs `uxmsimpleclaims.auto` |
| `/claim unclaim` | `birak` | Gives up the chunk you stand in, unless that would cut the claim in two; the last chunk asks again |
| `/claim delete` | `sil` | Deletes the claim you stand in, after you type it again within ten seconds |
| `/claim merge <claim>` | `birlestir` | Joins another of your claims that touches this one |
| `/claim split <name>` | `bol` | Cuts off the side of the claim you face, the row you stand in included, as a new claim |
| `/claim info` | `bilgi` | Says whose land you stand on |
| `/claim map` | `harita` | Opens the [map](../menus/claims.md#map) of the land around you |
| `/claim border` | `sinir` | Shows the claim's edges for a few seconds |
| `/claim menu` | `menu` | Opens the [claims window](../menus/claims.md) |
| `/claim list` | `liste` | Lists your claims and the ones you are in |
| `/claim tp <claim>` | `git` | Teleports to a claim you are in, or to a public warp |
| `/claim inbox` | `kutu` | Reads what happened while you were away |
| `/claim favourite` | `favori` | Keeps the claim you stand in among your favourites |
| `/claim lang`, `lang set <language>`, `lang reset` | `dil`, `sec`, `sifirla` | Your language. Needs `uxmsimpleclaims.language` |
| `/claim lang set-player <player> <language>`, `lang reset-player <player>` | `oyuncusec`, `oyuncusifirla` | Another player's language, applied at their next join when they are away. Needs `uxmsimpleclaims.admin.language` |

Related: [Claiming](../features/claiming.md)
