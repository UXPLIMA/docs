---
title: Mystery boxes
order: 303
description: Boxes, their tables and weights, the roulette, duplicates, refunds, and who may open a box.
icon: package-open
---

A mystery box costs its `price`, draws one piece from a weighted table, and gives it. Boxes are written in
[`boxes.conf`](../config/boxes-conf.md) and opened from `/cosmetics boxes` or a [box station](stations.md).

## The shipped boxes

| Box | Tier | Price | Table |
|---|---|---|---|
| `common` | common | 1000 | flame_trail 30, pumpkin_crown 25, glass_sphere 25, emote_smile 20, emote_heart 20, chicken_pet 12, pig_pet 12, lapis_balloon 8 (duplicate 200), cloud_trail 5 (duplicate 300) |
| `rare` | rare | 5000 | heart_trail 20, crit_trail 20, flower_crown 18, fox_pet 15, nyan_sheep 12, sun_banner 10, emote_cool 8 (duplicate 1200), gold_balloon 4 (duplicate 2000), ocean_suit 3 (duplicate 2500) |
| `legendary` | legendary | 20000 | totem_trail 10, gold_crown 10, beacon_balloon 12, panda_banner 12, enderman_pet 10, rainbow_suit 10, infernal_horror 8 (duplicate 4000), emote_angel 8 (duplicate 4000) |

A piece's chance is its weight over the sum of the box's weights. A weight of 0 is never drawn.

## Opening

1. The node is checked: `uxmcosmetics.box.<id>`, or the `permission` the box names.
2. The price is taken.
3. One entry is drawn.
4. A new piece is written to the wardrobe; one the player owns pays coins back instead.
5. The [roulette](../menus/roulette.md) spins and lands on that piece.

The prize is paid for and written before the first frame, so a player who closes the roulette keeps what they won.

<Callout type="warning" title="No player may open a box by default">

A box needs `uxmcosmetics.box.<id>`. No player holds one by default, and `uxmcosmetics.box.*` declares no children,
so it grants nothing on its own. Grant each box's node, such as
`lp group default permission set uxmcosmetics.box.common true`, or give the box a `permission` your players hold.

</Callout>

## Duplicates

A piece the player already owns pays coins instead: the entry's `duplicate-coins`, or when it writes none,
`economy.duplicate-refund-fraction` (0.25) of the piece's own price. A piece held only by node is not owned, so it can
still be won.

## Answers

| Answer | When |
|---|---|
| `box.won` | A new piece |
| `box.duplicate` | Coins back, with the amount |
| `box.not-permitted` | No node |
| `box.no-economy` | The currency's plugin is not on the server |
| `box.not-enough` | With the price and the balance |
| `box.broken` | The drawn entry names a piece no file defines. The price is refunded |

A box with no entry, or whose weights are all 0, is refused when the file is read and named in the console.

Related: [boxes.conf](../config/boxes-conf.md), [Boxes window](../menus/boxes.md), [Box stations](stations.md)
