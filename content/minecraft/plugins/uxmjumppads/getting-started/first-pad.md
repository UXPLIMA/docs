---
title: Your first pad
order: 104
description: Use a shipped pad, aim one by hand, and write a pad type of your own.
icon: flag
---

## 1. A matched pad

Place a redstone block, put a light weighted pressure plate on it, and walk onto the plate. You are thrown forward
the way you face, about five blocks up, with a wind burst and a cloud. That is the shipped `classic` type, and every
plate on a redstone block of the server now does the same. A plate on any other block stays a plate.

A slime block on an emerald block is `bounce`: it throws straight up.

## 2. A pad that throws one way

A matched pad throws where the player looks. For a pad that always sends players the same way, down a path or over a
wall, stand on the block, face where the throw should go, and type:

```
/uxmjumppads set classic
```

The reply names the block and the direction: *The block at world 12 64 -3 is a classic pad, throwing at 180
degrees.* The block can be anything; it need not be a plate on redstone. `/uxmjumppads remove`, standing on it,
takes the pad away. `/uxmjumppads list` lists every set pad.

## 3. A pad type of your own

Add a block under `pads` in `config.conf`:

```hocon
pads {
  launcher {
    block      = "HEAVY_WEIGHTED_PRESSURE_PLATE"
    under      = "GOLD_BLOCK"
    forward    = 2.5
    upward     = 1.2
    permission = ""
    effects = [
      "[sound] entity.firework_rocket.launch 1 1"
      "[particle] firework 30 0.4"
    ]
  }
}
```

Run `/uxmjumppads reload`. A heavy plate on a gold block now throws far and high. `/uxmjumppads doctor` says if a key
was wrong. See [Pad types](../pads/types.md) for every key and how far a speed carries.

## 4. A pad for some players only

Give the type a node and grant it to a group:

```hocon
permission = "lobby.pads.vip"
```

Everyone else walks over the pad as over any plate.

Related: [Pad types](../pads/types.md), [Set pads](../pads/set-pads.md), [Effects](../pads/effects.md)
