---
title: Storage & Selling
order: 32
description: Virtual and XP storage, the limits and their tiers, withdrawing, and selling from the menu.
icon: archive
---

A spawner keeps what it produces. The owner, or a friend, opens it, takes items out or sells them on
the spot. Nothing lies on the ground, so a busy farm costs the server one block instead of hundreds of
item entities.

## Virtual storage and XP storage

`modules/settings.yml`:

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
| `permission` | What a player needs to switch it in the spawner's settings menu |

With virtual storage off, items drop on the ground around the spawner; with XP storage off, XP orbs
do. `disable-xp-drops: true` in `config.yml` stops spawner mobs giving XP at all, stored or dropped.

`force-default` does not change spawners already placed: one that has storage off keeps it off until
someone presses the button.

## Limits

`modules/storage_limits.yml`:

```yaml
storage-limit:
  enabled: true
  default: 2000
  xpLimit: 10000
  permissions:
    uxmspawners-storage1: 5000
    uxmspawners-storage2: 10000
  xpPermissions:
    uxmspawners-xpstorage1: 15000
    uxmspawners-xpstorage2: 20000
```

| Setting | Meaning |
|---|---|
| `default` | How many of each item type a spawner holds. Rotten flesh and bones each have their own 2000 |
| `xpLimit` | How much XP a spawner holds |
| `permissions`, `xpPermissions` | Bigger limits for players with these nodes. The highest one a player holds counts |
| `enabled` | `false` ignores the permission lists. `default` and `xpLimit` still apply |

A spawner's limits come from its owner's permissions, and are worked out again every time the owner
opens its menu. A rank bought later raises the limit of spawners already placed, from their next
opening. What comes in while the storage is full is lost.

A newly placed spawner stores no XP until its owner opens its menu for the first time. Open each
spawner once after placing it.

## The storage menu

| Click | Does |
|---|---|
| Left-click an item | Takes 64, or what is there |
| Right-click an item | Takes as many as fit in the inventory |
| The sell button | Sells everything in the storage |

The XP button in the main menu gives all stored XP to whoever presses it. Item spawners have no XP
button.

## Selling

The sell button sells the whole storage at once, at the prices from [Prices](../config/prices.md),
multiplied by the seller's multiplier. The money goes to whoever pressed it: a friend who sells
gets the money, with their own multiplier.

<Callout type="danger" title="Selling empties the whole storage">

Items without a price are sold for nothing and are gone with the rest. If players should keep
something, give it no sell price and tell them to take it out first, or turn the sell button off for
that spawner type with `sell_button: false`.

</Callout>

`modules/multipliers.yml`:

```yaml
multipliers:
  enabled: true
  permissions:
    uxmspawners-1-25x: 1.25
    uxmspawners-1-50x: 1.50
```

The highest multiplier a player holds counts, and a player with none sells at 1. A value below 1
never applies.

With `title-messages.money-title` and `xp-title` on in `modules/other.yml`, selling and taking XP show
the amount as a title on screen. Their texts are `sellItemsTitle` and `takeXpTitle` in the language
file.

Related: [Prices](../config/prices.md), [Friends](friends.md), [Permissions](../permissions.md)
