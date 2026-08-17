---
title: The dragon
order: 31
icon: flame
---

Everything about the boss is in the `dragon` section of `config.yml`.

## Name and visibility

```yaml
dragon:
  name: "<#00E5FF>Dragon Health: <#FF5555>%dragonevent_current_health%♥"
  name-visible: true
  boss-bar-visible: true
  glow: true
  glow-color: RED
```

`name` is MiniMessage and accepts placeholders, so the nametag can be a live health readout.
`name-visible` shows it above the dragon; `boss-bar-visible` shows the vanilla boss bar.

`glow-color` takes any of `BLACK`, `DARK_BLUE`, `DARK_GREEN`, `DARK_AQUA`, `DARK_RED`,
`DARK_PURPLE`, `GOLD`, `GRAY`, `DARK_GRAY`, `BLUE`, `GREEN`, `AQUA`, `RED`, `LIGHT_PURPLE`,
`YELLOW`, `WHITE`. Glow makes the dragon visible through the end's terrain, which matters more than
it sounds when forty players are looking for it.

## Health

```yaml
dragon:
  health: 200
  health-modifier:
    add-as-extra: true
    health-per-player: 0.0
```

`health` is the base. `health-per-player` scales it with the number of players in the event:

- `add-as-extra: true` — final health is `health + (players × health-per-player)`
- `add-as-extra: false` — final health is `players × health-per-player`, and `health` is ignored

`health-per-player: 0.0` disables scaling.

Scaling is the setting that makes an event work at both twenty players and two hundred. Without it,
you are tuning a fixed number against an attendance you cannot predict.

<Callout type="warning" title="2048 is a hard ceiling by default">

Spigot caps entity health at 2048. A configured value above that is clamped, not honoured — raise
`maxHealth` in `spigot.yml` first if you need more. With `health-per-player` on a busy server it is
easy to cross that line without noticing.

</Callout>

## Damage

```yaml
dragon:
  damage-modifier:
    add-base-to-final-damage: false
    base-damage: 0.0
    multiplier: 1.0
```

| Key | Effect |
|---|---|
| `base-damage` | The damage to work from; `0.0` uses vanilla damage |
| `multiplier` | Multiplies it; `1.0` is unmodified |
| `add-base-to-final-damage` | Adds `base-damage` to the result instead of replacing it |

With `disableDamage: true` in the end rules — the default — none of this reaches players. Turn that
off first if you want the dragon to be dangerous.

## Rebirths

```yaml
dragon:
  required-kills: 2
```

How many times the dragon must die. Every death but the last respawns it and pays the
`each-last-hit` reward; the last one ends the event.

Two or three is the usual range. More than that and the later rebirths feel like repetition rather
than escalation, because the dragon does not get stronger between them.

## Piñata mode

```yaml
dragon:
  pinata: false
```

The dragon stays where it spawned and does not move. Worth considering for a large event: a moving
dragon in the end spends most of the fight out of reach, and a hundred players chasing it is not a
fight so much as a queue.

## Custom models

```yaml
dragon:
  custom-model:
    model-plugin: DEFAULT
    model-id: unknown
```

| Value | Uses |
|---|---|
| `DEFAULT` | The vanilla dragon; the model system does nothing |
| `MODEL_ENGINE` | ModelEngine V4 |
| `ITEMS_ADDER` | ItemsAdder V4 |
| `BETTER_MODEL` | The latest BetterModel |

Nexo and Oraxen both render through ModelEngine — use `MODEL_ENGINE` for those. `model-id` is the
model's id in whichever plugin you chose.

## Custom NBT

```yaml
dragon:
  custom-nbt: ""
```

Extra NBT applied to the dragon entity, for attributes or tags other plugins read. Write the contents
**without** the surrounding braces — `{` and `}` at either end will not parse.

Requires NBTAPI.
