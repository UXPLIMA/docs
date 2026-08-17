---
title: Vanilla crafting gates
order: 210
description: Requiring a blacksmith level before a crafting-table recipe works.
icon: table-2
---

The plugin can refuse ordinary crafting-table recipes until the player has a blacksmith level. It is
the one feature that reaches outside the blacksmith menu, and it is **on by default**.

```yaml
vanillaCrafting:
  enabled: true
  denyMessage: '&cYou need Blacksmith Level {level} to craft this item!'
  levelRequirements:
    IRON_SWORD: 2
    SHIELD: 3
    IRON_CHESTPLATE: 4
    DIAMOND_SWORD: 8
    DIAMOND_PICKAXE: 9
    DIAMOND_CHESTPLATE: 12
    ANVIL: 12
    NETHERITE_SWORD: 18
    NETHERITE_PICKAXE: 20
    NETHERITE_CHESTPLATE: 24
```

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Turn the whole gate on or off |
| `denyMessage` | see above | `{level}` is the required level |
| `levelRequirements` | 10 entries | Material name → blacksmith level |

## How it works

On a craft, the plugin looks up the **result material** by name. If it is in the map and the player's
blacksmith level is below the value, the craft is cancelled and `denyMessage` is sent.

That is the entire rule. Consequences worth knowing:

- The map is keyed by material, not by recipe. Gating `IRON_SWORD` gates every recipe producing one,
  including a custom recipe from another plugin.
- A material not in the map is never gated.
- The check reads the player's stored profile. With `progression.enabled: false` the level is
  whatever is on file and never rises, so **disable this gate too**, or nobody crafts an iron sword
  again.

## Why you would want it

The shipped list is a coherent design: iron gear at level 2–4, diamond at 8–12, netherite at 18–24.
A new player cannot skip straight to a diamond sword at the crafting table, so the blacksmith is
where early gear comes from, which is the whole point of installing the plugin.

If you only want the blacksmith as an extra shop, set `enabled: false`.

## Extending it

Any material name works:

```yaml
    ENCHANTING_TABLE: 15
    BEACON: 30
    ELYTRA: 40
```

Levels above `levelCurve.maxLevel` make the item permanently uncraftable: occasionally useful, but
write it down somewhere.

<Callout type="warning" title="This is on by default and it surprises people">

A fresh install silently blocks iron swords at the crafting table until a player reaches blacksmith
level 2, and a player who has never opened the blacksmith is level 1. If your server has an
established economy, review `levelRequirements` **before** the first restart, not after the first
ticket.

</Callout>

<Callout type="note" title="It gates crafting, not obtaining">

The check runs on `CraftItemEvent`. Loot, trades, kits, shops and `/give` are untouched. It shapes
progression; it does not make an item rare.

</Callout>
