---
title: Reference
order: 801
description: What uxmJoinItems reads from PlaceholderAPI, and why it publishes nothing.
icon: list
---

uxmJoinItems registers no PlaceholderAPI placeholders of its own.

It reads other plugins' placeholders in two places, with PlaceholderAPI installed:

| Where | Reads |
|---|---|
| `skip-when` | Whether a player is busy; see [Guards and regions](../features/guards-and-regions.md) |
| Action lines | Any placeholder in a message, a title or a command |

`%player%` in a command and `<player>` in a message are this plugin's own and need no PlaceholderAPI.

Related: [Clicks](../features/clicks.md)
