---
title: opening.conf
order: 602
description: Who sees a reveal, skipping, the bulk cap, the panes of the reveal windows, and the preview wait.
icon: play
---

Everything sits in one `opening` block.

| Key | Default | Meaning |
|---|---|---|
| `default-visibility` | `"opener"` | Who sees the reveal of a crate that does not say: `opener`, `radius` or `everyone` |
| `default-radius` | `16` | Blocks, for `radius`. It says who does see the reveal |
| `skippable` | `true` | A shift click or closing the window ends the reveal and pays out at once |
| `skip-after-ms` | `500` | How long the reveal runs before a skip is accepted. `0` allows one from the first frame |
| `bulk-cap` | `25` | The most openings one `/crates massopen` makes, unless the crate sets its own `bulk.cap` |
| `window.fill` | `"GRAY_STAINED_GLASS_PANE"` | The pane in every uncovered slot of a reel, turn or pick window. `""` for none |
| `window.pointer` | `"LIME_STAINED_GLASS_PANE"` | The pane above and below the slot the reel and the turn stop on. `""` for none |
| `preview-cooldown-ms` | `2500` | How long a player waits between two previews. `0` allows any rate |

A crate's own `visibility` block wins over the two defaults. A window an animation file draws puts its own pointer in
its `decor`.

Related: [Opening](../features/opening.md), [Reveals](../features/reveals.md)
