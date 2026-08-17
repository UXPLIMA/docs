---
title: Scores and leaderboards
order: 5
description: Personal bests, the per-game global tables, and what stops a fake score.
icon: trophy
---

Two things are stored: a personal best per game in the player's profile, and a global top
table per game in an ordered store.

## Personal bests

```lua
Profile = {
    Store = "uxrAM_Profile",
    Version = 1,
},
```

The store used is `Store .. "_v" .. Version`, so the shipped values give
`uxrAM_Profile_v1`.

Bumping `Version` starts everyone from scratch. It is versioned separately from the
leaderboards so a profile-schema change does not have to wipe the boards.

A best is per game id, not per cabinet. Two `Snake` cabinets share one number.

## The leaderboards

```lua
Leaderboard = {
    Enabled = true,
    TopN = 25,
    RefreshSeconds = 30,
    Store = { Prefix = "uxrAM_LB_", Version = "v1" },
},
```

One ordered store per game, named `Prefix .. GameId .. "_" .. Version`. With the shipped
values, `Snake` publishes to `uxrAM_LB_Snake_v1`.

| Field | Default | Effect |
|---|---|---|
| `Enabled` | `true` | Publish and read at all |
| `TopN` | `25` | Rows shown on the panel |
| `RefreshSeconds` | `30` | How often the cached top table is re-read |

The board panel is part of the game-over screen and the framework builds it; there is no
board model to place in the world for this system.

Bumping `Store.Version` starts a fresh season on every game at once without touching
anybody's saved best.

## What stops a fake score

The games run on the client, so the score arrives from the client. Four checks run on the
server before anything is stored.

| Check | Rejects |
|---|---|
| The game id must exist in `Settings.Games` | Scores for games you do not have |
| The score must be a real, finite, non-negative number | `nan`, `inf`, negatives, non-numbers |
| The score must not exceed that game's `MaxScore` | Absurd numbers |
| Submissions are rate limited by `SubmitCooldown` | Flooding the store |

`SubmitCooldown` ships at one second, per player rather than per game.

<Callout type="warning" title="MaxScore is the only ceiling, so set it deliberately">

A client-hosted game cannot be fully verified from the server: a determined player can
submit any score under the cap. `MaxScore` is what decides how bad that is.

The shipped values are chosen per game to be well above a genuine run and well below
absurd. If you add a game, set `MaxScore` to a few times the best score you can achieve
yourself, not to a round large number.

</Callout>

This is the standard trade for arcade games in Roblox. Server-authoritative arcade games
mean replaying every frame on the server, which costs more than the problem is worth for a
cabinet in the corner of a lobby.

If a game's score matters enough to be worth cheating for, do not pay it into your economy
directly. Cap what a leaderboard position can earn.

## What the submission returns

```lua
{ ok = true, best = 4200, newBest = true, rank = 12, top = { … } }
```

The game-over screen uses all four: the score, whether it beat the old best, where it
places, and the table to show.

A rejected submission comes back with `ok = false` and the player's existing best, so a
throttled submission does not appear to erase their record.

## Messages

```lua
Messages = {
    GameOver = "GAME OVER",
    NewBest = "NEW BEST!",
    Empty = "No scores yet. Be the first!",
},
```

Three strings, safe to translate.
