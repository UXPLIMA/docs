---
title: Teleport
order: 9270
description: Requests, back, random teleport, spawns and staff teleports under one warmup and cooldown system.
icon: move-3d
---

The teleport module owns everything that moves a player: the `/tpa` request handshake, `/back`, the random
wilderness teleport behind `/rtp`, the server spawns, and the direct staff teleports. All of them share one
warmup, cooldown and safety system, so the rules feel the same whichever command a player types, and every
search and lookup runs off the main thread.

Module `teleport` · enabled by default · `modules/teleport/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/ascend` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/back` (`/return`) | Return to your last location. | `uxmessentials.back.use` |
| `/bottom` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/bring` | /tp, /tphere, /goto, /bring and /tprandomplayer (/tprp) direct staff teleport. | `uxmessentials.tp.use` |
| `/deathback` (`/dback`) | Return to your last death location. | `uxmessentials.back.use` |
| `/descend` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/down` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/goto` | /tp, /tphere, /goto, /bring and /tprandomplayer (/tprp) direct staff teleport. | `uxmessentials.tp.use` |
| `/jump` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/mirrorspawn` | Redirect this world's spawn to another world. | `uxmessentials.spawn.set` |
| `/removespawn` | Clear this world's own spawn. | `uxmessentials.spawn.set` |
| `/rtp` (`/wild`) | Randomly teleport within the world. | `uxmessentials.rtp.use` |
| `/setmainspawn` | Set the global main spawn. | `uxmessentials.spawn.set` |
| `/setspawn` | Set the server spawn. | `uxmessentials.spawn.set` |
| `/settpr` | Set the random-teleport zone for the running /rtp. | `uxmessentials.teleport.settpr` |
| `/spawn` | Teleport to spawn. | `uxmessentials.spawn.use` |
| `/thru` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/top` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
| `/tp` | /tp, /tphere, /goto, /bring and /tprandomplayer (/tprp) direct staff teleport. | `uxmessentials.tp.use` |
| `/tpa` | Request and resolve a teleport. | `uxmessentials.tpa.use` |
| `/tpaall` | Request every online player to teleport to you. | `uxmessentials.tpa.all` |
| `/tpaccept` (`/tpyes`) | Request and resolve a teleport. | `uxmessentials.tpa.use` |
| `/tpahere` | Ask a player to come to you. | `uxmessentials.tpahere.use` |
| `/tpalist` (`/tprequests`) | List players waiting on your teleport reply | `uxmessentials.tpa.use` |
| `/tpall` | Pull every online player to you. | `uxmessentials.tp.all` |
| `/tpauto` | Auto-accept incoming teleport requests. | `uxmessentials.tpa.auto` |
| `/tpblock` | Block a player's requests. | `uxmessentials.tpa.block` |
| `/tpcancel` (`/tpacancel`) | Withdraw your outgoing request. | `uxmessentials.tpa.cancel` |
| `/tpdeny` (`/tpno`) | Request and resolve a teleport. | `uxmessentials.tpa.use` |
| `/tphere` | /tp, /tphere, /goto, /bring and /tprandomplayer (/tprp) direct staff teleport. | `uxmessentials.tp.use` |
| `/tpo` | Teleport overriding no-tp flags. | `uxmessentials.tp.others` |
| `/tpoff` | Refuse all incoming teleport requests. | `uxmessentials.tpa.toggle` |
| `/tpoffline` | A player's logout location. | `uxmessentials.tp.offline` |
| `/tpofflinehere` | A player's logout location. | `uxmessentials.tp.offline` |
| `/tpohere` | Teleport overriding no-tp flags. | `uxmessentials.tp.others` |
| `/tpon` | Refuse all incoming teleport requests. | `uxmessentials.tpa.toggle` |
| `/tppos` | Teleport to raw coordinates. | `uxmessentials.tp.position` |
| `/tprandomplayer` (`/tprp`) | Teleport to a random online player. | `uxmessentials.tp.use` |
| `/tpsettings` | Open your personal teleport settings panel. | `uxmessentials.tpsettings.use` |
| `/tptoggle` (`/toggletp`) | Refuse all incoming teleport requests. | `uxmessentials.tpa.toggle` |
| `/tpunblock` | Block a player's requests. | `uxmessentials.tpa.block` |
| `/up` | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. | `uxmessentials.tp.vertical` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.back.ondeath` | everyone | Allow /back and /deathback to return to a death location. |
| `uxmessentials.back.use` | everyone | /back to return to your last captured location; /deathback (alias /dback) to return to your last death location. |
| `uxmessentials.module.teleport` | op | Hot-reload / inspect the teleport module (/tp, /tpa, /back, /spawn and /rtp). |
| `uxmessentials.rtp.biome` | everyone | /rtp biome \<biome> to random teleport into a specific biome. |
| `uxmessentials.rtp.gui` | everyone | /rtp gui to open the random-teleport world picker. |
| `uxmessentials.rtp.others` | op | /rtp \<player> to force another online player to random teleport (staff). |
| `uxmessentials.rtp.radius.<blocks>` | everyone | How far from the world centre /rtp may drop you; the largest tier held wins. |
| `uxmessentials.rtp.use` | everyone | /rtp random teleport from the pre-warmed safe-location queue. |
| `uxmessentials.spawn.named` | everyone | /spawn \<name> to teleport to a named spawn. |
| `uxmessentials.spawn.set` | op | /setspawn, /setmainspawn, /removespawn and /mirrorspawn to define and manage spawns. |
| `uxmessentials.spawn.use` | everyone | /spawn to teleport to the resolved server spawn. |
| `uxmessentials.teleport.gui` | op | Show the teleport settings panel on the /uxmess gui hub. |
| `uxmessentials.teleport.settpr` | op | /settpr \<minRange> \<maxRange> to set the /rtp search zone at runtime. |
| `uxmessentials.tp.all` | op | /tpall to pull every online player to you. |
| `uxmessentials.tp.offline` | op | /tpoffline / /tpofflinehere to a player's logout location. |
| `uxmessentials.tp.others` | op | /tpo and /tpohere to teleport overriding no-tp flags. |
| `uxmessentials.tp.position` | op | /tppos to teleport to raw coordinates. |
| `uxmessentials.tp.use` | op | /tp, /tphere, /goto, /bring and /tprandomplayer (/tprp) direct staff teleport. |
| `uxmessentials.tp.vertical` | op | /top, /bottom, /jump, /up, /down, /ascend, /descend, /thru vertical teleports. |
| `uxmessentials.tp.warmup.<seconds>` | everyone | The stand-still countdown before a teleport runs, in seconds; the shortest tier held wins and 0 removes it. |
| `uxmessentials.tp.warmup.bypass` | op | Start teleports with no warmup, immune to move-cancel. |
| `uxmessentials.tpa.all` | op | /tpaall to request every online player to teleport to you. |
| `uxmessentials.tpa.auto` | everyone | /tpauto to auto-accept incoming teleport requests. |
| `uxmessentials.tpa.block` | everyone | /tpblock / /tpunblock to block a player's requests. |
| `uxmessentials.tpa.cancel` | everyone | /tpcancel / /tpacancel to withdraw your outgoing request. |
| `uxmessentials.tpa.toggle` | everyone | /tptoggle to refuse all incoming teleport requests. |
| `uxmessentials.tpa.use` | everyone | /tpa, /tpaccept, /tpdeny to request and resolve a teleport. |
| `uxmessentials.tpahere.use` | everyone | /tpahere to ask a player to come to you. |
| `uxmessentials.tpsettings.use` | everyone | /tpsettings opens your personal teleport settings panel. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `cooldown-start-phase` | `"teleport"` | when the cooldown clock starts: request \| accept \| teleport |
| `request-ttl-seconds` | `60` | how long a /tpa request stays pending |
| `single-request-displace` | `true` | a new request replaces a pending one instead of queueing |
| `default-warmup` | `3` | seconds, when the player holds no uxmessentials.teleport.warmup.\<n> |
| `default-cooldown` | `5` | seconds, when the player holds no uxmessentials.teleport.cooldown.\<n> |
| `teleport-to-center` | `true` | snap the destination to the block centre |
| `arrival-title` | `true` | master toggle: flash a short title/message on the screen when a teleport lands |
| `arrival-effects` | `true` | master toggle: play a sound/particle when a teleport lands |
| `effects.home.enabled` | `true` |  |
| `effects.home.sound` | `"ENTITY_ENDERMAN_TELEPORT"` |  |
| `effects.home.sound-volume` | `1.0` |  |
| `effects.home.sound-pitch` | `1.0` |  |
| `effects.home.particle` | `"END_ROD"` |  |
| `effects.home.particle-count` | `30` |  |
| `effects.home.particle-spread` | `0.3` |  |
| `effects.spawn.enabled` | `true` |  |
| `effects.spawn.sound` | `"ENTITY_ENDERMAN_TELEPORT"` |  |
| `effects.spawn.sound-volume` | `1.0` |  |
| `effects.spawn.sound-pitch` | `1.0` |  |
| `effects.spawn.particle` | `"END_ROD"` |  |
| `effects.spawn.particle-count` | `30` |  |
| `effects.spawn.particle-spread` | `0.3` |  |
| `effects.rtp.enabled` | `true` |  |
| `effects.rtp.sound` | `"ENTITY_ENDERMAN_TELEPORT"` |  |
| `effects.rtp.sound-volume` | `1.0` |  |
| `effects.rtp.sound-pitch` | `1.0` |  |
| `effects.rtp.particle` | `"END_ROD"` |  |
| `effects.rtp.particle-count` | `30` |  |
| `effects.rtp.particle-spread` | `0.3` |  |
| `effects.back.enabled` | `true` |  |
| `effects.back.sound` | `"ENTITY_ENDERMAN_TELEPORT"` |  |
| `effects.back.sound-volume` | `1.0` |  |
| `effects.back.sound-pitch` | `1.0` |  |
| `effects.back.particle` | `"END_ROD"` |  |
| `effects.back.particle-count` | `20` |  |
| `effects.back.particle-spread` | `0.3` |  |
| `effects.warp.enabled` | `false` |  |
| `effects.warp.sound` | `"ENTITY_ENDERMAN_TELEPORT"` |  |
| `effects.warp.sound-volume` | `1.0` |  |
| `effects.warp.sound-pitch` | `1.0` |  |
| `effects.warp.particle` | `"END_ROD"` |  |
| `effects.warp.particle-count` | `30` |  |
| `effects.warp.particle-spread` | `0.3` |  |
| `arrival-messages.home` | `[...]` |  |
| `arrival-messages.warp` | `[]` | disabled by default since warps configure their own welcome messages in warp editor |
| `arrival-messages.spawn` | `[...]` |  |
| `arrival-messages.rtp` | `[...]` |  |
| `arrival-messages.back` | `[...]` |  |
| `arrival-messages.request` | `[...]` |  |
| `arrival-messages.positional` | `[...]` |  |
| `cooldowns.back` | `-1` |  |
| `cooldowns.home` | `-1` |  |
| `cooldowns.warp` | `-1` |  |
| `cooldowns.spawn` | `-1` |  |
| `cooldowns.rtp` | `-1` |  |
| `warmup.cancel-on-move` | `true` |  |
| `warmup.cancel-on-rotate` | `false` |  |
| `warmup.cancel-on-damage` | `false` |  |
| `warmup.cancel-on-interact` | `false` |  |
| `warmup.move-threshold` | `0.0` | blocks of movement tolerated before a warmup cancels |
| `back.on-death` | `true` | /back may return to a death location |
| `back.death-delay-seconds` | `0` | seconds after dying before /back may return to the death point (0 = off) |
| `back.ignored-causes` | `["ender_pearl", "chorus_fruit"]` | causes that never overwrite the /back point |
| `combat.block-teleport` | `true` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_teleport_accepting%` | Whether the player accepts incoming teleport requests (yes/no). |
| `%uxmessentials_teleport_back_available%` | Whether the player has a location to return to with /back (yes/no). |
| `%uxmessentials_teleport_back_world%` | The world the player's /back location sits in. |
| `%uxmessentials_teleport_back_x%` | The block x of the player's /back location. |
| `%uxmessentials_teleport_back_y%` | The block y of the player's /back location. |
| `%uxmessentials_teleport_back_z%` | The block z of the player's /back location. |
| `%uxmessentials_teleport_cooldown_remaining%` | The wait left before the player may teleport again, in the compact 1m30s form. |
| `%uxmessentials_teleport_cooldown_remaining_formatted%` | The same remaining teleport cooldown, under the spelling a config may prefer. |
| `%uxmessentials_teleport_tpa_incoming%` | How many teleport requests are waiting for the player's answer. |
| `%uxmessentials_teleport_tpa_pending%` | How many teleport requests the player has sent and not had answered. |
| `%uxmessentials_teleport_warmup_remaining%` | The stand-still countdown left on the teleport in progress. |
| `%uxmessentials_teleport_warmup_remaining_formatted%` | The same remaining warmup, under the spelling a config may prefer. |
{/* /generated */}

