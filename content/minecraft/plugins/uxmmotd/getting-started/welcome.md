---
title: Welcome
order: 201
description: What uxmMOTD does, who it is for, and where to read next.
icon: hand
---

The server list is the first thing a player sees of a server, before they join. uxmMOTD writes all of it: the two lines
under the name, the picture beside them, the numbers on the right, the list that appears over those numbers, and what
the entry says while the server is closed.

## What it does

| Part | What it gives you |
|---|---|
| [Entries](../features/entries.md) | Two lines and an icon, several of them rotating in order or at random |
| [Centring and colours](../features/centring-and-colours.md) | Lines centred by font pixels, colours named by role, the network's name painted across the theme |
| [The player count](../features/player-count.md) | Real, fixed, or always one short of full, with an offset |
| [The hover list](../features/hover.md) | The lines shown over the player count |
| [The version text](../features/version.md) | Your own text in place of the protocol name |
| [Icons](../features/icons.md) | 64 by 64 pictures, one per entry |
| [Maintenance](../features/maintenance.md) | One command closes the server: its own entry, a kick screen, exempt players |

One source tree ships two jars: one for a Paper server and one for a Velocity proxy. Every line a player reads in chat or
on the kick screen is in a language file.

## Read next

- [Installation](installation.md): which jar goes where.
- [Concepts](concepts.md): entries, roles, the two platforms.
- [Your first entry](first-entry.md): write the server list entry of your network.

Related: [Commands](../commands/overview.md), [config.conf](../config/config-conf.md)
