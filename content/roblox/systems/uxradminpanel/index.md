---
title: uxrAdminPanel
order: 41
description: An in-game admin panel with ranks, 346 commands, moderation, logs and cross-server control.
icon: shield-check
---

A full administration system: a rank ladder, a searchable panel, a command bar, a chat
prefix, and 346 commands covering movement, cosmetics, combat, moderation, teleportation,
inventory, messaging, server operations and utilities.

Everything is server-authoritative. The panel is a view; every command is re-checked
against the actor's rank before it runs.

## Pages

- [Setup](setup.md)
- [Ranks and permissions](ranks.md)
- [Running commands](commands.md)
- [Choosing targets](targets.md)
- [Command reference](reference.md)
- [Moderation](moderation.md)
- [Server operations](server.md)
- [Cross-server](crossserver.md)
- [Logs, analytics and webhooks](logs.md)
- [The interface](interface.md)
- [Settings](settings.md)

## What it does

| | |
|---|---|
| Ranks | Six by default, granted by user id, gamepass, asset, group rank or team |
| Immunities | Per-rank flags that block being banned, kicked, muted or jailed |
| Commands | 346, each with a minimum rank, typed arguments and its own logging policy |
| Three ways in | The panel, a command bar, and a chat prefix |
| Targets | Keywords, partial names, teams, ranks, nearest, friends, and exclusions |
| Moderation | Bans through the Roblox ban API, mutes, warnings with an auto-action, notes, jails |
| Server control | Lock, lockdown, PVP, shutdown, time, lighting, terrain, teams, leaderstats, warps |
| Cross-server | Announcements, votes, lockdowns, shutdowns and migrations across every server |
| Logs | A structured action log, an analytics page, and Discord webhooks |

## The shape of it

Three config files. `Permissions.luau` decides who may do what, `Commands.luau` describes
every command and its arguments, and `Settings.luau` holds the prefix, the rate limits and
the message templates.

Each command exists twice: metadata in `Shared/Config/Commands.luau` and an implementation
in `Server/Commands/<Category>.luau`. The dispatcher joins them, resolves the targets,
parses the arguments by type and runs the code once per target.

<Callout type="danger" title="Replace the owner ids and the shared secret before you publish">

`Permissions.luau` ships with somebody else's user id as Owner and somebody else's group as
the admin group. `Server/SharedSecret.luau` ships with a placeholder value.

Both are covered in [Setup](setup.md), and neither is optional.

</Callout>
