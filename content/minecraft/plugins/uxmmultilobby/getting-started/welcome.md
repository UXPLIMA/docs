---
title: Welcome
order: 201
description: What uxmMultiLobby does, who it is for, and where to read next.
icon: hand
---

A network with more than one lobby needs a way between them. uxmMultiLobby puts a window of every lobby on every lobby
server, each with its live player count, and moves a player to the one they click. `/quicklobby` picks the least loaded
lobby for them, and a hotbar item opens the window.

## What it does

| Part | What it gives you |
|---|---|
| [The lobby window](../menus/lobbies.md) | One icon per lobby, with its count, its state and what a click does |
| [Live counts](../features/live-counts.md) | From the proxy, a server list ping, or Redis |
| [Load balancing](../features/balancing.md) | `/quicklobby` sends a player where there is the most room, by weight |
| [Moving](../features/moving.md) | Through the proxy, with a cooldown, permissions and a reason for every refusal |
| [The hotbar tool](../features/tool.md) | An item that opens the window, per world |
| [Outcomes](../features/outcomes.md) | What every move says and plays, as action lines |

Every window is a file, every command can be renamed, and every line a player reads is in a language file. The plugin
needs nothing else installed.

## Read next

- [Installation](installation.md): the jar on every lobby.
- [Concepts](concepts.md): lobbies, this lobby, sources, states.
- [Your first network](first-network.md): three lobbies in ten minutes.

Related: [Commands](../commands/overview.md), [Permissions](../permissions/reference.md)
