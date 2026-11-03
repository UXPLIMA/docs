---
title: The bow and the arrow
order: 401
description: The two items the plugin hands out, when they are handed out, how they look, and the lock that keeps them in place.
icon: package
---

The plugin hands out two items and recognises them by a hidden tag, never by their name:

| Item | Ships as | Why it exists |
|---|---|---|
| The bow | A glowing, unbreakable `BOW` in slot `1` (hotbar slot 2), named and described in the player's language | It is the toy |
| The arrow | One `ARROW` in slot `9`, the top left of the inventory | A bow cannot be drawn outside creative mode without an arrow. The shot never spends it |

Slots count from 0: 0 to 8 is the hotbar, 9 to 35 the inventory, 36 to 39 the armour, 40 the off hand.

## When it is handed out

| Key under `item.give` | Default | Gives the bow when |
|---|---|---|
| `on-join` | `true` | A player joins |
| `on-respawn` | `true` | A player respawns |
| `on-world-change` | `true` | A player changes world |
| `respect-world-filter` | `true` | Each of the three above is skipped in a world the [world filter](filters.md#worlds) closes |

A lobby gives it on join and leaves the other two on, so a player who loses it in some other way gets it back. An
arena that only wants it in one world gives it on world change. `/uxmteleportbow give` hands it out by hand and
ignores the world filter.

A player who changes into a closed world keeps the bow they hold; it is not taken away, and every shot there is
refused with `refused-world`.

## Where it goes

The bow goes to `item.bow.slot` when that slot is empty or holds the bow already; otherwise to the slot the player
moved it to, or the first free slot. Another item is never written over, and a player never gets a second bow. Two
plugins that each hand out an item need a slot each, or one of the items lands somewhere else.

## How it looks

| Key under `item.bow` | Default | What it does |
|---|---|---|
| `material` | `"BOW"` | The item. It must still be a bow for the game to fire an arrow; change it only for a resource pack that re-models one. A name the server does not know reads as `BOW` |
| `lore-lines` | `9` | How many lore lines are shown; line N is `item.bow-lore-N` in the messages |
| `custom-model-data` | `0` | The resource pack model; `0` for none |
| `glow` | `true` | The enchanted shimmer, with no enchantment behind it |
| `unbreakable` | `true` | The bow never wears out |

The bow shows no vanilla tooltip lines (damage, enchantments): its lore says what it is. The name ships as a single
space, so the first lore line carries the title. The name and lore are written in the messages files and drawn for
each player in their own language, so two players holding the same bow read different words. See
[Messages](../config/messages.md).

The shipped lore, in English:

```
 ◆ Teleport Bow
    Lobby

 ✎ Description
    Shoot, and go where the arrow lands.
    The arrow is never spent.

 → Shoot to travel
```

## The arrow

| Key under `item.ammo` | Default | What it does |
|---|---|---|
| `enabled` | `true` | Hand out the arrow. Turn it off only on a server where every player is in creative mode |
| `material` | `"ARROW"` | The item. A name the server does not know reads as `ARROW` |
| `slot` | `9` | The slot, by the same rule as the bow |
| `amount` | `1` | How many, from 1 to 64 |

`item.bow.infinite-arrows = true` (the default) keeps the shot from spending it. Turned off, the first shot spends
the arrow and the bow cannot be drawn again until the player gets another.

## The lock

`item.locked = true` keeps the bow and the arrow where they were put. They cannot be:

| Taken by | Result |
|---|---|
| A click, a shift click or a number key in any inventory | Refused |
| A drag | Refused |
| The drop key | Refused |
| The swap hand key | Refused |
| Placing | Refused |
| Death | They are left out of the drops |

With `item.locked = false` a player can drop the bow, store it or give it away, and whoever picks it up holds a bow
that works: the tag is on the item, not on the player.

## Notes

- **An ordinary bow is not the teleport bow.** A crafted or looted bow carries no tag, so it shoots arrows as usual.
- **A bow from before a rename still works.** Changing `bow-name` or the lore never breaks a bow already handed out.

Related: [Shooting](shooting.md), [config.conf](../config/config-conf.md#item)
