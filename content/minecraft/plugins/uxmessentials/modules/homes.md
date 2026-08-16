---
title: Homes
order: 9070
description: Private teleport points each player saves in a slot grid.
---

A home is a private point a player saves for themselves. `/home` opens a grid of numbered slots: a filled slot
teleports, an empty one saves the player's current position, and renaming, re-iconing and deleting happen in the
slot's own view. Homes live in the database, so they survive a world rollback, a `/reload` and a restart.

Module `homes` · enabled by default · `modules/homes/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/home` (`/h`, `/homes`) | Open and manage your homes. | `uxmessentials.home.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.home.admin` | op | /homeadmin to manage another player's homes. |
| `uxmessentials.home.bypass.cost` | op | Skip the per-action economy cost for home create, relocate, and teleport actions. |
| `uxmessentials.home.bypass.unsafe` | op | Skip the unsafe-destination confirm when teleporting to a home via the GUI. |
| `uxmessentials.home.icon` | everyone | Pick a custom GUI icon for one of your homes from the grid. |
| `uxmessentials.home.invite` | everyone | /invite and /uninvite to grant or revoke another player's access to one of your homes. |
| `uxmessentials.home.limit.<n>` | everyone | How many homes you may keep; the largest tier held wins. |
| `uxmessentials.home.use` | everyone | /home to open and manage your slot-based home grid. |
| `uxmessentials.home.visit` | everyone | /visit to teleport to another player's public home or one you were invited to. |
| `uxmessentials.module.homes` | op | Hot-reload / inspect the homes module (per-player homes and the slot grid). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `default-limit` | `3` | homes per player without a uxmessentials.home.limit.\<n> node |
| `limit-mode` | `"highest"` | how multiple uxmessentials.home.limit.\<n> tiers combine: "highest" (max tier wins) or "stack" (sum tiers) |
| `unlimited-max` | `1000` | slot ceiling for an unlimited (-1) quota, so the grid stays bounded |
| `date-format` | `"dd/MM/yyyy HH:mm"` | how a home's creation time renders in its GUI lore |
| `disabled-worlds` | `[]` | worlds where players cannot create/relocate homes |
| `block-unsafe-sethome` | `true` | reject placing a home in an unsafe spot (air/lava/suffocating) |
| `consider-midair-unsafe` | `true` | treat a spot with no solid ground just below as unsafe (anti-elytra) |
| `midair-ground-depth` | `5` | how many blocks below to look for solid ground when the above is on |
| `confirm-delete` | `true` | show a yes/no confirm before deleting a home from the GUI |
| `confirm-relocate` | `false` | show a yes/no confirm before relocating a home to the player's position |
| `confirm-unsafe-teleport` | `true` | show a warning confirm when teleporting to a home at an unsafe location |
| `economy.enabled` | `false` | charge players for home actions (requires an economy provider) |
| `economy.create-cost` | `0` | charged when creating a new home slot |
| `economy.relocate-cost` | `0` | charged when relocating an existing home to a new position |
| `economy.teleport-cost` | `0` | charged each time a player teleports to a home |
| `economy.currency` | `"default"` | currency id to charge in; "default" uses the server's primary currency |
| `claims.enabled` | `true` | honour the active claim plugin; off ignores claims entirely |
| `claims.require-claim` | `false` | a home may only be placed inside a claim the player trusts |
| `claims.block-foreign-claims` | `true` | a home may not be placed inside another player's claim |
| `claims.foreign-claim-chunk-distance` | `0` | chunk radius to reject placement near a foreign claim; 0 disables |
| `claims.check-teleport-access` | `true` | deny teleporting to a home the player is no longer trusted in |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_homes_<n>%` | The name of the player's nth home, counting from 1. |
| `%uxmessentials_homes_<n>_world%` | The world the player's nth home sits in. |
| `%uxmessentials_homes_<n>_x%` | The block x of the player's nth home. |
| `%uxmessentials_homes_<n>_y%` | The block y of the player's nth home. |
| `%uxmessentials_homes_<n>_z%` | The block z of the player's nth home. |
| `%uxmessentials_homes_count%` | How many homes the player has set. |
| `%uxmessentials_homes_exists_<home>%` | Whether the player has a home by that name (yes/no). |
| `%uxmessentials_homes_left%` | How many more homes the player may set. |
| `%uxmessentials_homes_limit%` | How many homes the player may keep; the infinity marker when the quota is unlimited. |
| `%uxmessentials_homes_list%` | The player's home names, comma separated. |
{/* /generated */}

## Notes

- **Slots start at 1.** `/home visit Steve 2` is Steve's second slot. Leaving the slot out means the player's
  first home.
- **The quota decides how many slots a player sees.** `uxmessentials.home.limit.<n>` grants `n` homes, and
  `limit-mode` decides what happens when a player holds several: `highest` takes the largest, `stack` adds them
  up. Granting `limit.3` and `limit.10` gives 10 homes under `highest` and 13 under `stack`.
- **A quota can be per world:** `uxmessentials.home.limit.5.world_nether` overrides the global limit in that
  world alone. The suffix is the Bukkit world name.
- **Homes can cost money.** The `economy` block is off by default; switched on, creating, relocating and
  teleporting each charge what you set, through the [economy](economy.md) module.
- **Claims are honoured when a claim plugin is installed.** You can require a home to sit inside a claim, refuse
  homes inside somebody else's claim, and deny teleporting to a home the player is no longer trusted in.
- **Unsafe spots are refused** rather than dropping a player into lava, and a home that has become unsafe since
  it was saved asks for confirmation. `uxmessentials.home.bypass.unsafe` skips both.

Related: [Warps](warps.md), [Teleport](teleport.md), [Economy](economy.md)
