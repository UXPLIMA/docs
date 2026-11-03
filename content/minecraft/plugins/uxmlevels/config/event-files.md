---
title: Event files
order: 603
description: The keys of an event in events/, in one table, with the rules a reload checks.
icon: file-text
---

Any `.conf` file in `events/` holds an `events { }` block; the file name is only for you. The keys are explained on
[Events](../tracks/events.md).

| Key | Default | Rule |
|---|---|---|
| the block name | | 1 to 48 lower-case letters, digits and hyphens; unique across every file |
| `track` | `"global"` | A track id. One no file declares makes the event pay nothing, with a console line |
| `type` | | `playtime`, `mining`, `combat`, `fishing`, `eating`, `crafting` or `chat` |
| `filter` | `[]` | Names, without regard to case |
| `amount` | `1` | Not negative |
| `minimum`, `maximum` | | Not negative; `maximum` at least `minimum` |
| `chance` | `1.0` | `0.0` to `1.0` |
| `cooldown-ms` | `0` | Not negative |
| `action-bar` | `true` | |
| `minimum-length` | `0` | For `chat`; not negative |

A duplicate id or a key that breaks a rule stops the load, and the console names the file and the event.

Related: [Events](../tracks/events.md), [Track files](track-files.md)
