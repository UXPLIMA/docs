---
title: Storage and Levels
order: 36
description: Per-product levels, capacity, requirements and upgrading everything at once.
icon: chart-no-axes-column-increasing
---

Every product the farmer collects has its own level. The level sets how much of that product the
farmer can hold, what one unit is worth, and what tax is taken when it sells — so upgrading wheat
does nothing for cactus, and a player builds the ladder they actually use.

## What a level defines

```yaml
1:
  price: 1000
  unit-price: 1.6
  tax-price: 0.14
  capacity: 10000
  requirements: {}
  player-commands: []
  console-commands: []
```

| Key | Effect |
|---|---|
| `price` | What the player pays to reach this level |
| `capacity` | How much of the product the farmer may hold |
| `unit-price` | Overrides the product's price from this level on |
| `tax-price` | Overrides the tax from this level on |
| `requirements` | Conditions the player must satisfy before paying |
| `player-commands` | Run as the player when the level is reached |
| `console-commands` | Run from console when the level is reached |

`unit-price` and `tax-price` are optional and inherit downward: a level that does not state them
keeps whatever the last level that did was using. This is how a product gets more profitable as it
is upgraded without you rewriting every line.

Both command lists support PlaceholderAPI.

## Requirements

A requirement is a PlaceholderAPI condition with a line of text to show the player:

```yaml
requirements:
  "playtime":
    display: "<blue>1 Hour Playtime (Current: <papi:'math_0:floor_{statistic_PLAY_ONE_MINUTE}/72000'>)"
    condition: "%math_({statistic_PLAY_ONE_MINUTE}/72000)>60%"
    required-output: "1"
  "vip":
    display: "<green>VIP Rank"
    condition: "%vault_hasgroup_VIP%"
    required-output: "yes"
```

The plugin resolves `condition` and compares the result to `required-output` as a string. The
`math` expansion returns `1` for a true expression and `0` for a false one, which is why the
playtime check compares against `"1"`.

`display` is what the player reads in the upgrade menu. Write it as the requirement, not as the
expression — the player does not need to see your placeholder.

<Callout type="note" title="Braces inside conditions">

Inside a placeholder argument, `{...}` is used for nested placeholders so the outer `%...%` does not
terminate early. `%math_({statistic_PLAY_ONE_MINUTE}/72000)>60%` reads the playtime statistic,
divides it, and compares — all inside one `math` call.

</Callout>

## Default levels

`default-levels.yml` is the ladder used by any product that does not define its own. It runs from
level 0 at 2,500 capacity to the top of your table, with prices climbing as capacity does. Products
in `collected-materials.yml` with their own `levels` section ignore it completely.

Keep the default ladder as your baseline and give a product its own only when it genuinely needs a
different shape — a rare product with high unit price and low capacity, for instance.

## Upgrade All

`/farmer` players can upgrade every product at once from the upgrade-all menu.

```yaml
upgrade-all-upgrades-everything: false
upgrade-all-must-meet-all-requirements: false
upgrade-all-discount: 50
only-owners-can-upgrade: false
```

| Option | What it does |
|---|---|
| `upgrade-all-upgrades-everything` | Upgrades every product in `collected-materials.yml`, not only the ones this farmer has collected |
| `upgrade-all-must-meet-all-requirements` | Requires every product's money requirement to be met before any upgrade happens |
| `upgrade-all-discount` | Percentage off the total, as an incentive to upgrade in bulk |
| `only-owners-can-upgrade` | Members with the UPGRADE rank permission are still refused |

`upgrade-all-must-meet-all-requirements` applies to money requirements only — placeholder
requirements are always checked per product, because there is no meaningful way to partially satisfy
a VIP rank check.

Per-player discounts come from `uxmfarmer.upgradediscount.<percentage>`, which replaces the
configured `upgrade-all-discount` for that player.

## Storage limits and full stock

`glow-if-stock-is-full: true` puts a glow on the product's slot in the menu, which is the fastest
way for a player to see what needs selling. `notify-when-stock-gets-full` messages them; the
`notify-amount` setting caps how many notifications they get before it stops repeating.

## Setting levels as staff

```
/uxmfarmer select
/uxmfarmer setproductlevel WHEAT 12
/uxmfarmer give LEVEL_ADDER Notch 1 WHEAT 3
```

The first sets a level directly. The second gives the player an item that adds three wheat levels
when right-clicked near a farmer, which is what you hand out as a reward rather than editing
someone's data by hand.