## Notes

- **Moving cancels a warmup.** That is the rule that stops a player teleporting out of a losing fight, and it
  applies to every teleport that has one. The `warmup` block decides whether rotating, taking damage or
  interacting also cancels, and `uxmessentials.tp.warmup.bypass` skips the whole thing. Setting
  `default-warmup = 0` removes the protection along with the wait.
- **Warmups and cooldowns are numbered tiers:** `uxmessentials.tp.warmup.<seconds>` and
  `uxmessentials.tp.cooldown.<seconds>`, highest match wins, with a bypass node for each.
  `cooldown-start-phase` decides whether the clock starts when the command is run or when the player lands.
- **`/back` returns to the last teleport, not the last death,** until you grant `uxmessentials.back.ondeath` and
  set `back.on-death = true`. That way whether dying can be undone is a per-rank decision.
- **`/rtp` is instant because the queue is pre-warmed.** Safe spots are searched in the background, off the main
  thread, and handed out on demand; the queue survives a restart. When players report failures, the search
  budget (`max-attempts`, `max-wall-clock-ms`) or `excluded-biomes` is usually starving it.
- **RTP lands in real wilderness.** With `respect-claims` and `respect-worldguard` on, a candidate inside a land
  claim or a WorldGuard region is rejected. Both need no setup beyond installing the other plugin.
- **Spawns are plural.** There is a main spawn, any number of named ones reachable with `/spawn <name>`, and
  `/mirrorspawn <world>` to hand a new world off to an existing spawn instead of a fresh one.
- **Combat blocking is borrowed, not invented.** With CombatLogX or PvPManager installed and
  `combat.block-teleport` on, a tagged player cannot self-teleport with `/home`, `/warp`, `/spawn`, `/back`,
  `/rtp` or `/tpa`. With neither installed nothing is ever blocked, and staff teleports are never affected.

Related: [Homes](homes.md), [Warps](warps.md), [Worlds](worlds.md)
