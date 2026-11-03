---
title: Guards and regions
order: 305
description: Holding an item back while another plugin says the player is busy, and giving it only in some regions.
icon: shield
---

## Guards

`skip-when` is a list of PlaceholderAPI texts. Any one of them that answers with something means the player is busy,
and the item is not handed out until it answers with nothing again.

```hocon
skip-when = ["%uxmkotl_arena%", "%uxmparkour_arena%"]
```

An answer of nothing, `0`, `-`, `false` or `no` means not busy. A placeholder no plugin on the server fills is passed
over, not obeyed. Guards need PlaceholderAPI.

A typical use: hide the lobby hotbar while a player runs a parkour course or fights on the hill.

## Regions

`regions` is a list of WorldGuard regions the item is handed out in, matched without case. An empty list means
everywhere, and a server with no WorldGuard never consults a region at all.

```hocon
regions = ["spawn"]
```

Related: [Concepts](../getting-started/concepts.md#before-an-item-is-placed)
