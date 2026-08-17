---
title: Slots
order: 205
description: How many trades a player runs at once, and how to turn the system off.
icon: layout-grid
---

A trade occupies a slot for its whole duration. A player with no slots cannot start anything.

## Granting slots

```
uxmblacksmith.slot.1
uxmblacksmith.slot.2
…
uxmblacksmith.slot.10
```

A player's slot count is **how many of those ten nodes they hold**, not the highest number. Give
`uxmblacksmith.slot.1` to your default group, then add `.2`, `.3` and so on per rank.

Ten is the ceiling. `uxmblacksmith.slot.11` does nothing.

The `EXTRA_SLOT` perk (`artisan_slots`, up to 3 levels) unlocks that many of the player's
**locked** slots, lowest-numbered first. A player holding all ten nodes gains nothing from it, which
is the intended relationship: the perk is a path to slots for players who did not buy a rank, not a
way past ten.

Anyone with `uxmblacksmith.admin` is treated as having **99** slots: that is what
`%uxmblacksmith_maxslots%` returns for staff, so do not be alarmed by the number.

<Callout type="warning" title="Slots are not granted by default">

Nothing in the shipped config gives a player their first slot. Open the server without
`uxmblacksmith.slot.1` on the default group and the menu opens, the trades render, and every click
refuses. Grant it before launch.

</Callout>

## Where slots live in the GUI

`gui.yml` maps each slot number to an inventory position and the node that unlocks it:

```yaml
mainMenu:
  tradingIconSlotsAndPermissions:
    1: { slot: 29, permission: uxmblacksmith.slot.1 }
    2: { slot: 30, permission: uxmblacksmith.slot.2 }
    3: { slot: 31, permission: uxmblacksmith.slot.3 }
    4: { slot: 32, permission: uxmblacksmith.slot.4 }
    5: { slot: 33, permission: uxmblacksmith.slot.5 }
    6: { slot: 38, permission: uxmblacksmith.slot.6 }
    7: { slot: 39, permission: uxmblacksmith.slot.7 }
    8: { slot: 40, permission: uxmblacksmith.slot.8 }
    9: { slot: 41, permission: uxmblacksmith.slot.9 }
    10: { slot: 42, permission: uxmblacksmith.slot.10 }
```

The `permission` is a config value, so you can point slot 4 at `donor.tier2` instead if that suits
your rank layout better. Locked slots render the "buy this slot" icon.

## Three separate caps

| Cap | Set in | Limits |
|---|---|---|
| `uxmblacksmith.slot.<n>` plus `EXTRA_SLOT` perks | Permissions, perk tree | Total concurrent trades |
| `sameTimeCraftCount` on a category | `config.yml` | Concurrent trades from that category |
| `sameTimeCraftCount` on a trade | The category file | Concurrent copies of that one trade |

Both `sameTimeCraftCount` values use `0` and `-1` to mean unlimited.

A player with 5 slots, a `swords` category capped at 1, and a legendary trade capped at 1 can run
five trades at once, but only one of them from `swords`, and only one copy of that legendary.

`{remaining_trade_slots}` in the trade lore shows the tightest of the three that currently applies.

## Turning the whole system off

```yaml
settings:
  disableSlotModule: false
  instantCraftBatchLimit: 64
```

Set `disableSlotModule: true` and the plugin becomes a straight recipe menu: no slot icons, no
waiting, trades complete on click. `time` is ignored. Use it when you want the requirement and burn
systems without the queue.

`instantCraftBatchLimit` caps how many crafts one shift-click may run. It is a guard against a
recipe whose requirements are effectively free looping forever: leave it at 64 unless you have a
reason.

A single trade can opt into the same behaviour without changing the server:

```yaml
item:
  instantCraft: true
```

That trade completes on click, occupies no slot, and earns XP at `instantCraftMultiplier`, `0.65`,
because it carries no opportunity cost.

## Performance

```yaml
settings:
  performance:
    remainingTradeSlotCacheTime: 10000
    remainingTradeSlotCacheLimit: 200
```

Remaining-slot counts are cached for 10 seconds, up to 200 entries. Raise the time if you have many
players and menus feel heavy; lower it if `{remaining_trade_slots}` looks stale.

## What happens when a slot frees up

The trade completes, the reward is handed over (or dropped at the player's feet if their inventory
is full) and the slot returns to the pool. If the player is offline the trade is finished on their
next login; a pending trade is a database row, not a memory object, so a restart does not lose it.
