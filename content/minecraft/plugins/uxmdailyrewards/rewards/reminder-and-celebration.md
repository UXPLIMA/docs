---
title: Reminder and celebration
order: 404
description: The nudge a player gets on join with a reward waiting, and the lines that run after every claim.
icon: bell
---

## The reminder

```hocon
reminder {
  enabled = true
  delay   = "5s"
  effects = [
    "[subtitle] @reminder.title | @reminder.subtitle 0.5 3.0 0.5",
    "[actionbar] @reminder.action-bar",
    "[sound] block.note_block.pling 1 1.4"
  ]
}
```

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Remind at all |
| `delay` | `"5s"` | How long after the join; a title shown during the join is a title nobody reads |
| `effects` | a title and subtitle, an action bar line, a pling | What the reminder does, in order |

The reminder runs only when the player has a day ready; otherwise nothing happens. The lines are written as in a
[day file](day-files.md#writing-a-line), with `%player%`, `%day%` and `%streak%`, and the `@` lines read `<day>` as
the day that is waiting. The shipped words:

| Key | English |
|---|---|
| `reminder.title` | `Daily reward` |
| `reminder.subtitle` | `Day <day> is waiting for you.` |
| `reminder.action-bar` | `Day <day> is waiting for you.` |

A `config.conf` from an earlier release that still has `reminder.title`, `reminder.action-bar`, `fade-in`, `stay`,
`fade-out` or `actions` keeps working: those keys are folded into the list, ahead of what it holds.

## The celebration

```hocon
celebration {
  actions = [
    "[sound] entity.player.levelup 1 1.2",
    "[particle] totem_of_undying 40 0.6"
  ]
}
```

`celebration.actions` runs after every claim, right after the day's reward, for every day and every rank. Empty it
for a quiet claim. `/uxmdailyrewards grant` does not run it.

Related: [Day files](day-files.md), [Messages](../config/messages.md)
