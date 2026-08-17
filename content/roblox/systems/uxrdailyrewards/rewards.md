---
title: The reward ladder
order: 3
description: Rewards.luau, the Grant function, and how long a ladder can be.
icon: gift
---

`Server/DailyRewards/Rewards.luau` is the ladder. It is a numbered list, one entry per
day, and it is the file you are expected to rewrite.

```lua
local Rewards = {
    [1] = {
        Icon = "rbxassetid://0",
        ValueText = "500",
        Grant = function(player)
            -- your code
        end,
    },
    [2] = { … },
}

return Rewards
```

Seven days ship as a starting point. The number of entries **is** the ladder length:
there is no separate setting for it.

## The three fields

| Field | Type | What it is |
|---|---|---|
| `Icon` | Asset id string | The image shown in that day's card |
| `ValueText` | String | The label on that day's card. Cosmetic only |
| `Grant` | `function(player)` | The code that actually hands the reward over |

`ValueText` is not read by anything but the interface. Writing `"5,000"` does not give
five thousand of anything; `Grant` does that.

## Grant

`Grant` runs on the server, inside a `pcall`, with the claiming `Player` as its only
argument. Because this file never leaves the server, it can call anything your game
exposes server-side.

```lua
Grant = function(player)
    local stats = player:FindFirstChild("leaderstats")
    local coins = stats and stats:FindFirstChild("Coins")
    if coins then
        coins.Value += 500
    end
end,
```

Some common shapes:

```lua
-- a tool from ServerStorage
Grant = function(player)
    local tool = game:GetService("ServerStorage").Rewards.GoldSword:Clone()
    tool.Parent = player:FindFirstChildOfClass("Backpack")
end,

-- another module in your game
Grant = function(player)
    require(game.ServerScriptService.Economy).addGems(player, 25)
end,

-- a gamepass-style permanent unlock in your own profile store
Grant = function(player)
    MyData.set(player, "HasSpeedBoost", true)
end,
```

## When Grant errors

The claim still counts. `Grant` is called inside a `pcall`; a failure is written to Output
as `[uxrDR ERROR]` and the streak advances anyway.

That ordering is deliberate. The alternative, refusing the claim, means a player whose
reward code has a bug is stuck on the same day forever and loses their streak the next
day too. This way the failure is visible in the log and costs one reward rather than the
whole ladder.

<Callout type="warning" title="Check Output after changing Rewards.luau">

Because a broken `Grant` does not stop the claim, a player can appear to claim normally
and receive nothing. Test each day you write with a short `ClaimIntervalSeconds` and
watch Output for `[uxrDR ERROR]`.

</Callout>

## Changing the ladder length

Add or remove entries and keep the numbering contiguous from `1`. A gap, for example
jumping from `[3]` to `[5]`, leaves a day with no reward: the claim succeeds and grants
nothing.

Shortening a live ladder is safe. A player whose saved streak index is past the new end
is clamped back into range on their next read, so nobody is stranded.

## Icons

`Icon` accepts anything an `ImageLabel` does. The shipped `rbxassetid://0` renders as
blank, which is why a fresh install looks unfinished until you paste your own art in.

Icons are sent to the client so the panel can draw the whole ladder up front, including
days the player has not reached. Do not treat the icon or `ValueText` as a secret: the
reward code behind them is server-side, but its shop window is not.
