---
title: "Guide"
order: 10
description: "How uxmLevels works, how to set it up and what each file holds."
---

uxmLevels gives players experience and levels on as many tracks as a server wants: a global one, a mining one, a combat one, or any other. Events decide what pays into which track, every track has its own ladder and rewards, and a finished track can be prestiged for a permanent bonus.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmLevels/`, with three example tracks (`global`, `mining`, `combat`) and their events. The examples are written only while `tracks/` or `events/` does not exist: once the folder is there it is yours, and a file you delete stays deleted.
3. Edit the files under `tracks/` and `events/`, then run `/uxmlevels reload`.

PlaceholderAPI and the economy plugins are optional.

## Files

| File | What it holds |
|---|---|
| `config.conf` | language, economy, storage, experience rules, anti-abuse, leaderboard, HUD, prestige |
| `tracks/<id>.conf` | one track per file; the file name is the id |
| `events/*.conf` | what pays into which track |
| `menus/main.conf`, `rewards.conf`, `prestige.conf`, `top.conf` | the windows |
| `commands.conf`, `messages/`, `theme.conf` | commands, words and colours |

## Tracks

A track id is lower-case letters, digits and hyphens, without underscores. Renaming a file renames the track, and events that name the old id then pay nothing and say so in the console.

| Key | Meaning |
|---|---|
| `display-name`, `icon` | the name (`@key` reads it from the messages file) and the menu icon |
| `maximum-level` | the top of the ladder, 2 to 10000 |
| `base-cost` | what level one costs, at least 1 |
| `growth` | each level costs the one before times this, rounded, never less than one; at least `1.0` |
| `hud` | `experience-bar`, `boss-bar`, `action-bar`: which surface shows this track. Each surface holds one track, and the first file that asks takes it |
| `rewards` | rewards at levels (below) |

With `base-cost = 100` and `growth = 2.0`, levels cost 100, 200, 400 and so on. A curve whose totals would overflow is refused with the level where it happens.

A reward has a `level`, `icon`, `name`, `lore`, `actions` and `claim`: `manual` waits for the player to click it in the rewards window, `automatic` runs at once. Actions are `[console] <command>`, `[player] <command>` or `[message] <text>`; `{player}`, `<player>` and `%player%` are the player's name.

```hocon
rewards {
  veteran {
    level = 50
    icon = "DIAMOND"
    name = "@track.global.veteran.name"
    lore = ["@track.global.veteran.lore"]
    actions = ["[console] give {player} diamond 8"]
    claim = "automatic"
  }
}
```

## Events

Each block under `events` in any file of `events/` is one rule. An event id must be unique across every file, because its cooldown is kept under it.

| Key | Meaning |
|---|---|
| `track` | the track it pays into |
| `type` | `playtime`, `mining`, `combat`, `fishing`, `eating`, `crafting` or `chat` |
| `filter` | the blocks, creatures, catches, foods or results it answers to, without case; empty accepts all |
| `amount`, or `minimum` and `maximum` | a fixed amount or a range |
| `chance` | `0.0` to `1.0` |
| `cooldown-ms` | the gap between two payments of this event, per player |
| `action-bar` | show the gain on the action bar |
| `minimum-length` | for `chat`, the shortest message that pays |

Two events may fire for the same action, each with its own cooldown: a broad rule for every block and a bonus for ores, for example.

## Experience rules

| `experience` key | Meaning |
|---|---|
| `playtime-interval-ms` | how often `playtime` events pay; the amount is per interval |
| `idle-timeout-ms` | how long a still player keeps earning playtime; `0` turns the check off |
| `hourly-cap` | the most one player may earn on one track in a rolling hour; `0` is no cap. `/uxmlevels give` ignores it |
| `global-multiplier` | multiplies every earned amount |
| `permission-multipliers` | node to multiplier; the best one a player holds wins, they do not stack |

The final multiplier is global times permission times temporary (from `/uxmlevels boost`) times prestige, rounded down.

`anti-abuse.mark-player-placed-blocks` marks placed blocks in chunk data so breaking them pays nothing, keeping up to `marks-per-chunk` marks per chunk.

## Prestige

A player at `prestige.required-level` (or the track's maximum, if lower) can prestige that track with `/levels prestige <track>` or the prestige window. Their experience on the track goes back to zero and their prestige count rises, up to `maximum`.

| `prestige` key | Meaning |
|---|---|
| `money-cost` | charged in `economy.currency` (`exp`, `levels`, `vault`, `vault-unlocked`, `playerpoints`, `ecobits:<currency>`, `treasury[:<currency>]`); `0` asks nothing of the economy |
| `experience-cost` | experience the player must hold |
| `bonus-per-prestige` | added to the permanent multiplier per prestige, not compounded: `0.1` makes the third prestige earn 1.3 times |
| `default-badge`, `badges` | the badge for each prestige count; `<prestige>` is the count |

Prestiging needs `uxmlevels.prestige`, which only operators have by default.

## Leaderboard and HUD

`leaderboard` keeps a snapshot of `size` places per track, rebuilt every `refresh-seconds` (an older `refresh-ms` is carried over on start); `chat-lines` is how many `/levels top` prints. `periods`, `time-zone`, `week-starts` and `keep-windows` work as in the other UXM plugins: all time ranks by level, a month, week or day by experience earned in it. `hud.refresh-ms` repaints the surfaces and `gain-notice-ms` is how long a gain holds the action bar.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/levels` | `uxmlevels.use` | open the main window |
| `/levels level [track]` | `uxmlevels.use` | where you stand |
| `/levels level-of <player> <track>` | `uxmlevels.others` | where another player stands |
| `/levels top <track> [period]` | `uxmlevels.use` | the best players on a track |
| `/levels prestige <track>` | `uxmlevels.prestige` | prestige a finished track |
| `/levels lang ...` | `uxmlevels.language`, `uxmlevels.admin.language` | languages |
| `/uxmlevels give <player> <track> <amount>` | `uxmlevels.admin.give` | add experience |
| `/uxmlevels take <player> <track> <amount>` | `uxmlevels.admin.take` | take experience, never below zero |
| `/uxmlevels set <player> <track> <level>` | `uxmlevels.admin.set` | set a level |
| `/uxmlevels reset <player>` | `uxmlevels.admin.reset` | start a player again on every track and remove them from every board |
| `/uxmlevels boost <player> <multiplier> <duration>`, `unboost <player>` | `uxmlevels.admin.boost` | a temporary multiplier |
| `/uxmlevels reload`, `version`, `doctor` | `uxmlevels.admin.reload`, `.version`, `.doctor` | the usual operator commands |

The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

Every player earns on every track by default. To stop a player or a group earning on one track, set `uxmlevels.track.<id>` to false in the permission plugin, for example `uxmlevels.track.mining`; set `uxmlevels.track.*` to false to stop every track and then grant the ones they keep. A track's own node wins over the wildcard. An admin `give` or `set` is not held to these permissions, and a change is read when the player joins again or on `/uxmlevels reload`.

## Placeholders

Written `%uxmlevels_<track>_<what>%`:

| What | Value |
|---|---|
| `level`, `max_level` | the level held, and the top of the ladder |
| `experience`, `experience_formatted` | the total, plain or grouped by thousands |
| `into`, `next`, `percent` | experience into the level, still needed, and percent through it |
| `prestige`, `badge`, `badge_raw` | the prestige count, and the badge as plain text or as written |
| `multiplier` | what a gain is multiplied by |
| `position` | the leaderboard place, empty outside the snapshot |

The boards use the shared grammar: `%uxmlevels_top_<track>_<period>_<n>_name|value|head%`, `%uxmlevels_rank_<track>_<period>%`, `%uxmlevels_value_<track>_<period>%` and `%uxmlevels_resets_<period>%`. `leaderboard.empty-head` is the head for a place nobody holds.

## Troubleshooting

- **An event pays nothing.** Its `track` names a track that does not exist; the console says so on reload.
- **Mining a placed block pays nothing.** That is the anti-abuse mark.
- **Playtime stops paying.** The player has been still for `idle-timeout-ms`.
- **A prestige is refused for everyone.** `prestige.money-cost` is set and `economy.currency` names an economy the server does not run.
