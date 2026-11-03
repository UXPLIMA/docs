---
title: colours.conf
order: 508
description: Presets and their keys, named colours, group defaults and effects.
icon: palette
---

`modules/colours/colours.conf`, read at start and on every reload.

## presets

| Key | Meaning |
|---|---|
| `display` | How it is shown, MiniMessage or `@key` |
| `icon` | The item its button shows; left out, the dye nearest its first colour |
| `order` | Where it sits in the window, lowest first |
| `permission` | Who may pick it; empty defaults to `uxmchat.colour.preset.<id>` |
| `pattern` | `solid`, `gradient`, `rainbow`, `sequence` or `random` |
| `colours` | `#rrggbb` codes, or MiniMessage colour names such as `gold` |

```hocon
sunset { display = "<gradient:#FF6B8B:#FFA07A:#FFE66D>Sunset", icon = "ORANGE_DYE", pattern = "gradient", colours = [ "#FF6B8B", "#FFA07A", "#FFE66D" ] }
```

## named

The colours a player with `uxmchat.colour.named` may type by name: `rose #FF6B8B`, `peach #FFA07A`, `butter #FFE66D`,
`mint #4ECCA3`, `sky #48CAE4`, `corn #6C8DFB`, `lilac #B388FF`.

## group-defaults

`vip = "sunset"`: the preset a player in a group has until they pick one.

## effects

`set`, `cleared` and `lapsed`, each a list of action lines. `colour.set` binds `<sample>`. Leave a list out to keep the
shipped one; write `[]` for silence.

Related: [Chat colours](../features/colours.md)
