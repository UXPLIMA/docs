---
title: Chat colours
order: 408
description: Presets, gradients, rainbow, sequence and random colours, named and hex colours, styles, group defaults and effects.
icon: palette
---

Module: `colours`. A player gives their own words a colour with `/chatcolor` or its [window](../menus/colours.md).

## Presets

| Preset | Pattern | Colours | Node default |
|---|---|---|---|
| `white` | solid | `#FFFFFF` | everyone |
| `sky` | solid | `#48CAE4` | everyone |
| `mint` | solid | `#4ECCA3` | everyone |
| `rose` | solid | `#FF6B8B` | everyone |
| `butter` | solid | `#FFE66D` | everyone |
| `sunset` | gradient | `#FF6B8B`, `#FFA07A`, `#FFE66D` | op |
| `ocean` | gradient | `#48CAE4`, `#6C8DFB` | op |
| `rainbow` | rainbow | every hue | op |
| `candy` | sequence | `#FF6B8B`, `#FFFFFF` | op |
| `confetti` | random | `#B388FF`, `#4ECCA3`, `#FFE66D`, `#48CAE4`, `#FF6B8B` | op |

Each preset needs `uxmchat.colour.preset.<id>`, or the `permission` it names.

| Pattern | Draws |
|---|---|
| `solid` | One colour |
| `gradient` | A blend across the line |
| `rainbow` | Every hue |
| `sequence` | The colours letter by letter in turn |
| `random` | A colour per letter |

## Named and hex colours

With `uxmchat.colour.named`, a player may pick any colour under `named`: `rose`, `peach`, `butter`, `mint`, `sky`,
`corn`, `lilac`. With `uxmchat.colour.hex`, any `#rrggbb`: `/chatcolor set #FF8800`.

## Styles

`/chatcolor style <style>` turns a style on or off over the colour: `bold`, `italic`, `underlined`, `strikethrough`,
`obfuscated`, each with `uxmchat.colour.style.<name>`, all given to operators only.

## Group defaults

`group-defaults` gives the players of a permission group a preset until they pick one: `vip = "sunset"`.

## When a colour is lost

The choice is checked at join. A player who no longer holds the preset, or whose preset is gone, loses it, and the
`lapsed` effect runs.

## Effects

| Moment | Shipped lines |
|---|---|
| `set` | `[message] @colour.set` (with `<sample>`, the player's name in the new colour), a chime |
| `cleared` | `[message] @colour.cleared`, a break sound |
| `lapsed` | `[message] @colour.lapsed` |

Leave a list out to keep the shipped one; write `[]` for silence.

## Moving from uxm-chat-color

Its presets and every player's choice are read on the first start. See [Migration](../integrations/migration.md).

Related: [colours.conf](../config/colours-conf.md), [Colours window](../menus/colours.md)
