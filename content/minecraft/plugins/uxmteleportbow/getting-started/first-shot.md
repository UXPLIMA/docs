---
title: Your first shot
order: 104
description: Shoot the bow, see a refusal, change the cooldown, and close a world to it.
icon: play
---

This walk-through uses a fresh install and an operator account.

## 1. Shoot

Join. The bow is in hotbar slot 2. Hold it, draw it fully and release it at a block some distance away. You hear the
shot, a line of white particles follows the arrow, and when it lands you stand where it stopped, looking the way you
were looking, with a burst of portal particles and the enderman sound.

The arrow is gone, the arrow in your inventory is still there, and the bow shows a white sweep for three seconds.

## 2. See a refusal

Shoot straight at a wall with a one block high gap under a ceiling, or into a low tunnel. The arrow lands, but there
is no room for a standing player, so you stay where you are and read `There is no room to stand where the arrow
landed.`

The plugin gives its own cooldown back, but the sweep on the bow is the game's and runs its three seconds: the bow
cannot be drawn again until it ends.

## 3. Change the cooldown

Open `plugins/uxmTeleportBow/config.conf` and set:

```hocon
teleport {
  cooldown-ms = 1000
}
```

Run `/uxmteleportbow reload`. The next shot starts a one second cooldown, and the sweep on the bow lasts one second.

## 4. Close a world

Say the bow belongs in the lobby only. Set the world filter to allow that one world:

```hocon
filters {
  worlds {
    mode = "allow"
    list = ["lobby"]
  }
}
```

Run `/uxmteleportbow reload`. In any other world a shot is refused before the arrow leaves the bow, and a player who
joins or changes into another world is not given the bow (`item.give.respect-world-filter`).

## 5. Give it by hand

`/uxmteleportbow give Steve` hands Steve the bow and the arrow, in any world: the command ignores the world filter.
`/uxmteleportbow give` with no name gives them to you.

## Next

- [Shooting](../bow/shooting.md): every step of the shot and the landing.
- [Filters](../bow/filters.md): worlds, WorldGuard regions and Towny towns.
- [Effects](../bow/effects.md): what each moment does.
