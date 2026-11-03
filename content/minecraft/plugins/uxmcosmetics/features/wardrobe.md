---
title: The wardrobe
order: 301
description: Wearing one piece per family, what a click does in each state, taking things off, and what happens at join and quit.
icon: shirt
---

The wardrobe is `/cosmetics`, or a right click on the [opener](hotbar-items.md#the-opener). It shows the twelve
families with how many pieces of each the player has; a click opens one family.

## One per family

A player wears at most one piece of each family. Putting a piece on stops whatever was worn in that family first, so
two pets never walk behind one player.

## What a click does

In a family window, a click on a piece does one thing, by its state:

| State | Click |
|---|---|
| Worn now | Takes it off |
| Yours (owned or by node) | Puts it on |
| Yours, and an emote | Plays it |
| For sale | Buys it; it is put on at the next click |
| Not for sale | Nothing can be done: it comes from a box, a grant or a node |

The window closes on the click and the answer arrives in chat with the `effects` of that moment.

## Taking everything off

The barrier in the wardrobe, or `/cosmetics unequip all`, takes every piece off in one write. A family whose piece was
removed from its file since it was put on is still cleared.

## Join and quit

The wardrobe is read before the player is let in, so a join costs no database trip. What the player wore is put back
on at join, after the node or ownership is checked again: a piece revoked or a node removed while they were away is
not restored. At quit every follower and item of theirs is taken away; nothing is written.

## The effects of a moment

`effects` in [config.conf](../config/config-conf.md#effects) lists what each moment does, as action lines:

| Moment | Shipped |
|---|---|
| `equipped` | `equip.done` and an amethyst chime |
| `unequipped` | `unequip.done` |
| `unequipped-all` | `unequip.all` |
| `bought` | `buy.done` and a level-up sound |
| `box-won` | `box.won` and a level-up sound |

Write `[]` to silence a moment.

Related: [Wardrobe window](../menus/wardrobe.md), [Family window](../menus/category.md)
