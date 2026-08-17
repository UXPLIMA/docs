---
title: Boosts
order: 36
description: Spawn-amount and spawn-time boosts, as items players apply.
icon: zap
---

A boost is an item a player applies to a spawner for a fixed period. Boosts are defined in
`modules/boosts.yml` and handed out with `/uxmspawners giveboost <player> <boost>`.

```yaml
boosts:
  enabled: true
  list:
    boost1:
      material: "DIAMOND"
      type: "SPAWN_AMOUNT"
      amount: 2
      model-data: 0
      duration: "1d"
      name: "<yellow><bold>BOOST <dark_gray>- <white>1 days"
      lore:
        - "<gray> Applying this boost will increase"
        - "<gray> the spawner's mob output by <green>+2"
```

| Key | Meaning |
|---|---|
| `material` | The item that represents the boost |
| `type` | `SPAWN_AMOUNT` or `SPAWN_TIME` |
| `amount` | How much is added or multiplied |
| `duration` | How long it lasts (`1d`, `12h`, and so on) |
| `model-data` | Custom model data for resource packs |
| `name`, `lore` | The item's appearance, MiniMessage |

## The two types

| Type | Effect |
|---|---|
| `SPAWN_AMOUNT` | Adds `amount` mobs to each spawn cycle |
| `SPAWN_TIME` | Divides the delay between cycles (`amount: 4` means four times faster) |

The boost id (`boost1`, `boost2`, whatever you name it) is what tab completion offers and what
`giveboost` takes. Name them after what they do rather than by number, and the staff command
becomes readable: `/uxmspawners giveboost Notch double-spawn`.

<Callout type="tip" title="Boosts are the thing to sell">

A boost is temporary and consumable, which makes it a much healthier crate or store item than a
permanent upgrade. Two shipped examples (`+2 per cycle for a day`, `4× speed for twelve hours`)
are a reasonable starting shape.

</Callout>
