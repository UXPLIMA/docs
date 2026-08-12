---
title: Player State
order: 9150
description: 'The everyday self-and-target verbs: heal, feed, fly, game mode, speed, personal time and weather.'
---

Player State bundles the verbs that act on a player rather than the world: healing and feeding, flight, game
mode, walk and fly speed, personal time and weather, experience, inventory inspection, and a stack of read-only
info commands. Most verbs take an optional player argument, gated by their own others node.

Module `playerstate` · enabled by default · `modules/playerstate/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/air` | Set a player's remaining air. | `uxmessentials.air.use` |
| `/biome` | Show the biome you are standing in. | `uxmessentials.biome.use` |
| `/burn` | Set a player on fire for some seconds. | `uxmessentials.burn.use` |
| `/clearinventory` (`/ci`, `/clear`) | Clear a player's inventory. | `uxmessentials.clearinventory.use` |
| `/clearinventoryconfirmtoggle` (`/citoggle`) | Toggle a confirmation before /clearinventory. | `uxmessentials.clearinventory.confirmtoggle` |
| `/compass` | Show the direction you are facing. | `uxmessentials.compass.use` |
| `/depth` | Show your height relative to sea level. | `uxmessentials.depth.use` |
| `/dimension` (`/dim`) | Show the dimension you are standing in. | `uxmessentials.dimension.use` |
| `/endersee` | View a player's ender chest. | `uxmessentials.endersee.use` |
| `/exp` (`/xp`) | Get or set a player's experience. | `uxmessentials.exp.use` |
| `/ext` (`/extinguish`) | Put out a burning player. | `uxmessentials.extinguish.use` |
| `/feed` (`/eat`) | Restore hunger. | `uxmessentials.feed.use` |
| `/fly` | Toggle flight. | `uxmessentials.fly.use` |
| `/flyspeed` | Set walk/fly speed. | `uxmessentials.speed.use` |
| `/foodlevel` | Set a player's food level. | `uxmessentials.foodlevel.use` |
| `/gamemode` (`/gm`) | Set game mode. | `uxmessentials.gamemode.use` |
| `/getpos` (`/coords`, `/whereami`) | Show a player's coordinates. | `uxmessentials.getpos.use` |
| `/glow` | Toggle a glowing outline. | `uxmessentials.glow.use` |
| `/god` (`/godmode`) | Toggle damage immunity. | `uxmessentials.god.use` |
| `/heal` | Restore health. | `uxmessentials.heal.use` |
| `/health` | Set a player's health. | `uxmessentials.health.use` |
| `/ice` | Freeze a player for some seconds. | `uxmessentials.ice.use` |
| `/invsee` | View a player's inventory. | `uxmessentials.invsee.use` |
| `/near` (`/nearby`) | List nearby players. | `uxmessentials.near.use` |
| `/nightvision` (`/nv`) | Toggle night vision. | `uxmessentials.nightvision.use` |
| `/ping` | Show a player's ping. | `uxmessentials.ping.use` |
| `/playtime` | Show or reset a player's playtime breakdown. | `uxmessentials.playtime.use` |
| `/ptime` | Set your personal time. | `uxmessentials.ptime.use` |
| `/pweather` | Set your personal weather. | `uxmessentials.pweather.use` |
| `/rest` | Reset a player's time-since-rest so phantoms stop. | `uxmessentials.rest.use` |
| `/seed` | Show the seed of the world you are standing in. | `uxmessentials.seed.use` |
| `/speed` | Set walk/fly speed. | `uxmessentials.speed.use` |
| `/suicide` | Kill yourself. | `uxmessentials.suicide.use` |
| `/walkspeed` | Set walk/fly speed. | `uxmessentials.speed.use` |
| `/world` | Show the world you are standing in. | `uxmessentials.world.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.air.others` | op | /air on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.air.use` | op | /air \<seconds> to set a player's remaining air. |
| `uxmessentials.biome.use` | everyone | /biome to show the biome you are standing in. |
| `uxmessentials.burn.others` | op | /burn on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.burn.use` | op | /burn \<seconds> to set a player on fire. |
| `uxmessentials.clearinventory.confirmtoggle` | everyone | /clearinventoryconfirmtoggle (/citoggle) to require a confirmation before /clearinventory clears your own inventory. |
| `uxmessentials.clearinventory.others` | op | /clearinventory on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.clearinventory.use` | op | /clearinventory (/ci /clear) [player] to empty a player's inventory. |
| `uxmessentials.compass.use` | everyone | /compass to show the direction you are facing. |
| `uxmessentials.depth.use` | everyone | /depth to show your height relative to sea level. |
| `uxmessentials.dimension.use` | everyone | /dimension to show the dimension you are standing in. |
| `uxmessentials.endersee.others` | op | /endersee to open another player ender chest. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.endersee.use` | op | /endersee [player] to view a player's ender chest. |
| `uxmessentials.exp.others` | op | /exp on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.exp.use` | op | /exp (/xp) get\|set\|give\|take\|reset to read or change experience. |
| `uxmessentials.extinguish.others` | op | /extinguish on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.extinguish.use` | op | /ext (/extinguish) [player] to put out a burning player. |
| `uxmessentials.feed.others` | op | /feed on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.feed.use` | op | /feed [player] to restore hunger. |
| `uxmessentials.fly.others` | op | /fly on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.fly.use` | op | /fly [player] to toggle flight. |
| `uxmessentials.foodlevel.others` | op | /foodlevel on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.foodlevel.use` | op | /foodlevel \<amount> [player] to set a player's food level. |
| `uxmessentials.gamemode.others` | op | /gamemode on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.gamemode.use` | op | /gamemode \<mode> [player] and the /gmc /gms /gma /gmsp aliases. |
| `uxmessentials.getpos.others` | op | /getpos on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.getpos.use` | op | /getpos (/coords /whereami) to show a player's coordinates. |
| `uxmessentials.glow.use` | op | /glow to toggle a glowing outline on yourself. |
| `uxmessentials.god.others` | op | /god on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.god.use` | op | /god [player] to toggle damage immunity. |
| `uxmessentials.heal.others` | op | /heal on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.heal.use` | op | /heal [player] to restore health. |
| `uxmessentials.health.others` | op | /health on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.health.use` | op | /health \<amount> [player] to set a player's health. |
| `uxmessentials.ice.others` | op | /ice on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.ice.use` | op | /ice [player] [seconds] to freeze a player (inverse of /burn). |
| `uxmessentials.invsee.modify` | op | Edit a player's inventory through the /invsee menu (without this it is view-only). |
| `uxmessentials.invsee.others` | op | /invsee to open another player inventory. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.invsee.use` | op | /invsee [player] to view a player's inventory. |
| `uxmessentials.module.playerstate` | op | Hot-reload / inspect the playerstate module (flight, god mode, speed, health and the rest). |
| `uxmessentials.near.use` | op | /near [radius] to list nearby players. |
| `uxmessentials.nightvision.use` | op | /nightvision (/nv) to toggle a night-vision effect on yourself. |
| `uxmessentials.ping.others` | op | /ping on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.ping.use` | everyone | /ping to show a player's round-trip latency. |
| `uxmessentials.playerstate.fly.allworlds` | op | Keep flight in worlds where flying is switched off. |
| `uxmessentials.playerstate.others` | op | Use any playerstate command with a [player] target other than yourself. |
| `uxmessentials.playtime.others` | op | /playtime on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.playtime.reset` | op | /playtime reset [player] to wipe a player's tracked playtime (resetting others also needs uxmessentials.playerstate.others). |
| `uxmessentials.playtime.use` | op | /playtime [player] to show a player's playtime breakdown (active/afk, today/week/month/all-time). |
| `uxmessentials.ptime.use` | op | /ptime \<value\|reset> to set a per-player client-side time. |
| `uxmessentials.pweather.use` | op | /pweather \<clear\|rain\|reset> to set a per-player client-side weather. |
| `uxmessentials.rest.others` | op | /rest on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.rest.use` | op | /rest to reset a player's time-since-rest so phantoms stop. |
| `uxmessentials.seed.use` | everyone | /seed to show the seed of the world you are standing in. |
| `uxmessentials.speed.others` | op | /speed on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.speed.use` | op | /speed, /walkspeed and /flyspeed to set walk/fly speed. |
| `uxmessentials.suicide.use` | everyone | /suicide to kill yourself. |
| `uxmessentials.world.command-bypass` | op | Run commands a world blocks through its per-world command list. |
| `uxmessentials.world.use` | everyone | /world to show the world you are standing in, and /worlds to reach the world manager. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `heal-remove-effects` | `false` | /heal also clears active potion effects |
| `rest-enabled` | `true` | /rest resets phantom-spawn pressure |
| `playtime.tracking` | `true` | run the sampler; false leaves /playtime empty |
| `playtime.sample-seconds` | `60` | how often a player's playtime is credited |
| `no-fly-worlds` | `[]` | e.g. ["pvp", "arena"] |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_playerstate_biome%` | The biome the player is standing in. |
| `%uxmessentials_playerstate_fly%` | Whether the player may fly (yes/no). |
| `%uxmessentials_playerstate_fly_speed%` | The player's fly speed. |
| `%uxmessentials_playerstate_flying%` | Whether the player is flying right now (yes/no). |
| `%uxmessentials_playerstate_food%` | The player's food level. |
| `%uxmessentials_playerstate_gamemode%` | The player's game mode. |
| `%uxmessentials_playerstate_god%` | Whether the player takes no damage (yes/no). |
| `%uxmessentials_playerstate_health%` | The player's current health. |
| `%uxmessentials_playerstate_level%` | The player's experience level. |
| `%uxmessentials_playerstate_max_health%` | The player's maximum health. |
| `%uxmessentials_playerstate_playtime%` | How long the player has played, in whole hours. |
| `%uxmessentials_playerstate_playtime_formatted%` | How long the player has played, in the compact 1d2h form. |
| `%uxmessentials_playerstate_speed%` | The speed that applies to how the player is moving: fly speed while flying, walk speed otherwise. |
| `%uxmessentials_playerstate_walk_speed%` | The player's walk speed. |
| `%uxmessentials_playerstate_world%` | The world the player is in. |
| `%uxmessentials_playerstate_x%` | The player's block x. |
| `%uxmessentials_playerstate_xp%` | How far the player is through the current experience level, from 0 to 1. |
| `%uxmessentials_playerstate_y%` | The player's block y. |
| `%uxmessentials_playerstate_z%` | The player's block z. |
{/* /generated */}

## Notes

- **`/ptime` and `/pweather` are client-side.** They change what one player sees and never touch the world for
  anyone else.
- **`/invsee` opens read-only by default.** Editing another player's inventory is a separate node, so watching
  and changing are different grants.
- **`/world` reports, `/worlds` manages.** This module owns the read-only one.
- **A command renamed in `commands.conf` keeps its permission node,** because nodes are keyed to the command id
  rather than to the name you give it.
- **Flight granted here and flight granted by staff mode are different things.** Leaving staff mode restores the
  player's real flight allowance rather than whatever the mode gave them.

Related: [Items & World](itemworld.md), [Teleport](teleport.md), [Presence](presence.md)
