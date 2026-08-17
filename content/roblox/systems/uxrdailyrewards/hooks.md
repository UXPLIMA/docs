---
title: Hooks and API
order: 7
description: The three server callbacks, the service functions, and the RPC types.
icon: webhook
---

## Hooks

`Server/Hooks.luau` ships with three empty functions. Fill in the ones you want, leave the
rest alone. Each is called inside a `pcall`, so an error in your code is logged and never
blocks a claim.

```lua
function Hooks.onClaim(player, day, reward) end
function Hooks.onStreakReset(player) end
function Hooks.onSkipPurchase(player) end
```

| Hook | Fires | Arguments |
|---|---|---|
| `onClaim` | Right after a successful claim, before the save | The player, the 1-based day, that day's config table |
| `onStreakReset` | When the claim in progress starts a fresh cycle after a miss | The player |
| `onSkipPurchase` | Right after the skip product is applied | The player |

`onStreakReset` fires **before** the `Grant` for the new day 1, so the two arrive in the
order a player experiences them: the streak is gone, then the reward is given.

A typical use:

```lua
function Hooks.onClaim(player, day, reward)
    Analytics.log(player, "daily_claim", { day = day })
    if day == 7 then
        Announce.server(player.Name .. " completed the weekly ladder")
    end
end
```

Hooks are for side effects. The reward itself belongs in `Grant`
(see [The reward ladder](rewards.md)), because that is the code that runs even when the
player claims through some other path.

## RewardService

`Server/DailyRewards/RewardService.luau`. Require it from your own server code when you
need to drive the system directly.

| Function | Returns | What it does |
|---|---|---|
| `buildState(player)` | Snapshot table, or `nil` | The same state the panel is drawn from |
| `push(player)` | none | Sends a fresh snapshot to that player's panel |
| `claim(player)` | `{ ok = true, day = n }` or a refusal | Runs the whole claim flow, server-authoritative |
| `handleReceipt(receiptInfo)` | A decision, or `nil` | Routes a Developer Product receipt |
| `init()` | none | Called by the bootstrap. You do not call this |

`claim` refusals:

| `reason` | Meaning |
|---|---|
| `busy` | A claim from this player is already running |
| `notloaded` | Their profile has not finished loading, or failed to |
| `cooldown` | Not claimable yet. Carries `secondsUntilNext` |
| `error` | Something threw. Details are in Output |

Granting a reward from your own code, for example from a quest completion, is a
`claim(player)` call. It goes through the same cooldown and streak rules as the button,
which is usually what you want. If you want to hand over a reward without touching the
ladder, call your own code, not this.

## The snapshot

What `buildState` returns and the client renders:

```lua
{
    currentDay = 3,
    claimable = false,
    secondsUntilNext = 41233,
    totalDays = 7,
    rewards = { { icon = "rbxassetid://…", valueText = "500" }, … },
    autoOpen = false,
    skip = { enabled = true, priceText = "99 R$", devProductId = 1234567890 },
}
```

Note what is not in it: no `Grant`, no player balance, nothing about days beyond their
icon and label. The client is told enough to draw the ladder and nothing that would help
somebody fake a claim.

## RPC types

Over the shared remotes at `ReplicatedStorage/uxrDailyRewardsSystem/Core/drEvents`.
See [The RPC layer](../platform/rpc.md).

| Type | Direction | Payload |
|---|---|---|
| `GetBootstrap` | Client invokes | Balance and the currency symbol |
| `DR_GetState` | Client invokes | The snapshot above |
| `DR_Claim` | Client invokes | The claim result |
| `DR_State` | Server fires | A snapshot, pushed after any change |

Every state change on the server ends with a `DR_State` push, so the panel never polls.
The client's one-second countdown is local; when it hits zero it re-invokes `DR_GetState`
rather than assuming.

## Testing the streak logic

`Shared/DailyRewards/Streak.luau` takes its time and its configuration as arguments and
calls no Roblox API, so it can be run directly. `Shared/DailyRewards/_spec/Run.luau` is
the shipped set of cases.

If you change the miss or loop behaviour, change it there and run that spec. It is the
only part of the system whose behaviour is worth asserting rather than clicking through
with a 30 second cooldown.
