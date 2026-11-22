---
title: Warps
order: 1306
description: Named teleport points inside lands, private to members or public to everyone.
icon: map-pin
---

A warp is a named teleport point inside a land. Members with the right role reach the land's private warps, and a public warp is open to every player on the server. Players create warps with `/claim warp create <name>` and browse public ones with `/claim warps`.

Module `warps` · enabled by default · `modules/warps/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim warp create <name>` | Creates a warp where you stand, in the land you stand in | `uxmclaims.command.warps.create` |
| `/claim warp delete <warp>` | Deletes a warp, after `/claim warp confirm` | `uxmclaims.command.warps.delete` |
| `/claim warp rename <warp> <name>` | Renames a warp | `uxmclaims.command.warps.rename` |
| `/claim warp move <warp>` | Moves a warp to where you stand | `uxmclaims.command.warps.move` |
| `/claim warp setpublic <warp> <true\|false>` | Makes a warp public or private | `uxmclaims.command.warps.setpublic` |
| `/claim warp teleport <warp>` | Teleports to a public warp or a warp of your lands | `uxmclaims.command.warps.teleport` |
| `/claim warp list` | Opens the warps of the land you stand in | `uxmclaims.command.warps.list` |
| `/claim warps` | Opens the list of public warps | `uxmclaims.command.warps.warps` |
| `/claim warp confirm`, `/claim warp cancel` | Confirms or cancels the pending warp action | none |

Every command node is open to everyone by default. Managing commands also need the ability node (`uxmclaims.ability.warp.create`, `.delete`, `.rename`, `.relocate` for move, `.visibility` for setpublic) and the land role. They look for the warp in the land you stand in first, then in lands you manage.

`/claim warp teleport <warp>` takes the first match in: public warps, warps of lands you own, warps of lands you are a member of. With `name-uniqueness: PER_LAND` the land you stand in comes first, and `<land>:<warp>` names one land's warp directly.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.warps.<sub>` | everyone | The command of the table above |
| `uxmclaims.ability.warp.create` | everyone | Create warps where your role allows it |
| `uxmclaims.ability.warp.delete` | everyone | Delete warps |
| `uxmclaims.ability.warp.rename` | everyone | Rename warps |
| `uxmclaims.ability.warp.relocate` | everyone | Move warps |
| `uxmclaims.ability.warp.visibility` | everyone | Make warps public or private |
| `uxmclaims.admin.warps` | op | Manage and use every warp, without limits, costs, cooldowns or warmup |

| Role permission | Default holder | Allows |
|---|---|---|
| `warps:use` (3.x `USE_WARPS`) | Member | Teleport to the land's private warps |
| `warps:manage` (3.x `MANAGE_WARPS`) | Owner | Create, delete, rename, move and publish warps |

| Limit or cost | Default | Applies to |
|---|---|---|
| `uxmclaims.limit.warp.<n>` | 3, highest wins | Warps per land, read from the owner when online, else from the player acting |
| `uxmclaims.delay.warprename.<n>` | 30, lowest wins | Seconds between warp renames |
| `uxmclaims.delay.warprelocate.<n>` | 60, lowest wins | Seconds between warp moves |
| `uxmclaims.delay.teleport.<n>` | 3, lowest wins | Warmup seconds (registered by [Lands](lands.md)); the only warmup setting |
| Cost `warps:create` | 0 | Creating a warp; may step up with the land's warp count |
| Cost `warps:teleport` | 0 | Teleporting to a private warp |
| Cost `warps:teleport-public` | 0 | Teleporting to a public warp |

## Settings

| Key | Default | What it does |
|---|---|---|
| `name-uniqueness` | `GLOBAL` | `GLOBAL`: a name is used once on the server. `PER_LAND`: each land has its own names |
| `name-pattern` | `^[\p{L}\p{N}_\-]+$` | Characters a warp name may use |
| `name-length.min` | `3` | Shortest warp name |
| `name-length.max` | `32` | Longest warp name |
| `public-pvp-warning` | `true` | Ask before teleporting a non-member to a public warp in a land with PvP on |
| `default-public` | `false` | Whether a new warp is public |
| `teleport.cancel-on-move` | `true` | Cancel the teleport when the player moves to another block during the warmup |
| `teleport.safe` | `true` | Move an unsafe target (void, lava, a long fall) to safe ground in its column |
| `confirm-seconds` | `30` | How long a confirmation or chat prompt stays open |

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_warps_count%` | Warps of the player's current land |
| `%uxmclaims_warps_list%` | Names of those warps, comma separated |
| `%uxmclaims_warps_public%` | Public warps on the server |

## Notes

- A warp must stand inside the land's chunks. Unclaiming a chunk deletes the warps in it.
- A player tagged by [Combat](combat.md) cannot teleport to a warp.
- The default Member role has `warps:use`. A custom role made in game starts without it.
- A deleted land keeps its warps while it can be restored. With `GLOBAL` names, another land may take one of those names meanwhile.
- Bedrock players confirm warp deletes and the PvP warning in chat, not in a form.
- Under LuckPerms, the ability nodes follow `undefined-ability` in the [LuckPerms](luckperms.md) module.
- `teleport.warmup-seconds` is no longer read: the warmup comes from `uxmclaims.delay.teleport` alone. A file that still has the key gets an unknown-key warning; delete the line.

Related: [Lands](lands.md), [Economy](economy.md), [Combat](combat.md)
