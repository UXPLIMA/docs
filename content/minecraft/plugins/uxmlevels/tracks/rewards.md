---
title: Rewards
order: 303
description: Rewards at a level, manual and automatic, the lines they run, and when they can be taken again.
icon: gift
---

A reward is a block under `rewards { }` in a track file, keyed by an id.

```hocon
rewards {
  starter {
    level = 5
    icon = "IRON_INGOT"
    name = "@track.global.starter.name"
    lore = ["@track.global.starter.lore"]
    actions = ["[console] give {player} iron_ingot 16"]
    claim = "manual"
  }
}
```

## The keys

| Key | Default | What it does |
|---|---|---|
| the block name | | The reward's id: 1 to 48 lower-case letters, digits and hyphens |
| `level` | `1` | The level that unlocks it |
| `icon` | `"CHEST"` | Its tile in the rewards window |
| `name` | the id | Its name: your own MiniMessage, or `@key` |
| `lore` | `[]` | Its description lines, each your own or `@key` |
| `actions` | `[]` | What it does, as action lines |
| `claim` | `"manual"` | `manual`: claimed with a click in the rewards window. `automatic` (or `auto`): runs the moment the level is reached |

## Manual and automatic

| | Manual | Automatic |
|---|---|---|
| Taken | With a click in the [rewards window](../menus/rewards.md), at the level or any time after | When an earning or `/uxmlevels give` crosses the level |
| Message | `reward.claimed` | `reward.granted-automatically` |
| Missed | Waits in the window | A level reached by `/uxmlevels set` runs nothing |

Each reward is taken once per climb; the claim is stored. A [prestige](prestige.md) clears the claims of that track,
so every reward can be taken again on the next climb. `/uxmlevels reset` clears them on every track.

An automatic reward is marked as taken and runs only for a player who is online at that moment. A level crossed by
`/uxmlevels give` on a player who is away marks the reward without running it.

## Writing a line

A line is `[verb] payload`, the same lines every UXM plugin runs: `[console]`, `[player]`, `[message]`,
`[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[sound]`, `[effect]`, `[particle]`,
`[give-money]`, `[take-money]`, `[take-item]`, `[close]`, each with an optional `{delay=, chance=, repeat=, every=,
if=}` block. `{player}`, `<player>`, `%player%` and `%player_name%` are the player's name; for a player whose name is
not a plain account name the line is skipped with a warning. Text starting with `@` is a line of the messages files.

`[give-money] <economy> <amount>` pays into `exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`,
`ecobits:<currency>` or `treasury`; see [Economies](../integrations/economies.md). A line that cannot be read is
skipped with a warning; the others run.

## The shipped rewards

| Track | Id | Level | Gives | Claim |
|---|---|---|---|---|
| `global` | `starter` | 5 | 16 iron ingots | manual |
| `global` | `settled` | 25 | 16 gold ingots | manual |
| `global` | `veteran` | 50 | 8 diamonds | automatic |
| `mining` | `efficient` | 20 | A diamond pickaxe | manual |
| `combat` | `blooded` | 15 | A shield | manual |

Their names and lore are under `track.<track>.<reward>` in the messages files.

Related: [Rewards window](../menus/rewards.md), [Prestige](prestige.md)
