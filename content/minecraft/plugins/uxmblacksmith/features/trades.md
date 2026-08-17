---
title: Trades
order: 202
description: Every field on a trade, and what each one changes.
icon: hammer
---

A trade lives in a category file under `categories/`, nested by page and then by id.

```yaml
trades:
  1:              # page
    1:            # trade id within the page
      item: { … }
      requirements: { … }
```

Pages exist so a category with forty trades paginates. Trade ids need not be contiguous.

## The reward

| Key | Default | What it does |
|---|---|---|
| `material` | n/a | The reward material, or a hook name (`MMOITEMS`, `ECOITEM`, `EXECITEM`, `IAITEM`, `NEXO`, `SAVE`) |
| `name` | n/a | Display name |
| `lore` | `[]` | Lore lines |
| `enchantments` | `[]` | `"ENCHANT:level"` entries |
| `customModelData` | unset | Custom model data |
| `customItemData` | unset | Hook-specific item id (see below) |
| `asCommand` | `false` | Run commands instead of giving an item |
| `commands` | `[]` | Commands to run when `asCommand` is true |
| `hideAttributes` | `false` | Hide the attribute lines |
| `forceGlow` | `false` | Force the enchantment glint (**hides enchantments**) |
| `removeDisplaySettings` | `true` | Strip `forceGlow` and `hideAttributes` from the item actually handed over |

Enchantment levels are not capped to vanilla limits. `"PROTECTION:15"` works and is one of the shipped
examples.

`forceGlow: true` hides the enchantment list, which is what you want for a decorative glow and not
what you want on a real weapon.

## Timing and risk

| Key | Default | What it does |
|---|---|---|
| `time` | `30` | Duration in **seconds** |
| `successChance` | `-1` | Percent chance of success; `-1` disables burn for this trade |
| `instantCraft` | `false` | Bypass the slot system and complete immediately |
| `sameTimeCraftCount` | `-1` | Concurrent copies of this trade; `-1` is unlimited |
| `burnBypassPermission` | `uxmblacksmith.burn.bypass` | Holding it makes this trade always succeed |

`instantCraft: true` is how you offer a cheap recipe that should feel like a normal crafting table,
no slot consumed, no waiting. It earns XP at `instantCraftMultiplier`, `0.65`, because it carries no
opportunity cost.

## Gating

| Key | Default | What it does |
|---|---|---|
| `reqLevel` | `0` | Blacksmith level required |
| `requiredMasteryLevel` | `0` | Mastery level required, in the resolved mastery category |
| `slot` | `-1` | Where it sits in the trades menu; `-1` leaves an empty slot |

## Progression rewards

| Key | Default | What it does |
|---|---|---|
| `xpReward` | unset | Explicit global XP; omitted means the default reward policy computes it |
| `masteryKey` | unset | Which mastery this trade feeds; omitted means the category key |
| `masteryXpReward` | unset | Explicit mastery XP; omitted means it is derived from global XP |

Leaving all three unset is usually right: the default policy already scales XP with duration, the
number of requirements, their amounts and the risk. Set `xpReward` when a trade should be worth
something the formula would not guess. See [Progression](progression.md).

## Custom items as rewards

Set `material` to the hook name and put the item's id in `customItemData`:

```yaml
item:
  material: MMOITEMS
  customItemData:
    customItemID: DRAGON_HELMET
    type: ARMOR
  time: 3600
```

| Hook | `customItemID` | `type` |
|---|---|---|
| `MMOITEMS` | The item id | The MMOItems type (`ARMOR`, `SWORD`, …) |
| `ECOITEM` | The EcoItems id | n/a |
| `EXECITEM` | The ExecutableItems id | n/a |
| `IAITEM` / `NEXO` | The item id | n/a |
| `SAVE` | The saved item key | n/a |

A shorthand form works for hooks that declare one (`material: 'SAVE:my_item'`,
`material: 'CUSTOMREQ:profile_id'`) instead of the nested block.

`name`, `lore` and `enchantments` are ignored for a hook reward: the item comes from the other plugin
exactly as that plugin defines it.

<Callout type="danger" title="A saved item carrying a personal key will not behave">

`SAVE` stores a serialised `ItemStack`. If the item you saved held a per-player NBT key (a soulbound
owner, a signed book author, a plugin's "belongs to" tag), every copy handed out carries that same
key. Save a clean item.

</Callout>

## Commands instead of an item

```yaml
item:
  material: "DIAMOND_PICKAXE"
  name: "&6&lSAGARIS"
  asCommand: true
  commands:
    - "give_custom_pickaxe {player} legendary-pickaxe"
  time: 3600
```

With `asCommand: true` the player receives **nothing**: the commands run instead. The `material`,
`name` and `lore` still describe the trade in the menu, which is how the player knows what they are
buying.

`{player}` is replaced with their name. Commands run from console.

<Callout type="warning" title="A command reward cannot be rolled back">

An item reward that cannot fit goes to the ground. A command reward has already run. If the command
targets a plugin that might be down, the player pays and gets nothing, and the refund queue does not
cover it. Prefer an item where you can.

</Callout>
