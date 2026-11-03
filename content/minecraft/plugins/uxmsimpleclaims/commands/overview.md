---
title: Overview
order: 201
description: The two commands, their words in Turkish, and how a missing permission is answered.
icon: terminal
---

| Command | Aliases | Turkish | For |
|---|---|---|---|
| `/claim` | `/claims` | `/arazi` | Players |
| `/claimadmin` | `/uxmsimpleclaims` | `/araziyonetim` | Staff |

Every word is in [commands.conf](../config/commands-conf.md). A Turkish word is shown to, and works for, Turkish
readers; the Turkish word replaces the last word of a branch. Every branch of `/claim` except `auto` and `lang` needs
`uxmsimpleclaims.use`; every `/claimadmin` branch needs `uxmsimpleclaims.admin.<node>`. Every `/claim` branch is for
players only.

A `[duration]` is a span such as `30m`, `12h` or `7d`.

A player node is held by everybody, so its command stays visible and a player without it is refused with
`command.no-permission`. A staff node hides its command.

Related: [Permissions](../permissions/reference.md)
