---
title: Permissions
order: 30
description: One node per colour, one for the cycle, and the admin branch.
icon: key
---

| Node | Grants | Default |
|---|---|---|
| `uxmglow.use` | `/glow` and the picker | everybody |
| `uxmglow.colour.<id>` | One colour, by its id | nobody |
| `uxmglow.cycle` | The cycling mode | nobody |
| `uxmglow.language` | `/glow lang` and the two branches under it | everybody |
| `uxmglow.admin.reload` | `/uxmglow reload` | operator |
| `uxmglow.admin.version` | `/uxmglow version` | operator |
| `uxmglow.admin.doctor` | `/uxmglow doctor` | operator |

The ids are the sixteen the client can draw: `black`, `dark_blue`, `dark_green`, `dark_aqua`,
`dark_red`, `dark_purple`, `gold`, `gray`, `dark_gray`, `blue`, `green`, `aqua`, `red`,
`light_purple`, `yellow`, `white`.

Every colour of a cycle is checked and not only the first, so a player who holds red and blue
but not gold cannot reach gold by hiding it in the middle of a list.

A colour a player no longer holds stops being drawn the next time they join. The choice is
stored, and the permission is asked again when it comes back, which is what makes a colour that
came with a rank leave with it.
