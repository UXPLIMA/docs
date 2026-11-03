---
title: Punishment plugins
order: 802
description: Handing /mute to the punishment plugin you run, reading its mutes, and the ladder's mutes.
icon: gavel
---

Module: `punishment-plugins`. When uxmEssentials runs its moderation module, or LiteBans, LibertyBans, AdvancedBan or
Essentials is installed, that plugin keeps `/mute` and `/unmute` with its records and history.

| uxmChat | Does |
|---|---|
| `/mute`, `/unmute` | Registers neither |
| Muted players | Reads the plugin's mutes to keep them quiet, except LibertyBans', whose own chat block does that |
| The filter ladder | Its `[console] mute` lines run the plugin's own command |

A staff member who reaches uxmChat's mute path anyway is told mutes belong to that plugin, by name.

Without any of them, uxmChat keeps mutes itself; see [Mutes](../features/mutes.md).

Related: [Mutes](../features/mutes.md), [uxmEssentials](uxmessentials.md)
