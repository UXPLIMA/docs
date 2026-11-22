---
title: PlaceholderAPI
order: 1340
description: The %uxmclaims_*% placeholders about lands, players and the server, and the 3.x placeholder paths.
icon: braces
---

The placeholders module answers the land, player and server placeholders of the `uxmclaims` PlaceholderAPI
expansion, and keeps the 3.x paths such as `%uxmclaims_claim_name%` working. Other modules add their own
placeholders under the same identifier (`%uxmclaims_town_name%`, `%uxmclaims_warps_count%`); each is listed on
that module's page.

Module `placeholders` · enabled by default · `modules/placeholders/config.yml` · needs PlaceholderAPI

## Settings

| Key | Default | What it does |
|---|---|---|
| `legacy-paths` | `true` | Keeps the 3.x paths working (`%uxmclaims_claim_name%`, `%uxmclaims_count:claim_members%`). Old paths that now belong to another module are forwarded through `aliases.yml`. Needs a restart. |
| `fallback-to-owned-land` | `true` | For `land_` and `claim_` placeholders: when the player picked no land with `/claim select` and stands in no land, use the only land they own (the 3.x order). `false` stops after the land underfoot. |

The texts the placeholders print live in `lang/<code>/messages.yml` of the module:

| Key | Default | What it does |
|---|---|---|
| `formats.time` | `dd days HH hours mm minutes ss seconds` | The duration pattern for the `time:` modifier. `dd`, `HH`, `mm` and `ss` are replaced. |
| `formats.date` | `dd-MM-yyyy HH:mm:ss` | The Java date pattern for dates and the `date:` modifier. |
| `formats.status-active` | `<green>Enabled</green>` | What the `status:` modifier prints for true. |
| `formats.status-passive` | `<red>Disabled</red>` | What the `status:` modifier prints for false. |
| `formats.unlimited` | `Unlimited` | What a limit placeholder prints for an unlimited or negative value. |
| `owner.server` | `Server` | The owner name of admin lands. |
| `defaults.<placeholder>` | see file | What a placeholder prints when there is no land. The key is the whole placeholder without percent signs, for example `defaults.uxmclaims_land_name: 'Wilderness'`. A placeholder without an entry prints nothing. |

`aliases.yml` maps an old 3.x path (the text after `%uxmclaims_`) to the path that answers it now. Case and `.`
versus `_` do not matter on the left side. The shipped table forwards warps, bans, invites, vault, expiry and
claim block paths; an entry for a path this module answers itself redirects it.

## Which land a placeholder reads

A `land_` placeholder reads the land picked with `/claim select`, else the land the player stands in, else
(with `fallback-to-owned-land`) the only land the player owns. A player who owns two or more lands and stands
in the wilderness gets the default text. A `here_land_` placeholder reads only the land underfoot, and an
offline player has none.

## Placeholders

The land in context:

| Placeholder | Renders |
|---|---|
| `%uxmclaims_land_name%` | The land's name. |
| `%uxmclaims_land_id%` | The land's id (a UUID). |
| `%uxmclaims_land_owner%` | The owner's name: a player, town or nation name, or `owner.server` for admin lands. Also `land_owner_name`. |
| `%uxmclaims_land_owner_uuid%` | The owner's id. Also `land_owner_id`. |
| `%uxmclaims_land_owner_type%` | `player`, `town`, `nation` or `server`. |
| `%uxmclaims_land_kind%` | `personal`, `town_home`, `outpost` or `admin`. |
| `%uxmclaims_land_world%` | The world name. |
| `%uxmclaims_land_chunks%` | Number of chunks. Also `land_chunks_count`. |
| `%uxmclaims_land_members%` | Number of members. Also `land_members_count`. |
| `%uxmclaims_land_roles%` | Number of roles. Also `land_roles_count`. |
| `%uxmclaims_land_flags_enabled%` | Number of flags that are on, counting unset flags whose default is on. |
| `%uxmclaims_land_created%` | The creation date, in `formats.date`. |
| `%uxmclaims_land_created_millis%` | The creation time in milliseconds since 1970. |
| `%uxmclaims_land_main_chunk_x%`, `%uxmclaims_land_main_chunk_z%` | The main chunk's coordinates. |
| `%uxmclaims_land_has_spawn%` | `true` or `false`. |
| `%uxmclaims_land_spawn_world%`, `_x`, `_y`, `_z` | The land spawn, empty when it has none. |
| `%uxmclaims_land_role%` | The player's role name there (the visitor role for outsiders). |
| `%uxmclaims_land_is_owner%` | `true` or `false`. |
| `%uxmclaims_land_is_member%` | `true` or `false`. |
| `%uxmclaims_here_land_<field>%` | Any `land_` field above for the land underfoot only, for example `%uxmclaims_here_land_name%`. |

