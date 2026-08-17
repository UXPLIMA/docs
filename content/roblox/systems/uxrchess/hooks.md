---
title: Hooks
order: 7
description: The four end-of-game callbacks, what they carry, and when they fire.
icon: webhook
---

`Server/Hooks.luau` ships with all four functions commented out. Uncomment the ones you
want. Each runs on the server inside a `pcall`, so an error in your code cannot break a
game.

```lua
local Hooks = {}

function Hooks.onWin(player, ctx) end
function Hooks.onLoss(player, ctx) end
function Hooks.onDraw(player, ctx) end
function Hooks.onGameEnd(result) end

return Hooks
```

Do not rename them. They are looked up by name.

## The per-player hooks

`onWin`, `onLoss` and `onDraw` are called once for each human, from that player's point of
view.

```lua
ctx = {
    color = "w",         -- "w" or "b"
    elo = 1240,          -- this player's rating
    opponent = Player,   -- nil against the bot
    opponentElo = 1000,
    vsBot = false,
    reason = "checkmate",
}
```

`reason` is one of `checkmate`, `resign`, `timeout`, `stalemate`, or a `draw_` value for
the other drawn endings.

```lua
function Hooks.onWin(player, ctx)
    if not ctx.vsBot and ctx.reason == "checkmate" then
        MyEconomy.addCoins(player, 100)
    end
end
```

Checking `vsBot` matters. Without it a player can beat the 600 bot in a loop for as long
as they like.

## onGameEnd

Once per game, whoever played.

```lua
result = {
    winner = "w",        -- "w", "b", or nil for a draw
    reason = "timeout",
    vsBot = true,
    white = { player = Player, elo = 1240, isBot = false },
    black = { player = nil,    elo = 1000, isBot = true },
}
```

This is the one that sees the whole game, so it is the right place for logging,
announcements and analytics.

```lua
function Hooks.onGameEnd(result)
    if not result.vsBot then
        Analytics.log("chess_pvp", { winner = result.winner, reason = result.reason })
    end
end
```

Note that the ratings in `white.elo` and `black.elo` are the ones the game was played at,
which is what you want for a match record.

## Ordering

1. `leaderstats` wins, losses and draws are updated, subject to `CountBotWinLoss`
2. ratings are updated, subject to `RatedVsBot`
3. the configured rewards are paid, subject to `PayBotGames`
4. `onWin` / `onLoss` / `onDraw` fire, once per human
5. `onGameEnd` fires

Reading `leaderstats` inside a hook therefore gives you the post-game numbers.

## Every ending fires the hooks

Checkmate, stalemate, an accepted draw, a resignation, a flag fall and a player leaving
mid-game all end the game properly and all fire the hooks with a `reason` naming what
happened.

A player who leaves rather than resigns is treated as a resignation. Do not build a
scoring rule that ignores resignations: it becomes the meta within a day.
