---
title: Worlds
order: 9340
description: Create, import, load and delete worlds, tune their rules, and gate them behind a permission or a fee.
icon: earth
---

`/worlds` is a full multi-world manager: create a world, import one that is already on disk, load and unload it,
tune its properties and gamerules, set its spawn, and move players into it, without editing `bukkit.yml` or
restarting. The bare command opens the world list, and picking a world opens its editor.

Module `worlds` · enabled by default · `modules/worlds/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/worlds` | Manage worlds: create, import, load, unload, unregister, delete, list, info, spawn, tp, gui, pregen, backup, backups, restore, restoreconfirm. | `uxmessentials.world.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.worlds` | op | Hot-reload / inspect the worlds module (world creation, properties and access). |
| `uxmessentials.world.<world>` | op | Enter one world that is configured as restricted. |
| `uxmessentials.world.access.bypass` | op | Enter a restricted world without holding that world's own entry node. |
| `uxmessentials.world.backup` | op | /world backup \<name>: snapshot a world's folder. |
| `uxmessentials.world.create` | op | /world create \<name>: generate and register a new world. |
| `uxmessentials.world.delete` | op | /world delete \<name>: unregister a world and delete its folder from disk. |
| `uxmessentials.world.gamemode.bypass` | op | Keep your own game mode in a world that forces one on entry. |
| `uxmessentials.world.gamerule` | op | /world gamerule \<name> \<rule> \<value>: change one gamerule on a world. |
| `uxmessentials.world.gui` | op | /world: open the world management GUI. |
| `uxmessentials.world.import` | op | /world import \<folder>: adopt an existing world folder into the registry. |
| `uxmessentials.world.info` | op | /world info \<name>: read one world's generator, properties, gamerules and spawn. |
| `uxmessentials.world.list` | op | /world list: list every registered world with its load state and player count. |
| `uxmessentials.world.load` | op | /world load \<name>: load a registered world that is currently unloaded. |
| `uxmessentials.world.pregen` | op | /world pregen \<name> \<radius>: pre-generate a world's chunks in the background. |
| `uxmessentials.world.restore` | op | /world restore \<name> \<backup>: restore a world from one of its snapshots. |
| `uxmessentials.world.set` | op | /world set \<name> \<property> \<value>: change a world property such as difficulty or PvP. |
| `uxmessentials.world.setspawn` | op | /world setspawn [name]: set a world's spawn point to where you are standing. |
| `uxmessentials.world.spawn` | everyone | /worlds spawn to teleport to a world's spawn (subject to per-world access rules). |
| `uxmessentials.world.tp` | op | /world tp \<name>: teleport yourself to a world's spawn. |
| `uxmessentials.world.tp.others` | op | /world tp \<name> \<player>: teleport somebody else to a world's spawn. |
| `uxmessentials.world.unload` | op | /world unload \<name>: unload a loaded world, moving anybody inside to spawn. |
| `uxmessentials.world.unregister` | op | /world unregister \<name>: drop a world from the registry, leaving its folder on disk. |
| `uxmessentials.world.voidrescue.exempt` | off | Keep falling in a world that catches players out of the void. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `auto-adopt-loaded` | `true` | On enable, take every already-loaded server world (the vanilla world/world_nether/world_the_end) under management so it appears in /worlds list and is fully manageable. |
| `auto-load-registered` | `true` | On enable, load every registered world flagged auto_load that is not yet loaded. |
| `protect-default-world` | `true` | Refuse to unload or delete the server's primary world. |
| `delete-confirm-timeout` | `"30s"` | How long a staged /worlds delete confirmation stays valid before it must be re-requested. |
| `defaults.environment` | `"NORMAL"` |  |
| `defaults.world-type` | `"NORMAL"` |  |
| `defaults.generate-structures` | `true` |  |
| `settings.defaults.pvp` | `true` |  |
| `settings.defaults.difficulty` | `"NORMAL"` |  |
| `settings.defaults.force-gamemode` | `"NONE"` |  |
| `settings.defaults.spawn-animals` | `true` |  |
| `settings.defaults.spawn-monsters` | `true` |  |
| `generators.void.biome` | `"plains"` |  |
| `generators.flat.biome` | `"plains"` |  |
| `generators.flat.layers` | `["minecraft:bedrock 1", "minecraft:dirt 3", "minecraft:grass_block 1"]` | Bottom -> top, each "\<blockId> \<height>". Default = Classic Flat (bedrock, 3x dirt, grass). |
| `access.redirect-on-restricted-join` | `true` |  |
| `pregen.max-radius` | `200` |  |
| `pregen.max-concurrent-chunks` | `10` |  |
| `pregen.tick-period-ticks` | `1` |  |
| `backup.directory` | `"backups/worlds"` |  |
| `backup.retention-count` | `10` |  |
| `auto-unload.enabled` | `false` |  |
| `auto-unload.idle-minutes` | `30` |  |
| `auto-unload.sweep-interval-seconds` | `60` |  |
| `auto-unload.excluded-worlds` | `[]` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_worlds_default%` | The name of the default world. |
| `%uxmessentials_worlds_default_players%` | How many players are in the default world. |
| `%uxmessentials_worlds_loaded_count%` | How many worlds are loaded right now. |
| `%uxmessentials_worlds_managed_count%` | How many worlds the plugin's registry holds. |
{/* /generated */}

## Notes

- **`/worlds` manages, `/world` reports.** The plural command is this module; the singular one is a player-state
  info command that names the world you are standing in.
- **`import` registers a folder in place**, regenerating nothing, which is how a downloaded map or a world from
  another plugin joins the roster. `/uxmess import multiverse` does the whole Multiverse-Core registry at once.
- **Two generators ship beyond the vanilla types:** `void` for an empty world and `flat` for a superflat one.
- **The default world can use them too.** `/world create` is the usual way in, but the world the server starts
  with is made before any command can run, so it is configured in `bukkit.yml` instead:

  ```yaml
  worlds:
    world:
      generator: uxmEssentials:void
  ```

  This works from 0.8.3 on. Earlier versions logged `Plugin 'uxmEssentials' is not enabled yet (is it
  load:STARTUP?)` and generated normal terrain, because the server builds the default world before the plugin
  had enabled. If the worlds module is off, or the id after the colon is not `void` or `flat`, the world still
  generates as normal terrain and the reason is now written to the log.

  One consequence is worth planning for: a world pointed at our generator falls back to whatever
  `server.properties` describes if the plugin ever fails to enable. Keep `level-type` and the rest of that file
  set to something you would accept seeing, because that is the world your players land in on a bad boot.
- **Access is not automatic.** The `access` block gates a world behind a permission and can charge an entry fee
  through the economy, which is how a paid event map or a premium world is built with no extra plugin.
- **A world can catch players who fall out of it.** Set `void-rescue` on a world and anybody who drops into the
  void there is moved instead of dying. The value is an ordered list of steps separated by `;`, and the first one
  that resolves wins:

  | Step | Sends the player to |
  |---|---|
  | `spawn` | the spawn `/spawn` would use for that world |
  | `warp:<name>` | a server warp |
  | `at:<world>,<x>,<y>,<z>` | exact coordinates, optionally `,<yaw>,<pitch>` |

  ```
  /worlds set lobby void-rescue spawn
  /worlds set event void-rescue warp:arena;spawn
  /worlds set parkour void-rescue at:parkour,0.5,80,0.5,180,0
  ```

  By default the catch happens when the void starts hurting the player. Set `void-rescue-y` as well to catch them
  earlier, at a height of your choosing (`/worlds set lobby void-rescue-y -10`), so they never see the bottom of
  the fall. Clear either setting by passing an empty value. Spectators are never moved, nor is anybody holding
  `uxmessentials.world.voidrescue.exempt`. If the destination is itself in the void the rescue would loop, so
  after three rescues in ten seconds the player is left to fall and a warning is written to the console.
- **Portals wire worlds together** so players walk between them instead of typing a command; a portal honours
  the same access gate and fee. It also honours the server's own policy: with `allow-nether` or `allow-end` off
  in `server.properties`, that portal is refused before any configured link is looked at.
- **Deleting and restoring are two-step.** Both ask for a confirm command, so a live world cannot be lost to a
  mistyped name. Snapshots land in `backup.directory` and are pruned to `backup.retention-count`.
- **Pre-generation is paced, not blocking.** `pregen <name> <radius>` walks the world outward under
  `max-concurrent-chunks` and `tick-period-ticks`, and `pregen cancel` stops it.
- **Auto-unload is off by default.** Switched on, a world with no players is unloaded after `idle-minutes`,
  skipping anything in `excluded-worlds`.

Related: [Teleport](teleport.md), [Regions](regions.md), [Menus](../menus/engine.md)
