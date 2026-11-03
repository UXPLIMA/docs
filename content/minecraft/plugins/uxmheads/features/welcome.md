---
title: The welcome
order: 402
description: The first-join and join greetings, their delay, and their lines.
icon: party-popper
---

When a player joins, once their face is ready and `welcome.delay` (500ms) later, they receive the welcome. A short wait
puts it under the lines other plugins send on a join.

| List | Who receives it |
|---|---|
| `welcome.first-join` | A player who has never played here. Empty: they receive `join` |
| `welcome.join` | Every other join |

Each list is effect lines run in order: `[message]`, `[sound]`, `[title]`, `[actionbar]`, `[bossbar]`, `[particle]`,
`[console]`, `[player]` and the rest of the effect grammar. A text starting with `@` names a line of the language files,
read in the player's language. `<player>` is the name; `<face:1>` to `<face:8>` are the rows.

## Shipped

| Line | Join | First join |
|---|---|---|
| 1 | the row | the row |
| 2 | `Welcome back` as a header | `Welcome` as a header |
| 3 | the row | the row |
| 4 | `Good to see you again, <player>.` | `Glad to have you here, <player>.` |
| 5 | `Everything is where you left it.` | `This is your first time on this server.` |
| 6 | `Have fun, and be kind to other players.` | the same |
| 7, 8 | the row | the row |

A first join also plays `ui.toast.challenge_complete 0.6 1.2`.

`welcome.enabled = false` sends no welcome; the placeholders keep working.

Related: [Your first welcome](../getting-started/first-welcome.md), [Messages](../config/messages.md)
