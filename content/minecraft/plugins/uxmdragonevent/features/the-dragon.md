---
title: The dragon
order: 31
description: Name, glow, health and how it scales, damage, rebirths, piñata mode and custom models.
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

| Key | Effect |
|---|---|
| `name` | The nametag, in MiniMessage. Placeholders work and are updated on every hit, so it can show the health live |
| `name-visible` | Shows the nametag above the dragon |
| `boss-bar-visible` | Shows the vanilla dragon boss bar |
| `glow` | Outlines the dragon so it can be seen through the terrain, for everyone online, including players who log in later |
| `glow-color` | `BLACK`, `DARK_BLUE`, `DARK_GREEN`, `DARK_AQUA`, `DARK_RED`, `DARK_PURPLE`, `GOLD`, `GRAY`, `DARK_GRAY`, `BLUE`, `GREEN`, `AQUA`, `RED`, `LIGHT_PURPLE`, `YELLOW`, `WHITE` |

A second, configurable boss bar is described in [Presentation](presentation.md#the-second-boss-bar).

## Health

```yaml
dragon:
  health: 200
  health-modifier:
    add-as-extra: true
    health-per-player: 0.0
```

`health` is the dragon's health. `health-per-player` adds to it based on how many players are
**online on the server** when the dragon spawns, which is when the event starts:

| `add-as-extra` | The dragon's health |
|---|---|
| `true` | `health` + online players × `health-per-player` |
| `false` | online players × `health-per-player`; `health` is not used |

`health-per-player: 0.0` turns this off. With 40 players online, `health: 200` and
`health-per-player: 25` give a dragon with 1,200 health.

The count is taken from the whole server, not from the event, because nobody has joined yet when the
dragon appears. A busy server gets a stronger dragon even if only part of it takes part.

<Callout type="warning" title="The server caps health at 2048">

Paper limits every creature's health to `settings.attribute.maxHealth.max` in `spigot.yml`, 2048 by
default. A higher value is lowered to that limit and the console warns once. With
`health-per-player` on a busy server this is easy to reach: raise the limit in `spigot.yml` first.

</Callout>

## Damage to players

```yaml
dragon:
  damage-modifier:
    add-base-to-final-damage: false
    base-damage: 0.0
    multiplier: 1.0
```

The modifier changes the damage the dragon's own hits deal to players in the event. Its breath and
fireballs deal their normal damage.

| `base-damage` | `add-base-to-final-damage` | A hit deals |
|---|---|---|
| `0.0` | any | The normal damage × `multiplier` |
| above `0.0` | `false` | `base-damage` × `multiplier`, whatever the normal damage was |
| above `0.0` | `true` | (normal damage + `base-damage`) × `multiplier` |

`multiplier: 1.0` leaves the damage as it is. `multiplier: 0.0` turns the whole modifier off,
`base-damage` included, so use `1.0` when you only want `base-damage`.

With `end-rules.disableDamage: true`, the default, players take no damage at all and none of this
applies. Turn that off first if you want the dragon to be dangerous.

## Rebirths

```yaml
dragon:
  required-kills: 2
```

How many times the dragon must die. Every death but the last brings it straight back with full
health: players in the event see the `respawn` title, and the player who landed the killing blow gets
the `each-last-hit` reward. The last death ends the event. The lowest value is `1`.

The dragon does not get stronger between rebirths. Two or three is the usual range; more and the
later rounds feel like repetition rather than a climax.

## Who can hurt it

Only players in the event damage the dragon. A hit from someone watching, such as staff who used
`/dragon teleport`, is refused. Every hit is added to the player's damage for the leaderboard; the
killing blow only counts the health the dragon had left, so one big hit at the end does not inflate
anyone's score.

The plugin removes any other dragon that appears in the event world, and spawns its dragon again if
it disappears while players are in the event.

## Piñata mode

```yaml
dragon:
  pinata: false
```

With `true`, the dragon hovers where it spawned instead of flying its circles. Worth considering for
a large event: a moving dragon spends most of the fight out of reach, and a hundred players chasing it
is more a queue than a fight.

## Custom models

```yaml
dragon:
  custom-model:
    model-plugin: DEFAULT
    model-id: unknown
```

| `model-plugin` | Uses |
|---|---|
| `DEFAULT` | The vanilla dragon |
| `MODEL_ENGINE` | A ModelEngine 4 model. Nexo and Oraxen models go through ModelEngine too |
| `ITEMS_ADDER` | An ItemsAdder 4 custom entity |
| `BETTER_MODEL` | A BetterModel model |

`model-id` is the model's id in that plugin. If the plugin is not installed, or the value is not one
of these, the console warns and the vanilla dragon is used.

## Custom NBT

```yaml
dragon:
  custom-nbt: ""
```

Extra NBT for the dragon, for tags or attributes other plugins read. Write it **without** the outer
braces: `{` and `}` at the start and end break it. Needs NBTAPI; without it the console warns and the
NBT is ignored.
