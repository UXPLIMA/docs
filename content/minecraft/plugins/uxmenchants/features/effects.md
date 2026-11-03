---
title: Effects
order: 402
description: The sixteen effects, their arguments, targets, chance, cooldown, delay and material filters.
icon: zap
---

An effect is a block, never a string. It names an `effect`, gives its arguments, and may carry its own `chance`,
`cooldown`, `delay`, `conditions` and `actions`. Those sit on the effect rather than the enchantment, because one
enchantment often wants two effects at two different chances.

```hocon
effects = [
  {
    effect   = ignite
    target   = victim
    chance   { base = 20.0, per-level = 10.0, cap = 45.0, mode = add, curve = linear }
    cooldown = 4.0
    seconds  { base = 2.0, per-level = 1.0, mode = add, curve = linear }
  }
]
```

## The effects

| Effect | Arguments | Acts on | What it does |
|---|---|---|---|
| `damage` | `amount` | `target` | Deals that much damage |
| `heal` | `amount` | `target` | Heals that much health, never above the entity's maximum |
| `feed` | `amount` | `target` | Adds that much food |
| `ignite` | `seconds` | `target` | Sets it alight |
| `potion` | `type`, `seconds`, `amplifier` | `target` | A vanilla potion effect, named the way the server names it; an unknown name is skipped |
| `launch` | `power` | `target` | Pushes it away from the player |
| `pull` | `power` | `target` | Pulls it towards the player |
| `extinguish` | | `target` | Puts out its fire |
| `lightning` | | The place | Strikes lightning |
| `explode` | `power`, `fire` | The place | An explosion that hurts but breaks no block; `fire = true` sets fire |
| `vein-miner` | `limit` | The block | Follows the same ore through the six faces, up to `limit` blocks |
| `tree-feller` | `limit` | The block | Takes the trunk, following every block that touches a corner, up to `limit` |
| `smelt-drops` | | The block | Cooks what the block drops |
| `extra-drops` | `multiplier` | The block | Drops the block's drops again, as many times over as the multiplier says |
| `repair-item` | `amount` | The item | Takes that much damage off the item the enchantment sits on |
| `give-experience` | `amount` | The player | Gives that much experience |

The arguments in the table are required, except `fire` on `explode`. The set is closed: a file that names another effect is refused with the file and the line. A message, a sound, a command
and a title are not effects; they are [actions](conditions-and-actions.md).

## target

`target = victim` (the default) aims at the entity hit or hitting; `target = self` aims at the player. A firing with no
victim, such as `block-break`, aims at the player whatever the file says.

## chance, cooldown and delay

| Key | Unit | Default | Meaning |
|---|---|---|---|
| `chance` | Out of a hundred | 100 | The effect fires only when the roll passes |
| `cooldown` | Seconds | 0 | After it fires, it waits this long for this player |
| `delay` | Seconds | 0 | How long after the trigger it runs |

Each may be a number or a [modifier block](numbers-and-descriptions.md). A cooldown lives in memory unless
`cooldowns.persist` is on; see [Charges and cooldowns](charges-and-cooldowns.md).

## materials

A block effect may narrow the blocks it touches:

```hocon
materials {
  whitelist = ["COAL_ORE", "DEEPSLATE_COAL_ORE"]
  blacklist = []
}
```

It can only narrow `effects.materials` of `config.conf`, never widen it, so `BEDROCK`, `BARRIER`, `END_PORTAL_FRAME`,
`SPAWNER` and `REINFORCED_DEEPSLATE` are never touched. A block another plugin names (`customcrops:tomato`) may be
written too.

Every block a block effect breaks passes the world list, the material lists, the claim check and the block break
treaty first; see [Protection](protection.md).

## What a firing asks

Before an effect runs, the trigger, the item's slot, the world, the enchantment's node, the item's charges, the
effect's chance and its cooldown are all asked, then its conditions. The effect runs after its delay and its actions
follow. A player without
the node sees the enchantment struck through on the item.

Related: [Numbers and descriptions](numbers-and-descriptions.md), [Conditions and actions](conditions-and-actions.md)
