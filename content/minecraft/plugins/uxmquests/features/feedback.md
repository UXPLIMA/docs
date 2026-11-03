---
title: Feedback
order: 409
description: The four moments, the toast and quest bar verbs, and how a level is chosen.
icon: bell
---

Four moments tell a player what happened: `started`, `progressed`, `completed` and `paid`. Each is a list of action
lines with no ceiling. The shipped `config.conf`:

| Moment | Lines |
|---|---|
| `started` | A subtitle, a pling |
| `progressed` | An action bar line |
| `completed` | A subtitle, a toast, a chat line, a pling |
| `paid` | An action bar line, a bell |

```hocon
feedback {
  toast-icon  = "BOOK"
  toast-frame = "TASK"
  completed = [
    "[subtitle] @feedback.completed.title | @feedback.completed.subtitle 0.5 2.5 0.5"
    "[toast] @feedback.completed.toast"
    "[sound] block.note_block.pling 0.6 1.5"
  ]
}
```

Two verbs are this plugin's own:

| Verb | Draws |
|---|---|
| `[toast] <text>` | The advancement popup, with `toast-icon` (a material) and `toast-frame` (`TASK`, `GOAL` or `CHALLENGE`) |
| `[questbar] <seconds> [colour] [style] \| <text>` | A boss bar filled to how far the quest has got |

`<quest>`, `<group>`, `<done>`, `<needed>` and `<percent>` are filled on every line.

## Levels

The same `feedback { }` block may sit in `config.conf`, in a group, in a quest and in a task. The first level that
answers for a moment wins, and a list that is present stops the fallback even when it is empty, which is how one noisy
task is silenced:

```hocon
tasks = [ { id = dig, type = block-break, amount = 500, feedback { progressed = [] } } ]
```

A moment written as the older block of channel switches is still read, as the list it describes.

Related: [config.conf](../config/config-conf.md#feedback)
