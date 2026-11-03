---
title: Gallery
order: 407
description: Ratings that resist friends, visits, trending, tags, hiding, and the pinned and sponsored rows.
icon: images
---

`/plot browse` opens the [gallery](../menus/gallery.md): every build, sorted as the viewer asks, with search, an
unrated filter, tag filters and a random visit.

## Ratings

`rating.conf`:

| Key | Default | Meaning |
|---|---|---|
| `must-have-stood-for-seconds` | 30 | How long in a plot before rating it |
| `wait-before-changing-seconds` | 3600 | How long before a rating may change |
| `trending-half-life-days` | 7 | How fast a vote or visit fades from trending |
| `trending-visit-weight` | 0.5 | One fresh visit against one star of one fresh vote |
| `confidence-votes` | 5 | Votes before the average is trusted; 0 uses the plain average |
| `confidence-average` | 3.0 | What a plot is pulled towards until then |
| `categories` | `[]` | Parts a vote is given in |

A rating counts only from a player who stood in the build, so friends rating from spawn do not outrank forty people who
walked round it. The rating order pulls a plot's average towards `confidence-average` until `confidence-votes` people
have said something. With `categories` named, `/plot rate 5` scores every part and `/plot rate part <part> <stars>` one
of them; it is still one vote, at the mean. A part reads the line `category.<id>` of the language file.

## Visits

```hocon
visits {
  count-after-seconds       = 15
  count-again-after-seconds = 3600
  sweep-every-seconds       = 10
}
```

A visit is a different person who stayed 15 seconds, counted again after an hour; the owner and members never count.

## Tags

`catalogues/tags.conf` lists the words a plot may wear, in the order offered, up to `max-per-plot` (4): `medieval`,
`modern`, `fantasy`, `redstone`, `farm`, `interior`, `terrain`, `pixel-art`, `minigame`, `spawn`, `work-in-progress`.
Each has an `id`, a `word`, an `icon` and a `permission` (empty for everybody). A tag removed from the file stops being
offered and plots wearing it are left alone.

## Hiding

`/plot hide` keeps a plot out of the gallery and off every board.

## Pinned and sponsored rows

```hocon
featured {
  pinned-slots       = 9
  sponsored-slots    = 9
  sponsored-hours    = 168
  sponsored-currency = ""
  sponsored-price    = 2500
}
```

Two rows above the ranking. Staff pin a build with `/uxmplots pin`; a player buys a sponsored place with
`/plot sponsor buy`. Neither feeds the ranking and nothing bought moves a build up it. The pinned row reads in the order
staff added; the sponsored row is shuffled on every draw. No currency means nothing is for sale; zero slots turns a row
off.

Related: [Gallery window](../menus/gallery.md), [rating.conf](../config/rating-conf.md)
