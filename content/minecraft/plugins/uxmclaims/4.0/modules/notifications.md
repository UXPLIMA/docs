---
title: Notifications
order: 1311
description: The title, action bar or chat line a player sees when walking into or out of a land.
icon: bell
---

The notifications module tells a player when they walk into a land and when they walk out of it. The text and
how it shows (a title, the action bar, a chat line or nothing) are set in the language file. Each player can
turn the notices off for themselves with `/claim toggle titles`.

Module `notifications` · enabled by default · `modules/notifications/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim toggle titles` | Turns land enter and leave notices off or on for you. The choice is saved. | `uxmclaims.command.notifications.toggle` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.notifications.toggle` | everyone | `/claim toggle titles`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `default-player-setting` | `true` | Whether players who never used `/claim toggle titles` get the notices. Set `false` to make them opt-in. |
| `cooldown-millis` | `2000` | Milliseconds before the same notice for the same land is shown to a player again, so walking back and forth over an edge does not repeat it. |
| `leave-when-entering-another` | `false` | Show the leave notice when walking straight from one land into another. `false` shows only the new land's enter notice, as in 3.x. |
| `notify-members` | `true` | Show the enter notice to the land's own owner and members too. |

## Notice texts

The texts are in `modules/notifications/lang/<language>/messages.yml`:

| Key | Default type | Shown when |
|---|---|---|
| `land-enter` | `TITLE` | A player walks into a land. |
| `land-leave` | `TITLE` | A player walks out of a land into the wilderness. |

Each entry has a `type` (`TITLE`, `ACTIONBAR`, `CHAT` or `NONE`), a `text`, and an optional `sound`, `volume`
and `pitch`. For `TITLE`, the first line of `text` is the title and the second the subtitle. `NONE` turns that
notice off for everyone. The texts take `<land_name>` and `<owner_name>`; `owner-server` in the same file is the
owner name shown for admin lands.

## Notes

- The per-player choice is stored in the database, so it survives a relog and a restart.
- The notices follow the land, not areas inside it, towns or nations: walking between two chunks of the same
  land shows nothing.
- A teleport into a land counts as walking in.

Related: [Lands](lands.md), [Borders](borders.md), [Holograms](holograms.md)
