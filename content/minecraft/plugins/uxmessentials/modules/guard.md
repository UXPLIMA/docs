---
title: Guard
order: 9155
description: Region protection with rules, roles and behaviours, built in, for Paper and Folia, one server or a network.
icon: shield
---

Guard protects areas of your worlds: who may build, open, fight, use items or walk in, what nature may do, and
what happens to players inside (flight, game mode, messages, healing, blocked commands). It needs no other
plugin and replaces WorldGuard, with an importer that brings WorldGuard regions over. The full manual is the
[Guard section](../guard/index.md); this page is the reference.

Module `guard` · enabled by default · `modules/guard/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/guard` (`/bolge`, and `/rg` with `commands.rg-alias`) | Protect regions: rules, roles, behaviours, history. Every subcommand is on [Commands](../guard/commands.md) | `uxmessentials.guard.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.guard.backups` | op | /guard backups [restore \<file>]: list the region backups, or put a backup's regions back. |
| `uxmessentials.guard.bypass` | op | /guard bypass: toggle ignoring every guard rule. |
| `uxmessentials.guard.bypass.gamemode` | off | Keep your own game mode in a region whose gamemode behaviour would change it. |
| `uxmessentials.guard.define` | op | /guard define, redefine, wand, pos1, pos2, point, clear, expand: draw and define regions. |
| `uxmessentials.guard.family` | op | /guard family \<region> \<name\|clear>: group regions so their borders open to each other. |
| `uxmessentials.guard.here` | everyone | /guard here: the regions you stand in and your role there. |
| `uxmessentials.guard.history` | op | /guard history \<region>: the change history of a region. |
| `uxmessentials.guard.import` | op | /guard import worldguard: import WorldGuard regions, with dry-run and undo. |
| `uxmessentials.guard.info` | op | /guard info \<region>: a region's shape, rules, roles and behaviours. |
| `uxmessentials.guard.inspect` | op | /guard inspect: click a block to read what a player with no role may do there, and see why each of your own refusals happened. |
| `uxmessentials.guard.list` | op | /guard list [world]: list the regions of a world. |
| `uxmessentials.guard.lock` | op | /guard lock and unlock; edit and WorldEdit inside a locked region. |
| `uxmessentials.guard.log` | op | /guard log [player]: recent denied actions. |
| `uxmessentials.guard.member` | everyone | /guard member \<region> \<player> \<member\|guest\|clear>: owners and managers edit their own region's roster. |
| `uxmessentials.guard.menu` | op | See the region list in /guard menu; without it a region owner or manager gets the roles window of the region they stand in. |
| `uxmessentials.guard.near` | op | /guard near [radius]: list regions around you. |
| `uxmessentials.guard.notify` | op | Receive staff alerts when players repeatedly hit guard denials. |
| `uxmessentials.guard.parent` | op | /guard parent \<region> \<parent\|clear>: inherit rules from another region. |
| `uxmessentials.guard.priority` | op | /guard priority \<region> \<value>: set a region's priority. |
| `uxmessentials.guard.reload` | op | /guard reload: reload the guard config and recompile every world. |
| `uxmessentials.guard.remove` | op | /guard remove and restore: delete a region or bring a removed one back. |
| `uxmessentials.guard.role` | op | /guard role \<region> \<player> \<role\|clear>: give any role on any region. |
| `uxmessentials.guard.rule` | op | /guard rule \<region> \<action> \<target> \<allow\|deny\|clear>: edit a region's rules. |
| `uxmessentials.guard.scope` | op | /guard scope \<region> \<server\|set:name>: choose which servers a region applies on. |
| `uxmessentials.guard.set` | op | /guard set \<region> \<behaviour> \<value>: set a region behaviour. |
| `uxmessentials.guard.show` | op | /guard show and hide: draw region borders for yourself. |
| `uxmessentials.guard.status` | op | /guard status: engine counters and memory use. |
| `uxmessentials.guard.tag` | op | /guard tag \<region> add\|remove\|bulk: tag regions and edit them by tag. |
| `uxmessentials.guard.template` | op | /guard template \<region> \<template\|clear>: apply a rule template. |
| `uxmessentials.guard.test` | op | /guard test \<player> \<action> [region]: check a verdict for another player. |
| `uxmessentials.guard.undo` | op | /guard undo \<history id>: undo one change from a region's history. |
| `uxmessentials.guard.use` | everyone | /guard and /guard help, actions: the base of every guard command. |
| `uxmessentials.guard.why` | everyone | /guard why \<action>: explain the verdict for an action where you stand. |
| `uxmessentials.module.guard` | op | Hot-reload / inspect the guard module (region protection). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | Switches the module on or off. |
| `network.shared-sets` | `[]` | The shared region sets this server loads, besides its own regions. See [Networks](../guard/networks.md). |
| `network.new-region-scope` | `"server"` | Where new regions go: `server` (this server only) or `set:<name>`. |
| `network.poll-seconds` | `10` | How often this server checks the database for regions other servers changed, for when the network bus does not deliver. |
| `deny-message.channel` | `"actionbar"` | Where a refusal is shown: `actionbar` or `chat`. |
| `deny-message.cooldown-ms` | `1500` | At most one refusal message per player in this time. |
| `bypass.reminder-seconds` | `30` | How often a player with bypass on is reminded of it. |
| `display.mode` | `"lines"` | How `/guard show` draws borders: `lines` (glowing blocks) or `particles`. |
| `display.seconds` | `20` | How long borders stay. |
| `display.range` | `64` | How far around the player `/guard show` without a name looks for regions. |
| `movement.check-ticks` | `10` | How often each player's position, healing, feeding, interval actions and bypass reminder are checked, in ticks. It also catches moves that fire no event. |
| `alerts.enabled` | `true` | Tells staff with `uxmessentials.guard.notify` when one player is refused many times. |
| `alerts.threshold` | `8` | How many refusals make an alert. |
| `alerts.window-seconds` | `20` | Within how many seconds. |
| `alerts.cooldown-seconds` | `60` | At most one alert per player in this time. |
| `alerts.discord` | `true` | Also posts alerts through the Discord bridge when it is on. |
| `log.enabled` | `false` | Keeps every refused action in the database for `/guard log`. |
| `log.keep-days` | `14` | How long logged refusals are kept. |
| `backups.keep` | `10` | How many region backups are kept in `guard/backups`. |
| `roles.extra` | `[]` | Region roles beyond owner, manager, member and guest, for `role:<name>` rules. |
| `commands.rg-alias` | `false` | Also answer `/rg`. Off so WorldGuard keeps `/rg` while both are installed. |
| `claims.guard-distance` | `16` | uxmSimpleClaims refuses a claim inside a guard region or closer to one than this many blocks. |
| `templates` | six templates | Named rule and behaviour sets: `spawn`, `lobby`, `arena`, `market`, `parkour`, `build`. See [Templates](../guard/templates.md). |
| `worlds.default.high-frequency` | `true` | `false` skips liquid flow, fire spread, growth, decay, forming and melting for a world. |
| `worlds.default.rules` | `[]` | Rules every world without its own entry starts from. |
| `worlds.default.behaviours` | `[]` | Behaviours every world without its own entry starts from. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_regions_available%` | Whether the guard module is on (yes/no). Every regions_ placeholder is also readable as guard_. |
| `%uxmessentials_regions_inside%` | Whether the player is standing in a protected region (yes/no). |
| `%uxmessentials_regions_here%` | The region the player is standing in, highest priority first when they overlap. |
| `%uxmessentials_regions_here_priority%` | That region's priority, which is what decides an overlap. |
| `%uxmessentials_regions_here_owners%` | Who owns the region the player is standing in, comma separated. |
| `%uxmessentials_regions_here_members%` | Who may build in the region the player is standing in, comma separated. |
| `%uxmessentials_regions_here_role%` | The player's role in the region they stand in (owner, manager, member, guest), or empty. |
| `%uxmessentials_regions_allowed_<action>%` | Whether the player may do a guard action where they stand, such as regions_allowed_break. |
| `%uxmessentials_regions_count%` | How many regions cover the player at once. |
| `%uxmessentials_regions_world_count%` | How many regions are defined in the world the player is in. |
{/* /generated */}

Every placeholder above also answers under `guard_` (`%uxmessentials_guard_here%`). `regions_allowed_<action>`
takes any action from [Actions](../guard/actions.md), such as `regions_allowed_use.door`.

## Notes

- **A new region does nothing** until it has a rule or a template, so a region you are still drawing never locks
  anyone out.
- **Players can ask why.** `/guard why <action>` is open to everyone and names the region and rule that decided.
- **Owners run their own roster.** A region owner or manager hands out `member` and `guest` with
  `/guard member` or `/guard menu`, without any staff node.
- **Every change has a history** and can be undone one by one, and a backup is written at start whenever the
  regions changed since the last one, ready for `/guard backups restore`.
- **Teleports are checked.** `/home`, `/warp`, `/tpa`, `/back` and other plugins' teleports obey `move.enter` and
  `move.teleport-in`; `/fly` and `/god` obey the region's `fly` and `god`.
- **uxmSimpleClaims keeps its distance.** Claims are refused inside a guard region or within
  `claims.guard-distance` of one.
- **Replaces the old Regions module,** which was an editor for WorldGuard. `/regions` and its nodes are gone.

Related: [Guard manual](../guard/index.md), [Worlds](worlds.md), [Player Warps](playerwarps.md)
