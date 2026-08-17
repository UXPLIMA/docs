---
title: Hooks
order: 7
description: The three end-of-match callbacks and what they carry.
icon: webhook
---

`Server/Hooks.luau` ships with three functions commented out. Uncomment the ones you want.
Each runs on the server, once per human player, inside a `pcall`.

```lua
local Hooks = {}

function Hooks.onWin(player, ctx) end
function Hooks.onLoss(player, ctx) end
function Hooks.onMatchEnd(player, ctx) end

return Hooks
```

Do not rename them. They are looked up by name.

## The context

All three receive the same shape:

```lua
ctx = {
    won = true,
    elo = 1240,          -- this player's rating after the match
    opponent = Player,   -- nil against the AI
    opponentElo = 1400,
    vsBot = false,
    mode = "1v1",        -- or "1vAI"
}
```

`elo` is the **post-match** rating, which is what you want for an announcement: the number
the player is about to see in their player list.

## onMatchEnd fires for everybody

`onWin` and `onLoss` are the two halves; `onMatchEnd` fires for every human regardless, with
`ctx.won` telling you which it was.

Use `onMatchEnd` when the logic is the same either way:

```lua
function Hooks.onMatchEnd(player, ctx)
    Analytics.log("darts_match", {
        won = ctx.won,
        mode = ctx.mode,
        elo = ctx.elo,
    })
end
```

Use `onWin` when it is not:

```lua
function Hooks.onWin(player, ctx)
    if not ctx.vsBot then
        MyEconomy.addCoins(player, 100)
    end
end
```

## Check vsBot before paying

A player can start an AI match at `Noob` and win it repeatedly. Any reward that does not
check `ctx.vsBot` is a coin printer.

The system's own rating does count AI matches, weighted by the tier's rating, which is a
different trade: it is farmable but slowly, and only up to the tier's own level.

## Ordering

1. Stats are recorded: wins, 180s, nine-darters and the accuracy counters
2. Ratings are updated
3. `onWin` or `onLoss` fires per human
4. `onMatchEnd` fires per human

So `ctx.elo` and anything you read from `leaderstats` inside a hook are already the new
values.

## A forfeit is a result

Leaving mid-match ends it with the other player as the winner, and the hooks fire normally
with that result.

Treat a forfeit as a loss in your own scoring. If you do not, quitting a losing match
becomes the correct play.
