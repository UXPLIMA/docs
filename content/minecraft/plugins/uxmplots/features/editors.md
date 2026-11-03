---
title: In-game editors
order: 417
description: The world editor, the contest editor, and how typing is asked for.
icon: pencil
---

## The world editor

`/uxmplots world <world>` (`uxmplots.admin.world`): twelve buttons, in `editor/world.conf` order: plots per player, the
starter layout, the first plot's price, what each plot after adds, how the price climbs, which money, the refund per
cent, the per kind entity cap, the game mode, the total entity cap, who may visit a new plot, and whether edits are held
inside the plot. Number buttons step by `step` and by `shift` with shift held, within `min` and `max`. There is no
delete: deleting a world file would forget a world still on disk.

## The contest editor

`/contest edit <contest>` (`uxmplots.contest.edit`): buttons for the contest file's fields, laid out by
`editor/contest.conf`.

Both files hold no words; every label is a message key. Delete one and the editor returns to the shipped shape.

## Typing

`text-input.conf`:

| Key | Default | Meaning |
|---|---|---|
| `default-mode` | `anvil` | `anvil`, `chat`, `sign` or `dialog` |
| `cancel-keywords` | `cancel`, `iptal` | Abort the prompt |
| `modes."plots.world-editor"` | `anvil` | |
| `modes."plots.contest-editor"` | `anvil` | |
| `modes.search` | `chat` | The gallery and hall of fame search |

`dialog` falls back to sign, then anvil, on a server before 1.21.6. A Bedrock player always gets a form.

Related: [Admin commands](../commands/admin.md)
