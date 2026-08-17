---
title: Setup
order: 1
description: Install, shorten the cooldown, write one reward, and watch a claim happen.
icon: play
---

Installation is the standard four steps in [Installing a system](../platform/installation.md).
Drag `uxrDailyRewardsSystem` into **ServerScriptService** and press Play once.

This page is the first useful thing to do afterwards.

## 1. Make the cooldown short

A 24 hour cooldown is a bad way to find out your reward code has a typo. In
`Shared/Config/Settings.luau`:

```lua
DailyRewards = {
    ClaimIntervalSeconds = 30,
},
```

Thirty seconds between claims. Put it back to `86400` before you publish.

## 2. Write a real reward

Open `Server/DailyRewards/Rewards.luau`. Day 1 ships with an empty `Grant`:

```lua
[1] = {
    Icon = "rbxassetid://0",
    ValueText = "500",
    Grant = function(player)
        local stats = player:FindFirstChild("leaderstats")
        local coins = stats and stats:FindFirstChild("Coins")
        if coins then
            coins.Value += 500
        end
    end,
},
```

That file is server-only, so anything your game can do on the server, `Grant` can do.

## 3. Press Play

The panel opens on its own, because day 1 is claimable for a player who has never
claimed. Click the day 1 card. The reward runs, the card goes to the claimed style, and
day 2 starts counting down.

Wait out your thirty seconds and claim again to see the streak advance.

## 4. Let a streak break

Claim, then wait `ClaimIntervalSeconds * 2` without claiming. The next claim is day 1
again, and `Hooks.onStreakReset` fires. That is the whole miss behaviour, and it is worth
seeing once before you decide on the real numbers in [Streaks and cooldowns](streaks.md).

## 5. Turn saving on

Progress is stored in a `DataStore`. In Studio that needs **Game Settings, Security,
Enable Studio Access to API Services**. Without it every Play starts from day 1 and
Output warns about the failed load.

<Callout type="tip" title="Change the store name to wipe test data">

`Profile.Store` in the settings is the save slot. Changing it, for example from
`uxrDR_Profile_v1` to `uxrDR_Profile_v2`, gives every player a fresh profile. It is the
cleanest way to throw away the streaks you built up while testing with a 30 second
cooldown.

</Callout>

## 6. Set the real numbers

| Setting | Test value | Live value |
|---|---|---|
| `DailyRewards.ClaimIntervalSeconds` | `30` | `86400` |
| `Profile.Store` | `uxrDR_Profile_test` | `uxrDR_Profile_v1` |
| `Skip.DevProductId` | `0` | Your product id |

Publish. Settings live in the file, so publishing the place ships them.
