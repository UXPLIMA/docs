---
title: Teleport & RTP
order: 970
---

## Teleportation

The `teleport` module is the busiest command surface in uxmEssentials. It covers player-to-player requests, return-to-last-spot, random wilderness teleports, spawns, staff teleports, and a whole family of vertical/directional jumps: all governed by a shared warmup and cooldown system.

<Callout type="warning" title="Move cancels warmup">

Teleports with a configured warmup are cancelled if you move (and, depending on config, rotate, take damage, or interact) before the countdown finishes. Stand still until the teleport fires. Holders of `uxmessentials.tp.warmup.bypass` skip the warmup entirely.

</Callout>

---

## Teleport Requests (TPA)

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/tpa <player>` | - | Request to teleport to a player | `uxmessentials.tpa.use` |
| `/tpahere <player>` | - | Ask a player to teleport to you | `uxmessentials.tpahere.use` |
| `/tpaccept` | `tpyes` | Accept a pending request | `uxmessentials.tpa.use` |
| `/tpdeny` | `tpno` | Deny a pending request | `uxmessentials.tpa.use` |
| `/tpcancel` | `tpacancel` | Withdraw your outgoing request | `uxmessentials.tpa.cancel` |
| `/tpalist` | `tprequests` | List players waiting on your reply | `uxmessentials.tpa.use` |
| `/tptoggle` | `toggletp` | Refuse all incoming requests (toggle) | `uxmessentials.tpa.toggle` |
| `/tpon` · `/tpoff` | - | Idempotently accept / refuse incoming requests | `uxmessentials.tpa.toggle` |
| `/tpauto` | - | Auto-accept incoming requests (toggle) | `uxmessentials.tpa.auto` |
| `/tpblock <player>` · `/tpunblock <player>` | - | Block/unblock a player's requests | `uxmessentials.tpa.block` |
| `/tpaall` | - | Request every online player to teleport to you | `uxmessentials.tpa.all` |

### Examples

```
/tpa Steve
/tpaccept
/tpahere Alex
/tptoggle
/tpblock Griefer123
```

---

## Return & Random Teleport

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/back` | `return` | Return to your last captured location | `uxmessentials.back.use` |
| `/deathback` | `dback` | Return to your last death location | `uxmessentials.back.use` |
| `/rtp` | `wild` | Opens the world-picker GUI by default (config `command-opens-gui`; off = random teleport from the pre-warmed safe queue) | `uxmessentials.rtp.use` |
| `/settpr <minRange> <maxRange>` | - | Set the `/rtp` search zone at runtime | `uxmessentials.teleport.settpr` |

<Callout type="info" title="Return-on-death is a grant">

Being able to `/back` to where you died requires the `uxmessentials.back.ondeath` marker node. Without it, `/back` and `/deathback` only return you to your last *teleport* location.

</Callout>

### Examples

```
/back
/rtp
/settpr 500 5000
```

---

## Spawns

| Command | Description | Permission |
|---------|-------------|------------|
| `/spawn [name]` | Teleport to the server spawn or a named spawn | `uxmessentials.spawn.use` / `.spawn.named` |
| `/setspawn [name]` | Set the (named) spawn point | `uxmessentials.spawn.set` |
| `/setmainspawn` | Set the primary server spawn | `uxmessentials.spawn.set` |
| `/removespawn` | Delete a spawn | `uxmessentials.spawn.set` |
| `/mirrorspawn <world>` | Mirror another world's spawn | `uxmessentials.spawn.set` |

### Examples

```
/spawn
/spawn pvp
/setspawn
/setspawn pvp
/mirrorspawn world_nether
```

---

## Staff Teleports

These pull players around directly, without a request handshake, and default to `op`.

| Command | Aliases | Description | Permission |
|---------|---------|-------------|------------|
| `/tp <player>` · `/tphere <player>` · `/goto <player>` · `/bring <player>` | - | Direct staff teleport to/from a player | `uxmessentials.tp.use` |
| `/tp <x> <y> <z> [world]` · `/goto …` · `/tppos …` | - | Teleport to raw coordinates | `uxmessentials.tp.position` |
| `/tprandomplayer` | `tprp` | Teleport to a random visible player | `uxmessentials.tp.use` |
| `/tpo <player>` · `/tpohere <player>` | - | Teleport overriding no-tp flags | `uxmessentials.tp.others` |
| `/tpall` | - | Pull every online player to you | `uxmessentials.tp.all` |
| `/tpoffline <player>` · `/tpofflinehere <player>` | - | To/from a player's logout location | `uxmessentials.tp.offline` |

### Examples

```
/tp Steve
/tphere Alex
/tppos 100 64 -250 world
/tpall
/tpoffline Steve
```

---

## Vertical & Directional

| Command | Description | Permission |
|---------|-------------|------------|
| `/top` `/bottom` `/jump` `/up [n]` `/down` `/ascend` `/descend` `/thru` | Vertical/directional teleports (to surface, void floor, crosshair target, up/down N blocks, through a wall) | `uxmessentials.tp.vertical` |

### Examples

```
/top
/jump
/up 10
/thru
```

---

## Personal Settings

| Command | Description | Permission |
|---------|-------------|------------|
| `/tpsettings` | Open your personal teleport settings panel | `uxmessentials.tpsettings.use` |

---

## Warmups, Cooldowns & Bypasses

Warmups and cooldowns are granted through numbered permission tiers, so different ranks can wait different amounts of time (highest matching value wins):

| Node | Purpose |
|------|---------|
| `uxmessentials.tp.warmup.<seconds>` | Warmup countdown before a teleport fires |
| `uxmessentials.tp.cooldown.<seconds>` | Cooldown before the next teleport |
| `uxmessentials.tp.warmup.bypass` | Skip warmups entirely |
| `uxmessentials.tp.cooldown.bypass` | Skip cooldowns entirely |
| `uxmessentials.back.ondeath` | Allow `/back` to return to a death location |

---

## Next Steps

- [🌀 Teleport & RTP](../features/teleport-rtp.md) - Request flow, the pre-warmed RTP queue, spawns, and staff teleports
- [🔐 Permission Reference](../permissions/reference.md) - Warmup/cooldown tiers and bypass nodes
- [⚙️ Command Configuration](../config/commands-conf.md) - Rename or disable teleport commands
