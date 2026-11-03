---
title: Concepts
order: 103
description: The bow and its arrow, the three checks a shot passes, the cooldown, the fall protection, and moments.
icon: lightbulb
---

## The bow and the arrow

The plugin hands out two items: the **bow**, and one **arrow** that only exists because a bow cannot be drawn outside
creative mode without one. The shot never spends the arrow. Both are found again by a hidden tag on the item, never
by their name, so rewording the bow in the messages files changes nothing about how it works. A bow without the tag,
such as one from a crafting table, is an ordinary bow.

Both are **locked** by default: they cannot be moved, dragged, swapped, dropped or placed, and they never drop on
death. See [The bow](../bow/the-bow.md).

## Three checks

A shot is checked three times, each at the moment its question can be answered:

| When | Asks | Refused with |
|---|---|---|
| As the bow is released | Is this world open to the bow? Is the cooldown over? | `refused-world`, `refused-cooldown`. No arrow leaves the bow |
| Where the arrow stops | Is it above `void-floor-y`? Is there room to stand? Is the region open? | `refused-void`, `refused-headroom`, `refused-destination` |
| As the player is moved | Is the region they stand in now open? | `refused-departure` |

The last check reads where the player is when they go, not where they were when they shot: a player who walked into
a closed region while the arrow flew is leaving that region. See [Shooting](../bow/shooting.md) and
[Filters](../bow/filters.md).

## The cooldown

`teleport.cooldown-ms`, 3 seconds by default, runs from the shot. The bow shows it as a white sweep, like an ender
pearl, and cannot be drawn while it runs. When a refusal is only found after the arrow flew, the plugin gives its own
cooldown back (`refund-cooldown-on-refusal`); the sweep on the bow is the game's and still runs to its end.

## The fall protection

A teleport onto a ledge can end in a drop. For `teleport.fall-damage-grace-ms`, 6 seconds by default, the **first**
fall the player takes does no damage. The protection ends at that fall or when the time runs out, whichever is first,
so it never turns into a player who takes no fall damage at all. Death and leaving clear it.

## Moments

The plugin calls the things that happen **moments**: the shot, the departure, the arrival, and the six refusals. Each
moment runs a list of action lines from `effects` in `config.conf`, in order: a sound, a particle, a message, a title,
a command. An empty list is silence. The words of a refusal are a line of the messages file, named with `@`, so they
stay translated. See [Effects](../bow/effects.md).

## Languages

The replies, the refusals and the bow's own name and lore are read in each player's language. Two players holding
the same bow read it in their own words. See [Languages](../config/languages.md).
