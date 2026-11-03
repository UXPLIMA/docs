---
title: Welcome
order: 201
description: What uxmGlow does, who it is for, and where to read next.
icon: hand
---

uxmGlow gives a player an outline they choose and everybody can see, through walls and across a lobby. A staff member
in a crowd, a tournament team, the host of an event: all findable at a glance, with nothing to hand out and nothing to
wear.

A player opens `/glow`, clicks a colour, and glows in it. With one permission more they set several colours instead
of one and the outline steps through them. The choice survives a rejoin and a restart, and it is checked against the
permission every time it comes back, so a colour that arrived with a rank leaves with the rank.

## What it does

| Part | What it gives you |
|---|---|
| Sixteen colours | Every colour a client can draw an outline in, one node each |
| Cycles | Two to eight colours stepping on a timer, under one more node |
| A picker | One window, every colour dyed in its own shade, a file you can change |
| Restoring | The glow comes back at join, and only if the player still holds it |
| A network | One database shared by every server, the choice following the player |
| Nametags | The colour is handed to uxmLib's shared nametag, so a chat tag and a glow sit on one name |

Every window is a file, every command can be renamed, and every line a player reads is in a language file you can add
to. The plugin needs nothing else installed: no economy, no PlaceholderAPI, no permissions plugin beyond the one your
server already runs.

## Read next

- [Installation](installation.md): drop the jar in.
- [Concepts](concepts.md): colours, cycles, and how a glow is drawn.
- [Your first glow](first-glow.md): give a rank its colours.

Related: [Commands](../commands/overview.md), [Permissions](../permissions/reference.md)
