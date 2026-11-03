---
title: Track files
order: 602
description: The keys of a file in tracks/, in one table, with the rules a reload checks.
icon: file-text
---

One file per track in `tracks/`; the file name is the id. The keys are explained on [Tracks](../tracks/tracks.md),
[Rewards](../tracks/rewards.md) and [Heads-up](../tracks/heads-up.md).

| Key | Default | Rule |
|---|---|---|
| `display-name` | the id | Not blank. `@key` reads a line of the messages files |
| `icon` | `"EXPERIENCE_BOTTLE"` | A material |
| `maximum-level` | `100` | `2` to `10000` |
| `base-cost` | `100` | At least `1` |
| `growth` | `1.15` | At least `1.0`; the curve may not overflow |
| `hud.experience-bar` | `false` | One track per surface |
| `hud.boss-bar` | `false` | One track per surface |
| `hud.action-bar` | `false` | One track per surface |
| `rewards.<id>.level` | `1` | At least `1` |
| `rewards.<id>.icon` | `"CHEST"` | Not blank |
| `rewards.<id>.name` | the id | `@key` or your own words |
| `rewards.<id>.lore` | `[]` | Lines, each `@key` or your own words |
| `rewards.<id>.actions` | `[]` | Action lines |
| `rewards.<id>.claim` | `"manual"` | `manual`, `automatic` or `auto` |

A file name that is not a valid id, or a key that breaks a rule, stops the load, and the console names the file.

Related: [Tracks](../tracks/tracks.md), [Event files](event-files.md)
