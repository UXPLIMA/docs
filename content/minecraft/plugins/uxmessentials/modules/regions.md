---
title: Regions
order: 9200
description: 'An in-game manager for WorldGuard regions: browse, create, edit flags, and manage rosters.'
icon: land-plot
---

Regions is a front end over an installed WorldGuard: browse a world's regions, carve out a new cuboid, cycle a
region's flags, set its priority, and edit its member and owner lists, all from a menu instead of `/rg` syntax
and raw UUIDs. It does not bundle WorldGuard and does not protect anything by itself.

Module `regions` · disabled by default · `modules/regions/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/regions` | Manage WorldGuard regions: list, create, edit flags, members and priority. | `uxmessentials.regions.list` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.regions` | op | Hot-reload / inspect the regions module (WorldGuard region management). |
| `uxmessentials.regions.admin` | op | /regions priority \<id> \<value>: set a WorldGuard region priority. |
| `uxmessentials.regions.create` | op | /regions create \<id> (and /regions pos1\|pos2): define a cuboid WorldGuard region. |
| `uxmessentials.regions.flags` | op | /regions flags \<id>: open the per-region flag editor GUI. |
| `uxmessentials.regions.list` | op | /regions [world]: open the WorldGuard region-list GUI for a world. |
| `uxmessentials.regions.members` | op | /regions members \<id> and /regions addmember\|addowner \<id> \<player>: manage a region roster. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `list.page-size` | `45` | How many regions fill one page of the /regions list before it paginates. 1..45 (one six-row chest page, with the bottom row reserved for the previous/next buttons). |
| `flags.editable` | `[]` | An OPTIONAL allow-list of the WorldGuard flags the editor shows, in display order. Leave it EMPTY (the default) to list every registered flag, of every type: state flags (pvp, build, ...) cycle allow/deny/unset, boolean flags toggle true/false/unset, text flags (greeting, farewell, ...) open a text prompt, number flags open a validated number prompt, and fixed-choice flags (game-mode, weather-lock, a region-group) open a choice picker. A complex flag the editor cannot map (a set, a location) is shown read-only. Set a non-empty list to restrict the editor to exactly those flags; names are the flag ids WorldGuard registers (lower kebab-case), e.g.: editable = [ "pvp", "build", "greeting", "game-mode" ] |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_regions_available%` | Whether a region provider is reachable at all (yes/no). |
| `%uxmessentials_regions_count%` | How many regions cover the player at once. |
| `%uxmessentials_regions_here%` | The region the player is standing in, highest priority first when they overlap. |
| `%uxmessentials_regions_here_members%` | Who may build in the region the player is standing in, comma separated. |
| `%uxmessentials_regions_here_owners%` | Who owns the region the player is standing in, comma separated. |
| `%uxmessentials_regions_here_priority%` | That region's priority, which is what decides an overlap. |
| `%uxmessentials_regions_inside%` | Whether the player is standing in a protected region (yes/no). |
| `%uxmessentials_regions_world_count%` | How many regions are defined in the world the player is in. |
{/* /generated */}

## Notes

- **Without WorldGuard the module is inert.** It loads, and every command answers with one line saying
  WorldGuard is not installed.
- **Creating a region is two steps: pick the volume, then name it.** With WorldEdit installed `/regions create`
  reads your selection; without it, mark the corners with `/regions pos1` and `/regions pos2`.
- **The flag editor lists every flag WorldGuard has registered** and edits each in the way that suits its type:
  a state flag cycles unset, allow and deny, a numeric one prompts for a value.
- **Priority is how overlapping regions are resolved.** A small no-pvp arena inside a pvp world needs a higher
  number than its parent.
- **Members and owners are edited by name,** so adding a co-owner does not mean pasting a UUID.

Related: [Worlds](worlds.md), [Poses](poses.md), [Homes](homes.md)
