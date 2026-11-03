---
title: Concepts
order: 203
description: Families, pieces, how a player has a piece, wearing one per family, and what keeps cosmetics harmless.
icon: lightbulb
---

## Families and pieces

A **family** (a category) is one kind of cosmetic, and each has a file under `categories/`. A **piece** is one entry
of that file: its id, its name, its rarity, its price and the keys of its kind.

| Family | Kind | Shipped |
|---|---|---|
| Trails | Particles behind a walking player | 23 |
| Halos | A ring of particles above the head | 15 |
| Cloaks | A cape or wings of particles behind the player | 13 |
| Hats | An item on the head | 20 |
| Suits | Armour pieces, coloured or modelled | 4 |
| Emotes | A face over the head, a burst and a sound, played on click | 20 |
| Balloons | A block on a lead, floating beside the player | 27 |
| Gadgets | A hotbar item that does something on right click | 44 |
| Pets | An entity that walks after the player | 19 |
| Mounts | An entity the player rides and steers | 3 |
| Morphs | The player is hidden and an entity stands where they do | 20 |
| Banners | A patterned banner worn on the head | 26 |

## One piece per family

A player wears at most one piece of each family. Putting on a second trail takes the first off. Twelve families
means up to twelve pieces at once. `/cosmetics unequip all`, or the barrier in the wardrobe, takes everything off
in one write.

## Having a piece

A player has a piece in either of two ways, and both count the same:

| Way | Kept in |
|---|---|
| **Owning it**: bought, won from a box, or granted by staff | The database |
| **Holding its node**: `uxmcosmetics.<family>.<id>`, or the node the piece names | The permission plugin |

`uxmcosmetics.*` gives every piece while `permissions.wildcard` is on. A player who has a piece by node is never
charged for it.

## Rarity

`common`, `uncommon`, `rare`, `epic` and `legendary`. A rarity is a label and a colour on the tile; it changes no
price and no chance. A box tier uses the same five words.

## Money

`economy.currency` names one balance for every price, every box and every duplicate refund. The shipped value is
`exp`, the player's own experience points, so every price works on a bare server.

## Harmless by design

| Rule | Why |
|---|---|
| A cosmetic item cannot be moved, dropped, swapped, placed or traded | A hat that drops is a hat someone else can wear |
| A cosmetic item never drops on death | Same |
| A follower is never saved to the world | A crash cannot leave a pet behind; `cleanup` sweeps one that was |
| A cosmetic mob does not burn, drown, convert, breed, lay, split, teleport, pick up blocks, hunt, trade or get sheared | It is a costume, not a mob |
| A gadget's blast breaks nothing and hurts nobody | The show is drawn instead |
| A gadget's structure is painted on clients only | The world never changes |

## Where a piece is drawn

One shared task, every `animation.tick-ms` (100), draws every particle, moves every follower and animates every
station. That task is the whole cost of the plugin on the server.

Related: [Your first cosmetic](first-cosmetic.md), [Getting cosmetics](../features/getting-cosmetics.md)
