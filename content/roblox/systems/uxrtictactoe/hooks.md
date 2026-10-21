---
title: Hooks
order: 7
description: The four end-of-game callbacks and what each one is handed.
icon: webhook
---

`Server/Hooks.luau` ships empty. Add any of the four functions below and it is called;
leave it out and nothing happens. Each runs inside a `pcall`, so an error in your code
cannot break a game.

```lua
local Hooks = {}

function Hooks.onWin(player, ctx) end
function Hooks.onLoss(player, ctx) end
function Hooks.onDraw(player, ctx) end
function Hooks.onGameEnd(info) end

return Hooks
```

## The per-player hooks

`onWin`, `onLoss` and `onDraw` are called once for each human in the game, from that
player's point of view. A human beating a human calls `onWin` for one and `onLoss` for the
other.

```lua
ctx = {
    mark = "X",          -- the side this player played
    opponent = Player,   -- the other player, or nil against the bot
    vsBot = false,
    reason = "line",     -- how the game ended
}
```

This is where your own economy goes:

```lua
function Hooks.onWin(player, ctx)
    if not ctx.vsBot then
        MyEconomy.addCoins(player, 100)
    end
end
```

Checking `ctx.vsBot` matters. Without it a player can sit alone at a table and beat Easy
for as long as they like.

## onGameEnd

Called once per game, whoever was playing.

```lua
info = {
    winner = "X",        -- "X", "O", or nil for a draw
    reason = "line",
    vsBot = true,
    x = { player = Player, isBot = false },
    o = { player = nil,    isBot = true },
}
```

Use it for logging, announcements and analytics: it is the one call that sees the whole
game rather than one side of it.

```lua
function Hooks.onGameEnd(info)
    if not info.vsBot then
        Analytics.log("ttt_pvp", { winner = info.winner, reason = info.reason })
    end
end
```

## Ordering

For a finished game the order is:

1. `leaderstats` are updated, subject to `CountBotGames`
2. the configured rewards are paid, subject to `PayBotGames`
3. `onWin` / `onLoss` / `onDraw` fire, once per human
4. `onGameEnd` fires

So by the time your hooks run, the built-in counters already reflect the result. Reading
them inside a hook gives the new numbers, not the old ones.

## A resignation is a result

Standing up mid-game, leaving the server, or pressing resign ends the game with the other
side as the winner and a `reason` naming it. The hooks fire normally.

Do not treat a resignation as a non-event in your own scoring: the players will find it
within an hour if you do.
