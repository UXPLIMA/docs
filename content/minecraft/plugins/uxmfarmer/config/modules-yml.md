---
title: modules.yml
order: 53
description: The five modules, the farm manager, and why permissions check the owner.
icon: toggle-right
---

Six sections: the five player-facing modules, and `farm-manager`, which is not a module players see
but the rules for what happens to items the farmer cannot take.

## The shape of a module

```yaml
auto-harvest:
  name: "Automatic Harvest"
  status: true
  default-status: false
  permission-required: true
```

| Key | Meaning |
|---|---|
| `name` | Shown in menus |
| `status` | Whether the module exists on this server |
| `default-status` | Whether a new farmer has it switched on |
| `permission-required` | Whether the owner needs the module's permission |

`status: false` removes the module from every farmer and every menu. `default-status` only decides
the starting position of a switch players can flip themselves.

## The permission rule

When `permission-required` is `true`, the permission is checked against the farmer's **owner**, not
the player using the farmer. A member with `uxmfarmer.autosell` on a farmer whose owner lacks it
still cannot auto-sell, and a member without it on a farmer whose owner has it can.

This is deliberate: the farmer's capabilities belong to the farmer, and the member's
[rank permissions](../features/members-and-ranks.md#rank-permissions) decide what they may do with
those capabilities. It also means a rank perk stops applying to a player's farmer the moment their
rank expires.

`/uxmfarmer checkperm <player> <permission>` is how you check what the plugin currently believes,
and `/uxmfarmer clearpermcache` is how you fix a stale answer.

## Module permissions

| Section | Permission |
|---|---|
| `production` | `uxmfarmer.production` |
| `auto-harvest` | `uxmfarmer.autoharvest` |
| `auto-kill` | `uxmfarmer.autokill` |
| `auto-sell` | `uxmfarmer.autosell` |
| `xp-collector` | `uxmfarmer.xpcollector` |

`auto-kill` and `auto-sell` additionally have `.modify` variants that gate changing their settings
separately from using them.

## farm-manager

```yaml
farm-manager:
  disable-growing-if-stock-is-full: true
  remove-remaining-drops-from-ground: true
  remove-drops-if-stock-is-full: true
  drop-items-to-ground-if-not-enabled-or-full: false
```

| Key | What it does |
|---|---|
| `disable-growing-if-stock-is-full` | Crops stop growing once the product's storage is full |
| `remove-remaining-drops-from-ground` | Deletes the overflow after the storage takes what it can |
| `remove-drops-if-stock-is-full` | Deletes drops entirely when the storage is already full |
| `drop-items-to-ground-if-not-enabled-or-full` | Leaves them on the floor instead |

`farm-manager` requires auto-harvest to be doing something; on its own it has nothing to manage.

The defaults are the cheap configuration and the right starting point. The last option is the
expensive one: it converts every overflowing farm into a pile of item entities, and on a server
with a hundred islands that is the difference between a healthy tick and a bad one.

## Module-specific keys

Each module has settings beyond the four common ones. They are documented with the module:

- [Auto Harvest](../features/auto-harvest.md): `items`, `piston-check-items`, `required-piston`,
  `extended-piston-check`, `drop-item-if-stock-full`
- [Auto Kill](../features/auto-kill.md): `mobs`, `only-kill-spawner-mobs`
- [Auto Sell](../features/auto-sell.md): `everything`, `items`, `send-message`, `send-to-members`
- [XP Collector](../features/xp-collector.md): `xp-unit-price`, `xp-tax`, `sellable-by-default`
