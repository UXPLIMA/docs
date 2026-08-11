---
title: Player State
order: 1000
---

## Player State

The **Player State** module bundles the everyday self-and-target verbs: healing, flight, game mode, speed, personal time/weather, XP, and a stack of read-only info commands. Every command is Brigadier-registered and can be renamed, re-aliased, or disabled in [`commands/commands.conf`](../config/commands-conf.md); permission nodes stay keyed to the command id regardless of the name you give it.

<Callout type="info" title="Targeting another player">

Most verbs accept an optional `[player]`. Running one on yourself needs only the command's own node, but the moment you pass a target, you **also** need `uxmessentials.playerstate.others`. Without it, the `[player]` argument is refused.

</Callout>

---

## Self buffs & body state

| Command | Aliases | Permission |
|---------|---------|------------|
| `/god [player]` | `godmode` | `uxmessentials.god.use` |
| `/fly [player]` | - | `uxmessentials.fly.use` |
| `/heal [player]` | - | `uxmessentials.heal.use` |
| `/feed [player]` | `eat` | `uxmessentials.feed.use` |
| `/foodlevel <amount> [player]` | - | `uxmessentials.foodlevel.use` |
| `/health <amount> [player]` | - | `uxmessentials.health.use` |
| `/ext [player]` | `extinguish` | `uxmessentials.extinguish.use` |
| `/nightvision` | `nv` | `uxmessentials.nightvision.use` |
| `/glow` | - | `uxmessentials.glow.use` |
| `/air <seconds> [player]` | - | `uxmessentials.air.use` |
| `/burn <seconds> [player]` | - | `uxmessentials.burn.use` |
| `/ice [player] [seconds]` | - | `uxmessentials.ice.use` |
| `/suicide` | - | `uxmessentials.suicide.use` |
| `/rest [player]` | - | `uxmessentials.rest.use` |

---

## Game mode & movement

| Command | Aliases | Permission |
|---------|---------|------------|
| `/gamemode <mode> [player]` | `gm` | `uxmessentials.gamemode.use` |
| `/gmc` · `/gms` · `/gma` · `/gmsp [player]` | - | `uxmessentials.gamemode.use` |
| `/speed <value> [player]` | - | `uxmessentials.speed.use` |
| `/walkspeed <value>` · `/flyspeed <value>` | - | `uxmessentials.speed.use` |

`/gmc`, `/gms`, `/gma`, `/gmsp` are shorthands for creative, survival, adventure, and spectator: the same node as `/gamemode`.

---

## Inventory

| Command | Aliases | Permission |
|---------|---------|------------|
| `/clearinventory [player]` | `ci`, `clear` | `uxmessentials.clearinventory.use` |
| `/clearinventoryconfirmtoggle` | `citoggle` | `uxmessentials.clearinventory.confirmtoggle` |
| `/invsee [player]` | - | `uxmessentials.invsee.use` |
| `/endersee [player]` | - | `uxmessentials.endersee.use` |

<Callout type="note" title="Editing what you open">

`/invsee` opens the target's inventory read-only by default. To actually move items in and out, the viewer additionally needs `uxmessentials.invsee.modify`. `/citoggle` flips whether **your own** `/clearinventory` asks for confirmation first.

</Callout>

---

## Personal time & weather

These change what **you** see client-side; they never touch the world for other players.

| Command | Permission |
|---------|------------|
| `/ptime <value\|reset>` | `uxmessentials.ptime.use` |
| `/pweather <clear\|rain\|reset>` | `uxmessentials.pweather.use` |

---

## Experience

| Command | Aliases | Permission |
|---------|---------|------------|
| `/exp get\|set\|give\|take\|reset [amount] [levels\|points] [player]` | `xp` | `uxmessentials.exp.use` |

Pick `levels` or `points` to choose the unit; omit it to use the default. As with every verb here, targeting `[player]` requires `uxmessentials.playerstate.others`.

---

## Information & location

Read-only lookups. Each has its own node so you can hand them out individually.

| Command | Aliases | Permission |
|---------|---------|------------|
| `/near [radius]` | `nearby` | `uxmessentials.near.use` |
| `/getpos [player]` | `coords`, `whereami` | `uxmessentials.getpos.use` |
| `/depth` | - | `uxmessentials.depth.use` |
| `/biome` | - | `uxmessentials.biome.use` |
| `/seed` | - | `uxmessentials.seed.use` |
| `/compass` | - | `uxmessentials.compass.use` |
| `/world` | - | `uxmessentials.world.use` |
| `/dimension` | `dim` | `uxmessentials.dimension.use` |
| `/ping [player]` | - | `uxmessentials.ping.use` |
| `/playtime [player]` | - | `uxmessentials.playtime.use` |
| `/playtime reset\|resetall [player]` | - | `uxmessentials.playtime.reset` |

<Callout type="tip" title="`/world` vs `/worlds`">

`/world` here just reports the world you are standing in. The multi-world management command is the plural `/worlds`, handled by the separate Worlds module.

</Callout>

---

## Examples

```
/heal                    # top up your own health and hunger
/heal Steve              # heal Steve (needs playerstate.others)
/fly                     # toggle flight
/gmc                     # switch yourself to creative
/gamemode survival Steve # put Steve back in survival
/speed 4                 # set movement speed (fly speed while flying)
/ptime night             # personal night sky, just for you
/exp give 30 levels      # grant yourself 30 XP levels
/near 50                 # list players within 50 blocks
/invsee Steve            # peek at Steve's inventory
```

---

## Next Steps

- [📖 Command Overview](overview.md): how every uxmEssentials command is organised
- [🧰 Item & World Commands](items-world.md): the related item/entity/admin verbs
- [🔐 Permissions Reference](../permissions/reference.md): the full `uxmessentials.*` node list
- [⚙️ Renaming Commands](../config/commands-conf.md): rename, re-alias, or disable any verb
