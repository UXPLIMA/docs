---
title: Events
order: 402
description: The seven event types, every key of an event, how several events pay at once, and the shipped ten.
icon: zap
---

An event pays experience into one track when something happens. Events live in any file in `events/`, under an
`events { }` block, keyed by an id.

```hocon
events {
  mining-ores {
    track = "mining"
    type = "mining"
    filter = ["DIAMOND_ORE", "DEEPSLATE_DIAMOND_ORE"]
    minimum = 10
    maximum = 25
    chance = 1.0
    cooldown-ms = 0
    action-bar = true
  }
}
```

## The keys

| Key | Default | What it does |
|---|---|---|
| the block name | | The event's id: 1 to 48 lower-case letters, digits and hyphens. Unique across every file in `events/` |
| `track` | `"global"` | The track it pays into. A track no file declares makes the event pay nothing, with a console line |
| `type` | | What happened; see the table below. Required |
| `filter` | `[]` | The blocks, mobs, items or foods it answers to, by name, without regard to case. Empty is everything of that type |
| `amount` | `1` | A fixed amount |
| `minimum`, `maximum` | | A range instead of `amount`; each payment rolls a whole number between them |
| `chance` | `1.0` | The probability it pays, `0.0` to `1.0` |
| `cooldown-ms` | `0` | The least time between two payments of this event to one player |
| `action-bar` | `true` | Show the gain over the hotbar, `+12 Mining`, for `hud.gain-notice-ms` |
| `minimum-length` | `0` | For `chat` only: the shortest message, in characters, that pays |

The cooldown is taken only when the event pays, so a run of failed chance rolls does not hold it open. Cooldowns are
kept in memory and forgotten when the player leaves.

## The types

| `type` | Fires when | `filter` names |
|---|---|---|
| `playtime` | Every `experience.playtime-interval-ms`, for each player who is not idle | nothing |
| `mining` | A player breaks a block | The block, such as `DIAMOND_ORE` |
| `combat` | A player kills a mob or a player | The entity type, such as `ZOMBIE` or `PLAYER` |
| `fishing` | A player catches something on a line | The item caught, such as `COD` |
| `eating` | A player eats or drinks an item | The item, such as `COOKED_BEEF` or `POTION` |
| `crafting` | A player takes a crafted result | The result, such as `IRON_PICKAXE`. Pays per item of the result: a recipe that makes 4 pays 4 times the amount |
| `chat` | A player sends a chat message of at least `minimum-length` characters | nothing |

A block a player placed pays nothing when it is broken; see [Anti-abuse](anti-abuse.md). A kill pays only the player
who struck last.

## Several events at once

Every event that matches pays, each with its own chance and cooldown. Killing a player pays both `combat-any` and
`combat-player` in the shipped files; mining diamond ore pays `mining-any` and `mining-ores`. Write the filters so
the sums are what you mean.

## The amount paid

The amount rolled is multiplied by the [multipliers](multipliers-and-boosts.md), rounded down, and then held to the
[hourly cap](anti-abuse.md#hourly-cap).

## The shipped events

| Id | File | Track | Type | Filter | Amount | Chance | Cooldown | Action bar |
|---|---|---|---|---|---|---|---|---|
| `playtime` | general | global | playtime | | 10 | 1.0 | 0 | no |
| `fishing` | general | global | fishing | | 5 to 15 | 1.0 | 0 | yes |
| `eating` | general | global | eating | | 2 | 0.5 | 10 s | no |
| `crafting` | general | global | crafting | | 1 | 1.0 | 0 | no |
| `chat` | general | global | chat, 12 characters | | 1 | 1.0 | 60 s | no |
| `mining-any` | mining | mining | mining | | 1 | 1.0 | 0 | no |
| `mining-ores` | mining | mining | mining | coal, iron, gold, diamond and emerald ores and their deepslate forms, ancient debris | 10 to 25 | 1.0 | 0 | yes |
| `combat-any` | combat | combat | combat | | 3 | 1.0 | 0 | no |
| `combat-player` | combat | combat | combat | `PLAYER` | 25 to 50 | 1.0 | 120 s | yes |
| `combat-boss` | combat | combat | combat | `ENDER_DRAGON`, `WITHER`, `WARDEN` | 500 | 1.0 | 0 | yes |

## When a file is wrong

Two events with one id, a type the plugin does not know, a chance outside 0 to 1 or a negative amount stop the load,
and the console names the file and the event. At a reload the rules that worked stay.

Related: [Tracks](tracks.md), [Event files](../config/event-files.md), [Anti-abuse](anti-abuse.md)
