---
title: Reference
order: 801
description: The tags that work in the server list lines, and why there is no PlaceholderAPI.
icon: list
---

There is no PlaceholderAPI integration: a ping comes from a client that has not joined, so there is no player to ask
about. Two tags work in every line of the server list entry (the `motd` lines, `hover`, `version.text` and their
`maintenance` counterparts):

| Tag | Value |
|---|---|
| `<online>` | The online count this ping shows, after `players.offset` |
| `<max>` | The maximum this ping shows, after `players.mode` |

The message files take their own values, each named in the line that uses it: `<version>` in `admin.version`, `<ms>` in
`admin.reloaded`, `<check>` and `<said>` in the doctor lines.

Related: [The player count](../features/player-count.md)
