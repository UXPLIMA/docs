---
title: Your first cosmetic
order: 104
description: Adding a halo of your own, selling it, putting it in a box and placing a box station.
icon: footprints
---

This walkthrough adds a gold halo, sells it, puts it in a box and places a station for the box.

## 1. Add the piece

Open `categories/halo.conf` and add an entry to the `cosmetics` list:

```hocon
  {
    id     = "sun_halo"
    name   = "Sun Halo"
    description = "A ring of gold light over your head."
    rarity = "rare"
    price  = 1200.0
    icon   = "GOLD_BLOCK"
    particle { type = "DUST", color-from = "#ffcc33", size = 1.0, count = 1 }
    pattern   = "HALO"
    radius    = 0.4
    points    = 8
    height    = 2.05
    period-ms = 100
  }
```

The id is lower case letters, digits, hyphens and underscores; it is stored in the database and is part of the node,
so never change it once players own the piece.

## 2. Load it

Run `/uxmcosmetics reload`. The console names the piece if a line did not parse. `/uxmcosmetics version` counts the
cosmetics now loaded.

## 3. Buy it

Open `/cosmetics`, click **Halos**, and click **Sun Halo**. The price is taken in experience points and the halo is
on. A second click takes it off; the next click puts it back without charging again.

## 4. Put it in a box

In `boxes.conf`, add a line to a box's `rewards`:

```hocon
{ cosmetic = "sun_halo", weight = 10, duplicate-coins = 300.0 }
```

A player who already owns it gets 300 back instead.

## 5. Let players open the box

A box needs its node, `uxmcosmetics.box.<id>`, and no player holds one by default:

```
lp group default permission set uxmcosmetics.box.common true
```

## 6. Place a station

Build a chest in the lobby, look at it and run `/uxmcosmetics station add common`. A ring of sparks turns over the
chest, a sign floats above it, and a right click opens the common box.

## Next

- Read every key of a family on [Category files](../config/category-files.md).
- Change the currency to Vault on [Economies](../integrations/economies.md).
- Show the worn halo on a scoreboard with `%uxmcosmetics_equipped_halo%`.

Related: [Mystery boxes](../features/mystery-boxes.md), [Box stations](../features/stations.md)
