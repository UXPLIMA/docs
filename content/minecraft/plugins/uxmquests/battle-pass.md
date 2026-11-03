---
title: "Battle pass"
order: 20
description: "A pass is a ladder of tiers over two tracks, fed by finished quests and by raw play."
---

A pass is a ladder of tiers over two tracks, fed by finished quests and by raw play. It is off until
`battlepass.enabled = true` in `config.conf`. Nothing is read, registered or drawn while it is off.

A server runs as many passes at once as it likes, such as a permanent season pass beside event passes. Each
has its own ladder, tasks and claims, and every command and window row names its pass.

## Experience

Two sources, each with its own switch under `battlepass.sources`:

```hocon
battlepass {
  enabled = true
  sources {
    tasks = true                 # a finished quest is worth the pass file's tasks.per-quest
    activity {
      enabled = true             # raw play
      any-block { xp = 1, chance = 0.05 }
      any-mob   { xp = 2, chance = 0.5 }
      blocks { DIAMOND_ORE { xp = 25, chance = 1.0 } }
      mobs   { WITHER { xp = 500, chance = 1.0 } }
    }
  }
  leaderboard-size = 10
}
```

`chance` is how often the value pays. Activity experience reads the same events as quest progress, so the
[anti-glitch](objectives.md#anti-glitch) levels cover it and a block a player just placed is worth nothing. An
entry removed from `blocks` or `mobs` stays removed when a release merges new keys into `config.conf`.

## A pass file

```hocon
pass {
  name    = "@pass.season.name"
  premium = "uxmquests.pass.season.premium"
  season  { started = "2026-09-01T00:00:00Z", length = "90d" }
  unclaimed { rule = "hold", for = "7d" }
  tasks   { groups = [ daily ], per-quest = 50 }
}
tiers = [
  { tier = 1, at = 100, free = ["[message] @pass.tier.reached tier=1"] }
  { tier = 3, at = 500, free = ["..."], premium = ["..."] }
]
```

- **Premium is a permission** named by `premium`, and nothing else. The plugin never learns about money: a
  timed sale is a timed permission that the permission plugin grants and expires. Leave it empty for a pass
  with one track.
- **`tasks.groups`** names the quest groups whose finished quests feed the pass, each worth `per-quest`.
  Daily, weekly and season-long tasks are quest groups.
- **`season.length`** may be left out for a pass that never ends. A pass that names no `started` begins at the
  epoch.
- **The season resets when it is looked at.** Nothing sweeps a table at midnight. The season experience and
  the claims go; the all-time total stays.
- **`unclaimed`** decides what happens to a reward that was earned and never taken when the season closes:
  `pay` pays it, `hold` keeps it for `for` into the next season, `lose` drops it.
- **Tiers** are a list. `at` is the experience the tier asks for, `free` and `premium` are action lines, the
  same as a quest reward.

## Players

`/quests pass` lists every pass and where the player stands. `/quests pass claim <pass>` takes every reward
reached and not yet taken, all tiers at once. The windows `passes` and `pass` draw the list and the ladder;
open them from a button with an `open:passes` action in a menu file.

## Boosters and top

```
/uxmquests pass xp <player> <amount>
/uxmquests pass booster <player|server> <multiplier> <time>
/uxmquests pass top <pass> <season>
```

A booster multiplies experience, for one player or, with `server`, for everybody. A player booster and the
server booster multiply together. `pass xp` gives experience by hand with every booster applied. `pass top`
prints the standing of a season; season `0` is the all-time board.

## Board hologram

`battlepass.board` puts a hologram of the top of one pass in a world, refreshed every `refresh-seconds`. Set
`enabled`, `pass`, `world`, `x`, `y` and `z`. A world another plugin loads later is not an error: the console
says the board stayed down and the rest of the pass runs. Each viewer reads the title in their own language.
