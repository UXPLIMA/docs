---
title: House
order: 402
description: The shelf, sorting, tabs, search, your listings and the mailbox.
icon: store
---

`/auction`, node `uxmauction.use`. Six rows.

| Slot | Holds | Click |
|---|---|---|
| 0 to 44 | The listings, one page, as their own items | Opens the [listing](listing.md) |
| 22 | A cobweb, when nothing is listed | |
| 45, 53 | Arrows | The page |
| 46 | A bookshelf | The [categories](category.md) |
| 47 | A writable book | [Your listings](mine.md) |
| 48 | A hopper | Left: the next sort; right: the previous |
| 49 | An oak sign | Closes the window and asks for search words in chat |
| 50 | An ender chest | The [mailbox](mailbox.md) |

## Sorting

`newest`, `oldest`, `price-low`, `price-high` and `ending-soon`. A file may name one with `auction:sort:<name>`.

## Search

The sign closes the window and waits for the player to write in chat, for `asking.wait-for` (60s). Writing
`asking.stop-word` (`cancel`) stops it. A Bedrock client writes in chat like a Java one.

Related: [Listing](listing.md), [Category](category.md)
