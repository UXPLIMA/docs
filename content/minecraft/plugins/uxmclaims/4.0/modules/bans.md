---
title: Bans
order: 1305
description: Keeping a player off a land for good or for a set time.
icon: ban
---

A land ban keeps a player off one land: they cannot walk in, teleport in or do anything there, and a banned member loses their membership. Bans can be permanent or temporary and carry a reason. Managers ban with `/claim ban` and see the list with `/claim bans`.

Module `bans` · enabled by default · `modules/bans/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim ban <player> [duration] [reason]` | Bans a player from the land; a first word such as `3d` is read as the duration | `uxmclaims.ability.member.ban` |
| `/claim unban <player>` | Lifts a player's ban | `uxmclaims.ability.member.unban` |
| `/claim bans` | Opens the list of players banned from the land | `uxmclaims.ability.member.unban` |

All three act on the land you stand in, else your only land.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.ability.member.ban` | everyone | Ban players from lands you manage |
| `uxmclaims.ability.member.unban` | everyone | Lift bans on lands you manage |
| `uxmclaims.bypass.enter` | op | Enter lands you are banned from |
| `uxmclaims.admin.bans` | op | Manage bans on every land |

| Role permission | Allows |
|---|---|
| `bans:manage` (3.x `MANAGE_BANS`) | Ban and unban on the land |

| Limit | Default | Applies to |
|---|---|---|
| `uxmclaims.limit.ban.<n>` | 25, highest wins | Bans per land, read from the owner when online, else from the player banning |

## Settings

| Key | Default | What it does |
|---|---|---|
| `eject-on-ban` | `true` | Put a banned player outside the land straight away |
| `allow-temporary-bans` | `true` | Allow a duration in `/claim ban` |
| `kick-members` | `true` | Take a banned member's membership away |
| `eject-search-radius` | `16` | Chunks to search for a spot outside the land when putting a player out |
| `purge-interval` | `5m` | How often lapsed temporary bans are removed from the database (at least 30 seconds) |

`/uxm reload` applies changes.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_bans_count%` | Bans on the player's current land |
| `%uxmclaims_bans_here%` | `true` when the player is banned from the land they stand in |

## Notes

- The owner cannot be banned, and you cannot ban yourself.
- A ban is checked before every role and flag, so even a role that allows everything does not let a banned player act.
- With [Invites](invites.md) on, a banned player cannot be invited.
- A deleted land keeps its bans while it can be restored; they are removed for good with the land.
- Bans apply to whole lands; area bans are not available yet.

Related: [Lands](lands.md), [Invites](invites.md)
