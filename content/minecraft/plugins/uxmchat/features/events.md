---
title: Server lines
order: 313
description: Join, first join, quit, death, kick, advancement and unknown command lines, per group and per cause, and network lines.
icon: door-open
---

Module: `events`. The lines the server says on its own, from `modules/events/events.conf`.

| Event | Shipped line | Per |
|---|---|---|
| `join` | `+ Name` | group: `vip` reads `+ Name is here` |
| `first-join` | `Welcome Name to the server!` | |
| `quit` | `- Name` | |
| `death` | the game's own line | cause: `player`, `fall`, `lava`, `drowning`, `void` |
| `kick` | `- Name was kicked: reason` | |
| `advancement` | `Name made the advancement X` | |
| `unknown-command` | `There is no command /x. Try /help.` | `rewrite = true` replaces the server's own line |

| Placeholder | In |
|---|---|
| `<player>`, `<display>` | Every event |
| `<killer>`, `<weapon>` (with its tooltip), `<vanilla>` (the game's line) | `death` |
| `<reason>` | `kick` |
| `<advancement>` | `advancement` |
| `<command>` | `unknown-command` |
| `<server>`, `<from>`, `<to>` | Network lines |

A line that starts with `@` is a line of the messages files in each reader's language; an empty line says nothing. A
death cause is the game's damage cause in lower case; a death by another player is `player`. A vanished player comes
and goes without a line. Each player can turn join and quit lines and death lines off in their settings.

## Network lines

Module: `network-events`. On a network with the proxy plugin, `network.enabled = true` (on every server) makes the
proxy's join, switch and quit lines replace each server's own, so a player moving between servers is not announced as
leaving one and joining another.

| Key under `network` | Default | Meaning |
|---|---|---|
| `enabled` | `false` | The proxy's lines replace this server's |
| `join`, `switch`, `quit` | `@events.network-*` | The lines |
| `resend-after-switch` | `missed` | What a player who moved gets again of network channel lines: `missed` (said while between servers), `recent` (the last lines they could read) or `off` |
| `resend-lines` | `10` | How many |

Related: [events.conf](../config/events-conf.md), [A network](network.md)
