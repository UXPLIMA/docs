---
title: Reference
order: 701
description: Every permission node of uxmGlow with its default.
icon: list-checks
---

| Node | Grants | Default |
|---|---|---|
| `uxmglow.use` | `/glow`, the picker, and every subcommand under `/glow` | everybody |
| `uxmglow.colour.<id>` | One colour, by its id | operators |
| `uxmglow.colour.*` | Declared, but lists no children: it works through a permission plugin's wildcard | operators |
| `uxmglow.cycle` | The cycling mode | operators |
| `uxmglow.language` | `/glow lang`, `lang set`, `lang reset` | everybody |
| `uxmglow.admin.language` | `/glow lang set-player`, `lang reset-player` | operators |
| `uxmglow.admin.reload` | `/uxmglow reload` | operators |
| `uxmglow.admin.version` | `/uxmglow version` | operators |
| `uxmglow.admin.doctor` | `/uxmglow doctor` | operators |
| `uxmglow.admin.*` | Declared as the family; lists no children | operators |

The ids are the sixteen the client can draw: `black`, `dark_blue`, `dark_green`, `dark_aqua`, `dark_red`, `dark_purple`, `gold`, `gray`, `dark_gray`, `blue`, `green`, `aqua`, `red`, `light_purple`, `yellow`, `white`.

Every colour of a cycle is checked and not only the first, so a player who holds red and blue but not gold cannot reach
gold by hiding it in the middle of a list.

A colour a player no longer holds stops being drawn the next time they join. The choice is stored, and the permission
is asked again when it comes back, which is what makes a colour that came with a rank leave with it.

```
lp group vip permission set uxmglow.colour.aqua true
lp group mvp permission set uxmglow.colour.* true
lp group mvp permission set uxmglow.cycle true
```

Related: [Colours and cycles](../features/colours-and-cycles.md)
