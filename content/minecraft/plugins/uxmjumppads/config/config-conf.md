---
title: config.conf
order: 501
description: Every key of the main file, with its shipped value and what it does.
icon: file-cog
---

The main settings file. Every key is shown with the value the plugin ships. `/uxmjumppads reload` applies every
change.

## language

| Key | Default | What it does |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a line a translation has not reached |
| `follow-client` | `true` | A player who has chosen nothing reads the language their client is set to; `false` starts everyone in `default` |
| `force` | `""` | One language for everybody, whatever they chose and whatever their client says; empty for none |

The languages are the files in `messages/`; nothing here lists them. See [Languages](languages.md).

## pads

The pad types, one block each, keyed by the type's name. The shipped map:

| Type | `block` | `under` | `forward` | `upward` | `permission` | `effects` |
|---|---|---|---|---|---|---|
| `classic` | `LIGHT_WEIGHTED_PRESSURE_PLATE` | `REDSTONE_BLOCK` | `1.6` | `0.9` | `""` | wind burst sound, cloud |
| `bounce` | `SLIME_BLOCK` | `EMERALD_BLOCK` | `0` | `1.4` | `""` | slime fall sound, slime particles |

Every key of a type, and what it does, is on [Pad types](../pads/types.md#the-keys). The map is yours: an update never
puts back a type you deleted.

## cooldown-ms

| Key | Default | What it does |
|---|---|---|
| `cooldown-ms` | `750` | Milliseconds before a pad throws the same player again. A negative value is read as `0` |

## fall-damage

| Key | Default | What it does |
|---|---|---|
| `grace-ms` | `10000` | Milliseconds after a throw in which the first fall costs nothing. `0` leaves fall damage alone |

See [Throws and landings](../pads/throws.md).

Related: [Configuration](index.md), [Pad types](../pads/types.md), [pads.conf](pads-conf.md)
