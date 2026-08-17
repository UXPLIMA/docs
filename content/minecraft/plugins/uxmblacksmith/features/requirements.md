---
title: Requirements
order: 203
description: Every requirement type, item matching, and PlaceholderAPI conditions.
icon: list-checks
---

A requirement is what the player gives up, or a condition they must satisfy. They are numbered under
a trade and evaluated together — the trade starts only when all of them pass.

```yaml
requirements:
  1:
    displayName: '<aqua>Diamond'
    material: DIAMOND
    amount: 32
```

## Fields

| Key | Default | What it does |
|---|---|---|
| `displayName` | — | The name shown in the trade lore |
| `material` | `STONE` | A Minecraft material, or a type keyword from the table below |
| `amount` | `1` | How many |
| `requiredName` | unset | The item must carry this display name |
| `requiredLore` | unset | The item must carry these lore lines |
| `customModelData` | unset | The item must carry this model data |
| `customItemData` | unset | `customItemID` and `type`, for hook items |
| `ignoreMeta` | `true` | With `customModelData` set, match on material and model data only |
| `condition` | unset | A PlaceholderAPI expression, for `PLACEHOLDER` requirements |
| `requiredOutput` | unset | What the condition must evaluate to |

Text fields accept **both** legacy `&` codes and MiniMessage — the parser converts `&a` to
`<green>` before deserialising, so `'&bRare Diamond'` and `'<aqua>Rare Diamond'` both work, and they
can be mixed in one string. There is no format switch to set.

## The types

`material` is either a real Minecraft material or one of these keywords:

| Keyword | Consumes | Needs |
|---|---|---|
| Any material name | The items | — |
| `MONEY` | Vault balance | Vault |
| `PLAYERPOINTS` | PlayerPoints | PlayerPoints |
| `COINSENGINE:<currency>` | A CoinsEngine currency | CoinsEngine |
| `XP` | Vanilla experience | — |
| `MMOITEMS` | An MMOItems item | MMOItems |
| `ECOITEM` | An EcoItems item | EcoItems |
| `EXECITEM` | An ExecutableItems item | ExecutableItems |
| `IAITEM` | An ItemsAdder item | ItemsAdder or Nexo |
| `NEXO` | A Nexo item | Nexo or ItemsAdder |
| `SKULL` | A player head | — |
| `SAVE` | A saved binary item | — |
| `CUSTOMREQ` | A requirement-editor profile | — |
| `PLACEHOLDER` | **nothing** | PlaceholderAPI |

Each keyword's hook must be **enabled** under `settings.Hooks` in `config.yml`, not merely installed.
A trade whose requirement names a disabled or invalid hook is skipped at load with a warning — the
whole trade, not just that requirement.

## Matching a specific item

Plain `material` matching accepts any diamond. These fields narrow it:

```yaml
1:
  displayName: '<aqua>Rare Diamond'
  material: DIAMOND
  amount: 16
  requiredName: '<aqua>Rare Diamond'
  requiredLore:
    - ''
    - '<aqua>This is a rare diamond!'
    - ''
```

Or match on model data alone:

```yaml
2:
  displayName: '&6Gold Ingot'
  material: GOLD_INGOT
  amount: 48
  customModelData: 10
  ignoreMeta: true
```

`ignoreMeta: true` — the default — checks material and model data and ignores name, lore and
enchantments. That is what you want for a resource-pack item whose name players may have changed.
Set it to `false` when the name is part of the identity.

## Custom items

Hook items use `customItemData`, not a per-plugin key:

```yaml
1:
  material: ECOITEM
  customItemData:
    customItemID: enchanted_cobblestone
  amount: 1
2:
  material: MMOITEMS
  customItemData:
    customItemID: DRAGON_HELMET
    type: ARMOR
  amount: 1
3:
  material: EXECITEM
  customItemData:
    customItemID: armorpiece1_v1_8
  amount: 1
```

| Hook | `customItemID` | `type` |
|---|---|---|
| `MMOITEMS` | The item id | The MMOItems type — `ARMOR`, `SWORD`, … |
| `ECOITEM` | The EcoItems id | — |
| `EXECITEM` | The ExecutableItems id | — |
| `IAITEM` / `NEXO` | The item id | — |
| `SAVE` | The saved item key | — |
| `CUSTOMREQ` | The profile id from `/blacksmith editor` | — |

A shorthand form works for hooks that declare one — `material: 'SAVE:my_item'`,
`material: 'CUSTOMREQ:mythic_sword'` — instead of the nested block.

Hook items carry their own display name, so `displayName` is optional. When it is omitted the plugin
copies the name off the resolved item.

## Condition requirements

`PLACEHOLDER` requirements are **checked, never consumed**. This is where gating lives.

```yaml
4:
  displayName: '&e10 Level Required'
  material: PLACEHOLDER
  condition: '%math_{player_level}>=10%'
  requiredOutput: '1'

5:
  displayName: '&e1 Hour Playtime Required'
  material: PLACEHOLDER
  condition: '%math_({statistic_PLAY_ONE_MINUTE}/1200)>60%'
  requiredOutput: '1'

6:
  displayName: '&eVIP Required'
  material: PLACEHOLDER
  condition: '%vault_hasgroup_VIP%'
  requiredOutput: 'yes'

7:
  displayName: '&eSpecial Permission Required'
  material: PLACEHOLDER
  condition: '%permission_has_blacksmith.special%'
  requiredOutput: 'yes'
```

The plugin resolves `condition` and compares the result to `requiredOutput` as a **string** — quote
`'1'` rather than writing a bare number.

| Expansion | Why |
|---|---|
| Math | Returns `1` when an expression is true, so comparisons become `requiredOutput: '1'` |
| Vault | `%vault_hasgroup_X%` returns `yes` or `no` |
| Permission | `%permission_has_node%` returns `yes` or `no` |
| Statistic | Raw statistics — `PLAY_ONE_MINUTE` is in **ticks**, hence `/1200` for minutes |

Install them with `/papi ecloud download Math`, `… Vault`, `… String`.

<Callout type="tip" title="Conditions are the reason this system is flexible">

Anything PlaceholderAPI can answer becomes a requirement — quest completion, a rank, a stat, another
plugin's level, the time of day. If you find yourself wanting a feature the plugin does not have,
check whether a placeholder already answers the question.

</Callout>

<Callout type="warning" title="Test a condition before shipping it">

A condition that never evaluates true makes the trade permanently unstartable, and the player sees
only the `displayName` you wrote. Check it with `/papi parse me <condition>` first — and write the
`displayName` in words the player can act on.

</Callout>
