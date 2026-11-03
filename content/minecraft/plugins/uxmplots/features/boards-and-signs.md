---
title: Boards and signs
order: 313
description: The six boards, planting a board in the world, and the sign over every plot.
icon: trophy
---

## Boards

| Board | Ranks by |
|---|---|
| `rating` | The confidence-weighted rating |
| `trending` | Votes and visits, fading by the half life |
| `visits` | Different visitors |
| `tips` | Tips in `leaderboard.tips-currency` (diamonds) |
| `level` | Plot level |
| `honours` | Contest places, worth `honour-points` (5, 3, 2) |

```hocon
leaderboard {
  refresh-seconds = 60
  size            = 10
  tips-currency   = "diamonds"
  honour-points   = [5, 3, 2]
  post-format     = "<muted>#%place% <value>%name% <dim>- <value>%score%"
}
```

A board is the one thing worked out ahead of time, every `refresh-seconds`. A hidden plot is off every board. Honours
never fade. `/plot leaderboard` opens the [boards window](../menus/gallery.md#boards).

## Planted boards

`/uxmplots board plant <board> <name>` stands a board in the world where you stand, written into `boards.conf` with
`post-format`; `/uxmplots board uproot <name>` takes it down.

## The plot sign

`hologram.conf`: a sign over every plot within `personality.sign-radius-plots`, drawn through uxmLib.

| Key | Default |
|---|---|
| `enabled` | true |
| `height` | 2.5 |
| `lines` | the name, the owner, rating, visits, tags, honours, tips, and the pinned guestbook note |
| `themes` | `billboard` (fixed, 1.6), `plain` (0.7, no shadow), `far` (view range 2.5, 1.3) |
| `appearance` | `billboard` CENTER, no see-through, text shadow, line width 200, view range 1, scale 1 |

A plot's owner picks a theme with `/plot feel hologram`, and turns the sign off with `/plot flag hologram false`. A
sign exists only near somebody who can see it, so their number follows the players online, not the size of the estate.
It hangs over the middle of the plot's low z edge. A line whose words all come back empty is not drawn. The lines read
the `%plot_...%` tokens.

Related: [Placeholders](../placeholders/reference.md)
