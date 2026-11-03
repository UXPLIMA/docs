---
title: Deaths and kills
order: 404
description: What a death does to a fighter, how a kill is credited, streaks, the ratio, and respawning.
icon: skull
---

A fighter who dies loses nothing and leaves the fight. The record says who won.

## A death

| What | Happens |
|---|---|
| Items | Nothing is dropped and no experience is lost |
| The fight | Ends. The player respawns with PvP off and their own inventory back |
| The record | One death; the streak goes back to 0 |
| Tags | The player's tag ends, and their killer's |

A death of a player who is not fighting is not this plugin's: the world's own rules apply.

## A kill

The server names a killer for a player who dies after a hit from another player. When it does:

| For the killer | For the victim |
|---|---|
| One kill, one more on the streak, and the best streak raised if the streak passes it | One death, the streak back to 0 |
| The `kills` board gains one; the `streak` board records the new streak | |
| The kill reward is paid ([Rewards](rewards.md)) | |
| The `kill` moment: *You beat Alex. Streak: 3*, and a level-up sound | The `death` moment: *Steve beat you.* |
| The `kill-paid` moment, when the reward paid | |

A death with no killer, such as a fall nobody caused, counts the death and plays no moment.

## The ratio

Kills divided by deaths, written with two decimals: `2.47`. A player with no deaths has their kills as their ratio.

## Respawning

```hocon
respawn {
  instant = true
  spawn { world = "", x = 0.5, y = 64.0, z = 0.5, yaw = 0.0, pitch = 0.0 }
}
```

| Key | Shipped | What it does |
|---|---|---|
| `instant` | `true` | Skips the death screen: the player is respawned on the next tick |
| `spawn.world` | `""` | The world a dead fighter wakes up in. Blank leaves the place to the server or the lobby plugin |
| `spawn.x`, `y`, `z`, `yaw`, `pitch` | `0.5`, `64.0`, `0.5`, `0.0`, `0.0` | The point in that world |

A world that is not loaded leaves the place to the server, as a blank one does.

Related: [Combat tag](combat-tag.md), [Rewards](rewards.md), [Moments](moments.md)
