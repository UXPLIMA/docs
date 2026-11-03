---
title: Catalogues
order: 608
description: shop.conf, personality.conf and tags.conf.
icon: book-open
---

| File | Holds | Entry keys |
|---|---|---|
| `catalogues/shop.conf` | Prices for the world's patterns, and tags | `word`, `icon`, `permission`, `price` (`currency`, `amount`) |
| `catalogues/personality.conf` | Music, ambience, time, weather, sign themes | `value`, `word`, `icon`, `permission`, `price`; music also `loop-seconds` |
| `catalogues/tags.conf` | The words a plot may wear, and `max-per-plot` | `id`, `word`, `icon`, `permission` |

The shipped entries are in [Personality and shop](../features/personality-and-shop.md) and
[Gallery](../features/gallery.md#tags). A shop entry's name matches the pattern's name in the world file.

Related: [Shop](../menus/shop.md)
