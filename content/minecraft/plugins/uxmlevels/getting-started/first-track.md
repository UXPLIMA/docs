---
title: Your first track
order: 204
description: Add a woodcutting track, an event that pays into it, and a reward at level 5.
icon: play
---

This walk-through adds a woodcutting track to the shipped files.

## 1. The track

Create `plugins/uxmLevels/tracks/woodcutting.conf`:

```hocon
display-name = "<good>Woodcutting"
icon = "IRON_AXE"

maximum-level = 50
base-cost = 50
growth = 1.12

hud {
  experience-bar = false
  boss-bar = false
  action-bar = false
}

rewards {
  lumberjack {
    level = 5
    icon = "DIAMOND_AXE"
    name = "<good>Lumberjack"
    lore = ["<subtext>An axe that lasts."]
    actions = ["[console] give {player} diamond_axe 1"]
    claim = "manual"
  }
}
```

The file name is the id, so the track is `woodcutting`.

## 2. The event

Create `plugins/uxmLevels/events/woodcutting.conf`:

```hocon
events {
  woodcutting-logs {
    track = "woodcutting"
    type = "mining"
    filter = ["OAK_LOG", "SPRUCE_LOG", "BIRCH_LOG", "JUNGLE_LOG", "ACACIA_LOG", "DARK_OAK_LOG", "MANGROVE_LOG", "CHERRY_LOG"]
    amount = 4
    chance = 1.0
    cooldown-ms = 0
    action-bar = true
  }
}
```

Breaking a log is a `mining` event: any block broken counts, filtered to the logs. Event ids are unique across
every file in `events/`.

## 3. Reload

Run `/uxmlevels reload`. Break a log of a tree: the action bar shows `+4 Woodcutting`. A log placed by hand pays
nothing when broken again, and neither does the block a sapling was planted in: see
[Anti-abuse](../tracks/anti-abuse.md).

## 4. Level up and claim

```
/uxmlevels set Steve woodcutting 5
Steve is now level 5 in Woodcutting.
```

Steve runs `/levels`, clicks Woodcutting, and clicks Lumberjack: `You claimed Lumberjack.` and the axe arrives.

## 5. See it

```
/levels level woodcutting
Woodcutting: level 5 of 50, 0 of 80.
/levels top woodcutting
```

`%uxmlevels_woodcutting_level%` shows the level on a scoreboard.

## Next

- [Events](../tracks/events.md): every type and key.
- [Rewards](../tracks/rewards.md): manual and automatic rewards.
- [Prestige](../tracks/prestige.md): the reset with a multiplier.
