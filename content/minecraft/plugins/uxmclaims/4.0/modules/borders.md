---
title: Borders
order: 1310
description: Particle walls along the edges of the land you stand in, shown on request or after a claim.
icon: square-dashed
---

The borders module draws a wall of coloured particles along the edges of the land you stand in, or around the
wilderness chunk you stand in. `/claim borders` turns the wall on and off, and the wall follows you as you walk
from land to land. Only you see your own walls.

Module `borders` · enabled by default · `modules/borders/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim borders`, `/claim showborders` | Shows or hides the borders. | `uxmclaims.command.borders.show` |
| `/claim borders stay` | Keeps the borders shown until you turn them off, also after a relog. Run it again to stop. | `uxmclaims.command.borders.stay` |
| `/claim borders off` | Hides the borders, including a `stay` wall. | `uxmclaims.command.borders.show` |

The local names of these commands are in `modules/borders/lang/<language>/commands.yml`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.borders.show` | everyone | `/claim borders`, `/claim showborders` and `/claim borders off`. |
| `uxmclaims.command.borders.stay` | everyone | `/claim borders stay`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `auto-close-seconds` | `30` | Seconds before the borders hide again; `0` keeps them until turned off. Walking into another chunk restarts the time. |
| `show-on-claim` | `true` | Show a land's borders to the player who just created it or claimed or unclaimed one of its chunks. |
| `interval-ticks` | `5` | Ticks between two particle draws (20 is one second). |
| `max-distance` | `64` | Only edges this many blocks from the player are drawn, so big lands stay cheap. |
| `wall.below` | `2` | Blocks below the player's feet where the wall starts. |
| `wall.above` | `10` | Blocks above the player's feet where the wall ends. |
| `wall.row-spacing` | `2` | Vertical gap between two particle rows, in blocks. |
| `wall.particles-per-block` | `2` | Particles per block along an edge. |
| `wall.particle-size` | `1.0` | Particle size, `0.5` to `4.0`. |
| `colors.own` | `'#24FF00CB'` | Lands you own or are a member of. |
| `colors.foreign` | `'#FC0606CB'` | Other players' lands. |
| `colors.wilderness` | `'#15BDEFB3'` | The unclaimed chunk you stand in. |

Colours are `#RRGGBB` or `#RRGGBBAA`; particles ignore the alpha.

## Notes

- The wall is drawn at your own height, from `wall.below` under your feet to `wall.above` over them, so it
  moves up and down with you.
- `stay` is kept as a player setting in the database. `/claim borders` while `stay` is on turns both off.
- A wall shown with plain `/claim borders` is held in memory and ends at quit or restart.
- Only particle walls exist. The 3.x display-entity walls are not in 4.0.
- `/uxm reload` applies new colours and sizes to walls already shown.

Related: [Lands](lands.md), [Notifications](notifications.md)
