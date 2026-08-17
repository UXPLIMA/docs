---
title: The AI opponent
order: 5
description: How the AI picks a shot, the three difficulties, and why its games are unrated.
icon: cpu
---

A player alone at a table can start a match against the AI. It takes the second seat, plays
by the same rules, and is beatable at every setting.

## Configuration

```lua
Config.ai = {
    FallbackUsername = "siracozmen01",
    UseFriendAvatar = true,
    difficulty = "medium",
    aimError = { easy = 0.09, medium = 0.045, hard = 0.015 },
    thinkSeconds = 1.0,
},
```

| Field | Default | What it does |
|---|---|---|
| `difficulty` | `"medium"` | Which entry of `aimError` is used |
| `aimError` | see above | Random aim drift in radians, per difficulty |
| `thinkSeconds` | `1.0` | Pause before each shot |
| `UseFriendAvatar` | `true` | Borrow a random friend's name and appearance |
| `FallbackUsername` | a specific account | Used when no friend is available |

## How it plays

The AI finds a shot at a legal ball, then aims at it with a small random error added. The
error is what separates the difficulties.

| Difficulty | Aim drift | Feels like |
|---|---|---|
| `easy` | 0.09 radians | Misses regularly, punishes only easy positions |
| `medium` | 0.045 radians | Takes the shots you would take, misses the thin cuts |
| `hard` | 0.015 radians | Pots almost anything with a clear line |

Radians are the honest unit here: 0.09 is about five degrees, which over the length of a
table is a comfortable miss. 0.015 is under a degree.

It is a shot-picker, not a position player. It does not plan the next shot or play safeties,
so a player who thinks a move ahead beats `hard` regularly. That is the right level for a
pool table in a lobby.

<Callout type="tip" title="Difficulty is one global setting, not a per-match choice">

Unlike the chess and tic tac toe systems, there is no difficulty button on the panel: every
AI match uses `Config.ai.difficulty`.

If you want players to choose, that is a small change: `startMode` already receives the
mode, and the aim error is read per match at start.

</Callout>

## The pause

```lua
thinkSeconds = 1.0,
```

The AI waits a second before shooting, even though it decided instantly. Without it the
reply lands while the player's own shot is still settling in their head.

## The avatar

An NPC stands at the other seat. With `UseFriendAvatar = true` it borrows the name and
appearance of a random friend of the player, otherwise `FallbackUsername`.

That is a well-judged touch: playing "against" somebody you know is more engaging than
playing against a nameless bot. Change `FallbackUsername` to an account you control.

## AI matches are unrated

An AI match never touches ELO, wins, losses, draws or the leaderboards. Neither does solo
practice.

This is stricter than the shipped defaults on the other game systems in the range, and it
is the right call for a game with an ELO ladder: an opponent whose difficulty the player
chooses cannot be allowed to move a rating.

Coin rewards do still apply, so an AI match is worth playing.
