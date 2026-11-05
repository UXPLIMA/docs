---
title: Team
order: 203
description: Members, roles and permissions, trust, bans and locking.
icon: users
---

## Members

| Command | What it does | Permission |
|---|---|---|
| `/is invite <player>` | Invites a player; they have five minutes to answer | `uxmskyblock.island.invite` |
| `/is accept` | Accepts the invite you were sent | |
| `/is deny` | Turns the invite down | |
| `/is kick <player>` | Removes a member whose role is below yours | `uxmskyblock.island.kick` |
| `/is leave` | Leaves the island you belong to; the owner cannot | |
| `/is members [list]` | Opens the [members window](../menus/team.md#members), or lists them in chat | |
| `/is role <player> <role>` | Gives a member a role below your own | |

How many members an island holds is its [member limit upgrade](../features/upgrades.md). A player who left an island
waits 24 hours before joining another, as [anti_abuse.conf](../config/anti_abuse-conf.md) ships it.

## Roles and permissions

| Command | What it does |
|---|---|
| `/is permissions` | Opens the [roles window](../menus/team.md#roles) |
| `/is permissions <role>` | Lists what a role may do |
| `/is permissions <role> <permission> on \| off` | Allows or denies one permission for a role below yours |
| `/is permissions create <name>` | Makes a role of your own; the owner only |
| `/is permissions delete <name>` | Removes a role of your own; whoever held it becomes a member again |

`/is roles` is an alias of `/is permissions`. The permissions are listed on [Roles](../features/roles.md).

## Trust, bans and locking

| Command | What it does | Permission |
|---|---|---|
| `/is trust <player> [duration]` | Lets a visitor build and use things on your island for a while: an hour unless you say, at most a day | |
| `/is untrust <player>` | Takes that trust back | |
| `/is trusted` | Lists the players you trust | |
| `/is ban <player> [reason]` | Bans a player from your island and sends them away | `uxmskyblock.island.ban` |
| `/is unban <player>` | Lifts a ban | `uxmskyblock.island.unban` |
| `/is bans` | Lists the players banned from your island | |
| `/is lock` | Closes your island to everyone but its members and allies | |
| `/is unlock` | Opens it to visitors again | |
| `/is flag [list \| <name> <value>]` | Shows or sets island settings such as fighting and monster spawns | |

How long trust lasts is in [temporary-access.conf](../config/temporary-access-conf.md).
