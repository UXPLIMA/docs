---
title: Your first trade
order: 104
description: "A category, a trade, a requirement and a risk: start to finish."
icon: play
---

## 1. Grant slots

Nothing works until a player has a slot.

```
uxmblacksmith.slot.1
uxmblacksmith.slot.2
```

Two nodes, two concurrent trades. Give at least one to your default group.

## 2. Define a category

In `config.yml`:

```yaml
categories:
  swords:
    name: "Swords"
    permission: ""
    trades: "swords.yml"
    slot: 13
    sameTimeCraftCount: 0
    menuDisplayItem:
      material: "IRON_SWORD"
      displayName: "<gradient:#F8E889:#C97B36><bold>Swords</bold></gradient>"
      lore:
        - ""
        - "<gray>Forge blades worth carrying.</gray>"
        - ""
```

`sameTimeCraftCount: 0` means unlimited concurrent trades from this category; `-1` means the same.
A positive number caps it.

Restart. The plugin writes `categories/swords.yml` with worked defaults.

## 3. Write the trade

`categories/swords.yml`:

```yaml
trades:
  1:                          # page 1
    1:                        # trade 1
      item:
        material: "NETHERITE_SWORD"
        name: "&c&lKAVACHA &8-- &eLegendary"
        lore:
          - ""
          - "&7 Forged over three hours. "
          - ""
        enchantments:
          - "SHARPNESS:5"
          - "FIRE_ASPECT:2"
        time: 10800           # 3 hours, in seconds
        successChance: 85     # 15% chance to burn
        slot: 11
        reqLevel: 5
        xpReward: 250
      requirements:
        1:
          displayName: "&bNetherite Ingot"
          material: "NETHERITE_INGOT"
          amount: 4
        2:
          displayName: "&eMoney"
          material: "MONEY"
          amount: 25000
```

`time` is seconds. `successChance: -1` would make it never burn.

## 4. Add a gate

A requirement does not have to be an item. This one checks a placeholder:

```yaml
        3:
          displayName: "&e10 Hours Playtime Required"
          material: "PLACEHOLDER"
          condition: "%math_({statistic_PLAY_ONE_MINUTE}/1200)>600%"
          requiredOutput: '1'
```

`PLACEHOLDER` requirements are **checked, not consumed**. The Math expansion returns `1` when the
expression is true, which is what `requiredOutput` compares against, as a string.

This needs PlaceholderAPI and the Math expansion: `/papi ecloud download Math`.

## 5. Reload and try it

```
/blacksmith reload
/blacksmith
```

The category menu opens. Click **Swords**, click the trade, hand over the materials. The slot fills,
the progress bar starts, and `{time_remaining}` counts down.

## 6. Watch the progression

```
/blacksmith admin profile <player>
```

Prints their level, XP, perk points and craft counts. After the trade completes they will have gained
global XP, mastery XP in `swords`, and a perk point if they levelled.

```
/blacksmith perks
/blacksmith mastery
```

Open the two progression menus.

## 7. Give yourself a boost to test

```
/blacksmith giveboost <player> instant_finish
```

`instant_finish` is one of the five shipped boosts, and the only single-use `INSTANT` one:
right-click it, pick the slot, and the trade finishes immediately so you can verify the reward
without waiting three hours.

<Callout type="tip" title="Test with a short duration first">

Set `time: 30` while you are building a trade, confirm the reward, the burn message and the XP, then
raise it to the real number. A three-hour test loop is a bad way to find a typo in an enchantment
name.

</Callout>
