---
title: Hooks and the API
order: 6
description: Reacting to trolls, and firing them from your own code.
icon: code
---

Two ways to connect this system to the rest of your game: a hook that tells you what
happened, and an API that lets you make things happen.

## The hook

`Configuration/Server/Hooks.luau` ships empty. Add a function and it is called.

```lua
local Hooks = {}

function Hooks.OnTrollApplied(by: Player, key: string, scope: string, victims: { Player })
    print(by.Name .. " used " .. key .. " on " .. #victims .. " players")
end

return Hooks
```

| Argument | |
|---|---|
| `by` | Who paid |
| `key` | `Kill`, `Slow`, `Freeze`, `Fire`, `Fling`, `Explode` or `Kick` |
| `scope` | `"One"` or `"All"` |
| `victims` | Who it actually reached |

<Callout type="info" title="victims is who it landed on, not who was aimed at">

An everyone troll fired at a server of ten with three immune players gives you seven
victims. A single-target troll on somebody without a character gives you none, and the hook
does not fire at all.

That is the number to log, not the number you might infer from the scope.

</Callout>

The hook fires once per troll, after it has landed on at least one player.

## Hook rules

| | |
|---|---|
| It runs inside `pcall` | A mistake in your code prints a warning, it does not stall the system |
| Its return value is ignored | A hook observes; it cannot block or change a troll |
| It is server side | The file lives in `Configuration/Server` and is never replicated |

## Things to build on it

```lua
function Hooks.OnTrollApplied(by, key, scope, victims)
    myLeaderboard:Add(by, "TrollsBought", 1)

    for _, victim in victims do
        myStats:Add(victim, "TimesTrolled", 1)
    end

    if scope == "All" then
        myWebhook:Post(by.Name .. " used " .. key .. " on the whole server")
    end
end
```

A server-wide troll counter, a "most trolled player" board, a log of who spent what, an
achievement for surviving an explosion. All of it belongs here rather than inside the
system.

## The API

The system registers named actions your own server code can call.

```lua
local API = require(game.ServerScriptService.uxrTrollSystem.Core.Server.API)

API.OnReady(function()
    API.Call("ApplyTroll", "Freeze", "All", nil, nil)
end)
```

| Action | Arguments | Does |
|---|---|---|
| `ApplyTroll` | `key, scope, target, by` | Fires a troll with no purchase |
| `GrantCredit` | `player, key, scope` | Gives a player a free go |

`ApplyTroll` returns `true` if it landed on anybody.

| Argument | For `scope = "One"` | For `scope = "All"` |
|---|---|---|
| `target` | The victim | Ignored |
| `by` | Who to credit in chat and the hook. Optional | Required |

Passing `by` as `nil` on a single-target troll applies the effect silently: no chat line and
no hook.

<Callout type="warning" title="ApplyTroll bypasses everything except immunity">

No purchase, no cooldown, no rate limit. Immunity is still respected, because it is checked
inside the effect path.

That makes it right for an event script, a boss mechanic or a staff command, and wrong for
anything a player can trigger directly.

</Callout>

`GrantCredit` is the compensation tool: a player whose purchase went wrong, or a reward from
a quest of yours.

```lua
API.Call("GrantCredit", player, "Explode", "One")
```

Their next Explode button reads `FREE (1)` and costs nothing.

## Waiting for readiness

```lua
API.IsReady()
API.OnReady(callback)
API.Actions()
```

Services start in order, and calling an action before the system is ready raises an error
telling you to wrap the call in `OnReady`.

`OnReady` runs your callback immediately if the system is already up, so it is always safe
to use.

`API.Actions()` lists the registered action names, which is a quick way to confirm the
system started as expected.

## Uses worth building

| Idea | How |
|---|---|
| A boss that freezes the arena | `API.Call("ApplyTroll", "Freeze", "All", nil, bossOwner)` |
| A staff command | `API.Call("ApplyTroll", "Kick", "One", target, admin)` |
| A round-end fireworks moment | `ApplyTroll` with `Fling` on everyone |
| Compensating a failed purchase | `GrantCredit` |
| A daily free troll | `GrantCredit` on join, once per day |

The last one is worth considering as a retention mechanic: a free go each day gets players
into the panel, and the panel is where the paid buttons are.