The player:

| Placeholder | Renders |
|---|---|
| `%uxmclaims_player_lands%` | Lands the player is a member of, owned ones included. |
| `%uxmclaims_player_lands_owned%` | Lands the player owns. |
| `%uxmclaims_player_lands_names%` | Names of the lands the player owns, comma separated, sorted. |
| `%uxmclaims_player_chunks%` | Chunks over all lands the player owns. |
| `%uxmclaims_player_limit_<name>%` | A limit value, online players only. `limit_claim` reads `uxmclaims.limit.claim`, `limit_delay_teleport` the delay `uxmclaims.delay.teleport`; an underscore stands for a dot. Unlimited prints `formats.unlimited`; a limit no module registered prints nothing. |

The spot the player stands on (`true` or `false`, empty for an offline player):

| Placeholder | Renders |
|---|---|
| `%uxmclaims_is_claimable%` | No land here (3.x name). |
| `%uxmclaims_is_in_land%` | A land is here. |
| `%uxmclaims_is_in_own_land%` | The player owns the land here. |
| `%uxmclaims_is_member_here%` | The player is a member of the land here. |

The server:

| Placeholder | Renders |
|---|---|
| `%uxmclaims_server_lands%` | All lands. |
| `%uxmclaims_server_chunks%` | All claimed chunks. |

3.x paths, with `legacy-paths: true`:

| Placeholder | Renders |
|---|---|
| `%uxmclaims_claim_name%`, `%uxmclaims_claim_id%` | Name and id of the land in context. |
| `%uxmclaims_claim_ownerUid%`, `%uxmclaims_claim_owner_name%` | The owner's id and name. |
| `%uxmclaims_claim_spawnLocation_world%`, `_x`, `_y`, `_z` | The land spawn. |
| `%uxmclaims_claim_mainChunk_x%`, `%uxmclaims_claim_mainChunk_z%` | The main chunk. |
| `%uxmclaims_claim_creationDate%` | The creation time. |
| `%uxmclaims_count:claim_chunks%`, `count:claim_members`, `count:claim_roles`, `count:claim_flags` | Counts. |
| `%uxmclaims_date:claim_creationDate%` | The creation date in `formats.date`. |
| `%uxmclaims_<modifier>:claim_<path>%` | Any path above through a 3.x modifier: `upper`, `lower`, `float`, `count`, `status`, `time`, `date`. |
| `%uxmclaims_count:claim_warps%`, `count:claim_bans`, `count:claim_invites`, `count:claim_vault_items`, `time:claim_remainTime`, `date:claim_expireDate`, `status:claim_isExpired`, `claim_block_keyId`, `status:claim_block_placed` | Forwarded through `aliases.yml` to the warps, bans, invites, vault, expiry and claim block modules. They print the default text while that module is off. |

## Notes

- The `uxmclaims` expansion belongs to uxmClaims itself and is registered whenever PlaceholderAPI is installed.
  Turning this module off removes only the placeholders above; other modules keep theirs.
- `player_limit_` placeholders need the player online, because limits come from permissions.
- `aliases.yml` and the language texts apply after `/uxm reload`. Switching `legacy-paths` needs a restart.
- PlaceholderAPI has not been tried on 26.2 servers yet; it works on Spigot 1.21.1.

Related: [Placeholders overview](../placeholders.md), [Limits](limits.md), [Lands](lands.md), [Integrations](../integrations.md)
