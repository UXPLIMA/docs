---
title: Pass experience and boosters
order: 312
description: Experience from quests and raw play, boosters, the top of a pass and a board in the world.
icon: zap
---

## Sources

```hocon
battlepass {
  enabled = false
  sources {
    tasks = true
    activity {
      enabled   = false
      any-block { xp = 1, chance = 0.05 }
      any-mob   { xp = 2, chance = 0.5 }
      blocks { DIAMOND_ORE { xp = 25, chance = 1.0 }, ANCIENT_DEBRIS { xp = 40, chance = 1.0 } }
      mobs   { WITHER { xp = 500, chance = 1.0 }, ENDER_DRAGON { xp = 1000, chance = 1.0 } }
    }
  }
  leaderboard-size = 10
}
```

| Key | Meaning |
|---|---|
| `sources.tasks` | A finished quest of a group the pass names is worth its `tasks.per-quest` |
| `sources.activity.enabled` | Raw play counts; off by default |
| `any-block`, `any-mob` | What any broken block or killed mob is worth |
| `blocks`, `mobs` | What a named one is worth instead |
| `chance` | How often the value pays |

Activity experience reads the same events as quest progress, so the [anti-glitch](anti-glitch.md) levels cover it and
a block a player just placed is worth nothing. An entry you remove from `blocks` or `mobs` stays removed when a release
merges new keys into `config.conf`.

## Boosters

```
/uxmquests pass booster <player|server> <multiplier> <time>
/uxmquests pass xp <player> <amount>
```

A booster multiplies experience for one player or, with `server`, for everybody. A player booster and the server
booster multiply together. A booster that has run out is deleted. `pass xp` gives experience by hand with every booster
applied. The pass window shows the booster a player has.

## The top

`/uxmquests pass top <pass> <season>` prints the standing of a season, `leaderboard-size` (10) lines; season `0` is the
all-time board.

## A board in the world

```hocon
board {
  enabled         = false
  pass            = "season"
  world           = "world"
  x               = 0.5
  y               = 100.0
  z               = 0.5
  refresh-seconds = 30
}
```

A hologram of the top of one pass, refreshed every `refresh-seconds`. Each viewer reads the title in their own
language. A world another plugin loads later is not an error: the console says the board stayed down and the rest of
the pass runs.

Related: [Battle pass](battle-pass.md)
