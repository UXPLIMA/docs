---
title: Placeholders
order: 280
description: The PlaceholderAPI expansion for scoreboards, tab lists and holograms.
icon: braces
---

Identifier `uxmbuilders`, registered automatically when PlaceholderAPI is installed.

A player can run more than one build. The build placeholders report the **first** active one, which is
what a scoreboard line wants.

## Builds

| Placeholder | Returns | With no build |
|---|---|---|
| `%uxmbuilders_active%` | Builds running, queued or paused | `0` |
| `%uxmbuilders_current_package%` | The package's display name, without colours | `-` |
| `%uxmbuilders_state%` | `BUILDING`, `QUEUED` or `PAUSED` | `-` |
| `%uxmbuilders_progress%` | Progress with a percent sign, `42%` | `0` |
| `%uxmbuilders_progress_raw%` | The number alone, `42` | `0` |
| `%uxmbuilders_progress_bar%` | A ten-segment bar | an empty bar |
| `%uxmbuilders_placed%` | Blocks placed | `0` |
| `%uxmbuilders_total%` | Blocks in the build | `0` |
| `%uxmbuilders_eta%` | Time left, `3m 20s` | `-` |
| `%uxmbuilders_queue_position%` | Builds ahead of it in the server queue | `0` |

`queue_position` is `0` while the build is running as well as when it is first in line; read
`state` to tell the two apart.

## Builder

| Placeholder | Returns |
|---|---|
| `%uxmbuilders_builder%` | The selected builder's name, without colours |
| `%uxmbuilders_builder_level%` | Its level |
| `%uxmbuilders_builder_speed%` | Its blocks per second at that level |

The builder placeholders return `-` for an offline player, whose profile is not in memory.

Related: [Messages and menus](config/messages-and-menus.md)
