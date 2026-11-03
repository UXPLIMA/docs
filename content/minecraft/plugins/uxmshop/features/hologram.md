---
title: The shop hologram
order: 408
description: Text over a player shop, sent as packets, its keys and its tags.
icon: type
---

The text above a shop is sent to the players near it and is never an entity on the server: it is in no entity count,
no region file holds it, an entity sweep cannot reach it and nobody can pick it up. Each line is its own display, the
first highest.

| Key in `player-shops.hologram` | Default | Meaning |
|---|---|---|
| `enabled` | true | Draws it at all |
| `range` | 16.0 | How far away, in blocks, a player is sent it |
| `refresh` | `5m` | How often every shop is read again from the database; a trade redraws its own shop at once |
| `frame` | `500ms` | How often one shop works out who is near and what it says; reads no database |
| `height` | 1.4 | How far above the container the first line sits |
| `lines` | item, owner, `@hologram.offer`, `@hologram.stock` | A player shop |
| `admin-lines` | item, `@hologram.server-shop`, `@hologram.offer` | A server shop |

A line written `@key` is a line of the language files, read in each viewer's language. In a line, `<buy>...</buy>`
disappears when the shop is not selling, `<sell>...</sell>` when it is not buying, and `<separator>...</separator>`
when either did. A line left with nothing in it is not drawn. `<owner>`, `<item>`, `<stock>`, `<price-buy>` and
`<price-sell>` are filled in. A server with very many shops raises `frame`.

Related: [Player shops](player-shops.md)
