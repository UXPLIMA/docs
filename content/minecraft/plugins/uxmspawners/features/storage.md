---
title: Storage & Selling
order: 32
description: Virtual and XP storage, the limits and their ranks, taking items out, and selling from the menu.
icon: archive
---

A spawner keeps what it produces. The owner, or a friend, opens it, takes items out or sells them on
the spot. Nothing lies on the ground, so a busy farm costs the server one block instead of hundreds of
item entities.

## Virtual Storage and XP Storage

`general-settings.yml`:

```yaml
virtual-storage:
  default: true
  force-default: false
  permission: uxmspawners.settings.togglestorage

xp-storage:
  default: true
  force-default: false
  permission: uxmspawners.settings.togglexp
```

| Setting | Meaning |
|---|---|
| `default` | Whether a newly placed spawner has it on |
| `force-default` | Players cannot change it. Pressing the button sets the spawner back to `default` |
| `permission` | What a player needs to switch it in the spawner's Settings menu. Empty lets everyone |

With Virtual Storage off, items drop on the ground around the spawner, or where its mob died; with
XP Storage off, XP orbs do. `disable-xp-drops: true` stops spawners giving XP at all, stored or
dropped.

`force-default` does not change spawners already placed: one that has storage off keeps it off until
someone presses the button.

## Limits

`general-settings.yml`:

```yaml
storage-limit:
  enabled: true
  default: 2000
  xp-limit: 10000
  permissions:
    uxmspawners-storage1: 5000
    uxmspawners-storage2: 10000
    uxmspawners-storage3: 20000
  xp-permissions:
    uxmspawners-xpstorage1: 15000
    uxmspawners-xpstorage2: 20000
    uxmspawners-xpstorage3: 25000
  multiply-by-level: false
  over-limit: KEEP
```

| Setting | Meaning |
|---|---|
| `default` | How many of each item type a spawner holds. Rotten flesh and bones each have their own 2000 |
| `xp-limit` | How much XP a spawner holds |
| `permissions`, `xp-permissions` | Bigger limits for owners with these nodes. The highest one counts |
| `enabled` | `false` ignores the two permission lists. `default` and `xp-limit` still apply |
| `multiply-by-level` | Multiplies both limits by the spawner's level, so a stacked spawner fills no faster than a single one |
| `over-limit` | A spawner holding more than its limit, after its owner lost a rank: `KEEP` keeps it all but stores nothing new; `DELETE` cuts it down to the limit at the next cycle |

A spawner's limits follow its owner's permissions. They are worked out again when the owner places
or opens it, joins the server, and on `/usp reload`. With LuckPerms they also change at once when the
owner's or a group's permissions change, even while the owner is offline.

What comes in while the storage is full is lost.

## Taking items and XP

| Where | Click | Does |
|---|---|---|
| Storage menu | Left-click an item | Takes a stack of 64, or what is there |
| Storage menu | Right-click an item | Takes as many as fit in the inventory |
| Main or storage menu | The XP button | Gives all stored XP to whoever presses it |

The storage menu shows one slot per item type, as many as it has free slots: 28 in the shipped
layout. `ITEM` spawners have no XP and no XP button.

## Selling

**Sell All** in the storage menu sells everything the price source has a price for, at the prices
from [Prices](../config/prices.md), times the seller's multiplier. The money goes to whoever pressed
it: a friend who sells gets the money, at their own multiplier.

Items without a price are not sold. They stay in the storage and the seller is told how many. To
keep a spawner type from being sold at all, set `sell-button: false` in its file; the button is then
hidden.

```yaml
multipliers:
  enabled: true
  permissions:
    uxmspawners-1-25x: 1.25
    uxmspawners-1-50x: 1.50
```

The highest multiplier a player holds counts, and a player with none sells at 1. A value below 1
never applies. A Sell Multiplier boost on the spawner multiplies on top. See [Boosts](boosts.md).

With `title-messages.money-title` and `xp-title` on, selling and taking XP show the amount as a title.
`fade-in`, `stay` and `fade-out` set how long, in ticks. The texts are `sell-items-title` and
`take-xp-title` in the language file, and `number-format` there sets the separators of amounts.

When a spawner is broken or destroyed with something in its storage, see
[Breaking Rules](breaking.md#storage-when-a-spawner-goes).

<Callout type="tip" title="Selling counts in your shop plugin if you want">

With `dynamic-pricing: true` in `prices.yml`, a sale from a spawner counts in EconomyShopGUI or
ExcellentShop like a sale in the shop: its sell limits apply and its dynamic prices move.

</Callout>

Related: [Prices](../config/prices.md), [Friends](friends.md), [Permissions](../permissions.md)
