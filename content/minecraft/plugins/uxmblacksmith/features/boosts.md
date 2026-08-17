---
title: Boosts
order: 206
description: Time multipliers and instant completions, defined in modules/boosts.yml.
icon: zap
---

A boost is an item a player right-clicks. There are two kinds, both defined in
`modules/boosts.yml`.

| Type | Effect |
|---|---|
| `TIME` | Multiplies the speed of the player's trades for a duration |
| `INSTANT` | Completes one active trade immediately, per use |

## The shipped boosts

| Key | Item | Type | Effect |
|---|---|---|---|
| `apprentice_rush` | Blaze Powder | `TIME` | 2x for 15 minutes |
| `forge_momentum` | Emerald | `TIME` | 3x for 30 minutes |
| `master_clockwork` | Nether Star | `TIME` | 4x for 60 minutes |
| `instant_finish` | Diamond | `INSTANT` | 1 use |
| `queue_breaker` | Clock | `INSTANT` | 3 uses |

## Defining one

```yaml
boosts:
  forge_momentum:
    id: forge_momentum
    material: EMERALD
    customModelData: -1
    name: '&a&lFORGE MOMENTUM &8(&f3x&8, &f30m&8)'
    lore:
      - ''
      - '&8• &f&lEFFECT'
      - '&73x faster trade progress for 30 minutes.'
      - ''
      - '&a[Right click to activate]'
    type: TIME
    multiplier: 3
    duration: 30
```

| Key | Applies to | Meaning |
|---|---|---|
| `id` | both | The key used by `/blacksmith giveboost` (keep it equal to the map key) |
| `material` | both | Bukkit material |
| `customModelData` | both | `-1` means none |
| `name` | both | Display name; `{uses}` is substituted |
| `lore` | both | Lore lines; `{uses}` is substituted |
| `type` | both | `TIME` or `INSTANT` |
| `multiplier` | `TIME` | Speed multiplier |
| `duration` | `TIME` | **Minutes** |
| `uses` | `INSTANT` | How many completions the item carries |

`duration` is minutes here, unlike a trade's `time`, which is seconds.

## Handing them out

```
/blacksmith giveboost <player> <boost>
```

Needs `uxmblacksmith.admin`, and the player must be online. The item goes into their inventory, or on
the ground if it is full.

That is the only built-in way to distribute boosts, which is deliberate: it means your crate
plugin, shop or reward system hands them out with a console command and you keep one source of truth
for the item's appearance.

## How they are identified

The boost id lives in the item's persistent data, not its name. A player can rename a boost item and
it still works; a player cannot craft a fake one by renaming an emerald.

`INSTANT` boosts store their remaining uses the same way and decrement in place, so one item can
carry three completions.

## Active state survives a restart

A running `TIME` boost is a row in `active_boosts` with its start and expiry. Log out with 20 minutes
left and you come back with 20 minutes left: the clock is wall-clock, not session time.

<Callout type="warning" title="A TIME boost keeps running while the player is offline">

The expiry is an absolute timestamp. Activating a 60-minute boost and logging off wastes it. Say so
in the lore if your players buy these.

</Callout>

## Using one

Right-click the item.

A `TIME` boost applies immediately to every trade the player has running and every one they start
before it expires. The item is consumed on activation.

An `INSTANT` boost arms the player and opens the main menu: they then click the slot they want
finished. One use is deducted from the item, and the item is returned to their inventory while uses
remain (or dropped at their feet if it is full).

`{time_remaining}` and `{progress_bar}` reflect the boosted rate, so the effect is visible in the
menu the moment it is applied.

## One at a time

A player may hold **one** active `TIME` boost and **one** armed `INSTANT` boost. Right-clicking a
second while one is running is refused with `timeBoostAlreadyActive` (which reports the running
multiplier and the seconds left) or `instantBoostAlreadyActive`, and the item is not consumed.

This is why a 4x boost cannot be stacked on top of a 2x one. If you sell boosts, the practical effect
is that a player buys the best one they can afford rather than several at once.
