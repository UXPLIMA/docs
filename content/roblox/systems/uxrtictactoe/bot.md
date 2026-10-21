---
title: The bot
order: 4
description: The three difficulties, what the search actually does, and why it never lags.
icon: cpu
---

A player alone at a table can start a game against the bot. It plays as the free side, on
the chosen board size, at the chosen difficulty.

## The three difficulties

```lua
Difficulties = {
    Easy = { depth = 1, randomness = 0.6 },
    Medium = { depth = 3, randomness = 0.15 },
    Impossible = { depth = 99, randomness = 0 },
},
```

| Field | What it means |
|---|---|
| `depth` | How many moves ahead the search looks. Higher is stronger and slower |
| `randomness` | Chance from 0 to 1 that it throws the search away and plays at random |

`randomness` is what makes an easy bot feel human rather than merely weak. A pure
depth-1 search still takes an obvious win and blocks an obvious threat, every time. Mixing
in random moves is what lets a beginner win.

You can add difficulties. Every entry becomes a button on the seat panel, so
`Hard = { depth = 5, randomness = 0.05 }` needs no other change.

## Impossible on 3x3

At depth 99 on a 3x3 board the search reaches the end of the game on every move, so the
bot plays perfectly. Tic tac toe is a draw under perfect play: the bot never loses, and
wins only against a mistake.

That is worth telling players. An unbeatable opponent presented as a challenge is
frustrating; presented as a puzzle, "can you force the draw", it is not.

On 4x4 and 5x5, depth 99 is not reachable. The search is bounded by
`BotMaxThinkSeconds` and falls back on a heuristic evaluation of the position, so
Impossible there is strong but not solved.

## It runs on a separate thread

The search runs in a parallel Actor rather than on the main server thread. A slow search
therefore does not cost anybody frames, on a big board or with several tables thinking at
once.

Three things bound it:

| Bound | Value | Effect |
|---|---|---|
| Depth | `depth` per difficulty | How far the tree goes |
| Time | `BotMaxThinkSeconds`, `1.0` | The search stops and returns its best so far |
| Watchdog | 10 seconds | The worker is given up on entirely |

If the watchdog fires, or the search errors, the bot plays a random legal move rather than
stalling the game. A game never hangs waiting for the bot.

## The move delay

```lua
BotMoveDelay = 0.6,
```

The bot waits this long before playing even when it decided instantly. Without it, an
Impossible bot on 3x3 answers before the player's mark has finished animating, which
reads as broken rather than as fast.

## The bot's avatar

A cosmetic NPC sits in the opposite chair. With `Npc.UseFriendAvatar = true` it wears the
appearance of a random friend of the player, which is a nice touch on a busy game and a
non-event for a player with no friends online, in which case
`Npc.FallbackUsername` is used.

The game waits up to `BotLoadMaxSeconds` for the avatar and then starts anyway. The
appearance is decoration; the opponent is the search.

## Do bot games count

Two settings decide, and they are independent:

| Setting | Controls |
|---|---|
| `Leaderstats.CountBotGames` | Whether wins, losses and draws move |
| `Rewards.PayBotGames` | Whether currency is paid |

Both shipped answers are cautious in the right direction: stats count, money does not.
Reverse `CountBotGames` if you run public leaderboards, because Easy is farmable.

## Testing the engine

`Server/TTT/_spec/BotSpec.luau` and `Shared/TTT/_spec/` hold the shipped cases for the
rules, the engine and the bot. Run them from Studio:

```lua
require(game.ReplicatedStorage.uxrTicTacToeSystem.TTT._spec.Run)()
```

The rules and engine modules use no Roblox API, which is why they can be asserted rather
than clicked through.
