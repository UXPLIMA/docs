---
title: Areas
order: 1302
description: Block-precise boxes inside a land with their own members, roles and flags.
icon: box-select
---

An area is a box of blocks inside one land, such as a shop or a rented room, with its own members, roles and flags. Anything the area leaves unset falls through to the land. Select two corners with the selection tool or `/claim area select`, then run `/claim area create <name>`.

Module `areas` · disabled by default · `modules/areas/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim area`, `/claim areas`, `/claim area list` | Opens the areas of the land you stand in | `uxmclaims.command.areas.list` |
| `/claim area menu [area]` | Opens the menu of an area | `uxmclaims.command.areas.list` |
| `/claim area info [area]` | Shows an area, or the one you stand in | `uxmclaims.command.areas.info` |
| `/claim area show [area]` | Draws the outline of an area | `uxmclaims.command.areas.info` |
| `/claim area tool` | Gives you the selection tool | `uxmclaims.command.areas.select` |
| `/claim area select <pos1\|pos2\|chunk\|expand\|clear>` | Sets a corner at the block you look at (else your feet), selects your chunk, stretches the selection to full height, or clears it | `uxmclaims.command.areas.select` |
| `/claim area create <name>` | Creates an area from your selection in the land you stand in | `uxmclaims.command.areas.create` |
| `/claim area redefine <area>`, `/claim area resize` | Moves or resizes an area to your selection | `uxmclaims.command.areas.redefine` |
| `/claim area delete <area>` | Deletes an area, after `/claim area confirm` | `uxmclaims.command.areas.delete` |
| `/claim area rename <area> <name>` | Renames an area | `uxmclaims.command.areas.rename` |
| `/claim area setspawn <area>` | Sets the area spawn where you stand | `uxmclaims.command.areas.setspawn` |
| `/claim area spawn <area>` | Teleports to an area | `uxmclaims.command.areas.spawn` |
| `/claim area trust <area> <player> [role] [duration]` | Adds a member, with a role of the area or the land, for a while if a duration such as `3d` is given | `uxmclaims.command.areas.trust` |
| `/claim area untrust <area> <player>` | Removes a member | `uxmclaims.command.areas.untrust` |
| `/claim area leave <area>` | Leaves an area you are a member of | `uxmclaims.command.areas.trust` |
| `/claim area setrole <area> <player> <role>` | Gives an area member another role | `uxmclaims.command.areas.role` |
| `/claim area setflag <area> <flag> <allow\|deny\|unset>` | Sets an area flag; `unset` uses the land's | `uxmclaims.command.areas.flag` |
| `/claim area role create <area> <role>` | Adds a custom role to an area | `uxmclaims.command.areas.role` |
| `/claim area role delete <area> <role>` | Deletes a custom area role | `uxmclaims.command.areas.role` |
| `/claim area role perm <area> <role> <permission> <allow\|deny\|unset>` | Sets a permission of an area role; `unset` uses the land's | `uxmclaims.command.areas.role` |

With the tool, left click sets the first corner and right click the second.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.areas.<sub>` | everyone | The command of the table above |
| `uxmclaims.ability.area.create` | everyone | Create areas in lands you manage |
| `uxmclaims.ability.area.delete` | everyone | Delete areas in lands you manage |
| `uxmclaims.ability.area.redefine` | everyone | Move and resize areas in lands you manage |
| `uxmclaims.admin.areas` | op | Manage every area, without limits, costs or land roles |

| Role permission | Default holder | Allows |
|---|---|---|
| `areas:manage` | Owner | On a land role: run every area of the land. On an area role: run that area. |

| Limit or cost | Default | Applies to |
|---|---|---|
| `uxmclaims.limit.area.<n>` | `max-per-land` (10), highest wins | Areas per land, read from the land owner |
| Cost `areas:create` | 0 | Creating an area |

## Settings

| Key | Default | What it does |
|---|---|---|
| `selection.tool` | `GOLDEN_HOE` | Item that selects corners |
| `selection.max-volume` | `200000` | Largest area in blocks; admins are not limited |
| `selection.show-seconds` | `30` | How long an outline stays visible |
| `selection.interval-ticks` | `10` | Ticks between outline draws |
| `selection.max-distance` | `48` | Outline parts farther away are not drawn |
| `selection.full-height` | `false` | A selection covers the whole world height |
| `max-per-land` | `10` | Areas per land when the limits module does not decide. With [Limits](limits.md) on, it is copied into `limits.yml` at the first start; change it there afterwards |
| `inherit-land-roles` | `true` | Area members may be given one of the land's roles |
| `naming.pattern` | `[\p{L}\p{N}_\-]+` | Characters a new or renamed area name may use; commands still find areas whose name has spaces |
| `naming.min-length` | `2` | Shortest area name |
| `naming.max-length` | `32` | Longest area name |
| `tracking.interval-ticks` | `10` | Ticks between checks of where players stand; `0` turns enter and leave events off |
| `tracking.notify` | `true` | Tell players the area name when they walk in or out |
| `default-roles.<id>` | `manager`, `member`, `visitor` | Roles every new area starts with |

Each entry of `default-roles` has `type` (`MEMBER` for new members, `VISITOR` for non-members, `OWNER` or `CUSTOM`), `priority` (higher outranks lower) and `permissions` (key to `ALLOW` or `DENY`). A key not listed is left to the land. The shipped `visitor` role is empty, so non-members follow the land's rules.

## Notes

- An area lies wholly inside its land's chunks, in the land's world, and shares no block with another area of the land, so areas cannot nest.
- Land managers (the owner, or a land role with `areas:manage`) run every area of the land. Area members whose area role has `areas:manage` run that one area.
- Area bans and per-member permission overrides on areas are not available yet.
- Timed area trust runs out on a 60 second timer, like timed land trust.

Related: [Lands](lands.md), [Plots](plots.md), [Limits](limits.md)
