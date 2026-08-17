---
title: The scripting API
order: 6
description: LevelAPI, the result table, the three signals, and the player value objects.
icon: code
---

`Server/LevelAPI.luau` is the supported way for your own server code to read and change
XP.

```lua
local LevelAPI = require(
    game.ServerScriptService.uxrLevelSystem.Server.LevelAPI
)

LevelAPI:AddXPToCurrentTeam(player, 250)
```

## The two shapes of every write

Every write comes in two forms. The plain one returns a boolean; the `Result` one returns
the full table.

```lua
local didIt = LevelAPI:AddXP(player, "Police", 250)

local result = LevelAPI:AddXPResult(player, "Police", 250)
-- result.ok, result.currentLevel, result.levelUp, …
```

Use the boolean when you only care that it worked. Use the result when you want to react
to the rank change it caused.

## Writes

| Function | What it does |
|---|---|
| `AddXP(player, team, amount, opts)` | Adds XP on a named team |
| `RemoveXP(player, team, amount, opts)` | Subtracts, floored at zero |
| `SetXP(player, team, amount, opts)` | Writes an exact value |
| `AddXPToCurrentTeam(player, amount, opts)` | The same, on whichever team they are on |
| `RemoveXPFromCurrentTeam(player, amount, opts)` | |
| `SetCurrentTeamXP(player, amount, opts)` | |
| `SetPlaytime(player, seconds, opts)` | Overwrites total playtime |
| `SavePlayerData(player)` | Forces a profile save |

Each has a `Result` twin: `AddXPResult`, `SetCurrentTeamXPResult`, and so on.

Amounts are floored and clamped at zero. A negative amount is not a subtraction; use
`RemoveXP`.

## Options

```lua
LevelAPI:AddXP(player, "Police", 250, { notify = false, save = true })
```

| Option | Default | Effect |
|---|---|---|
| `notify` | `true` | Whether the player sees the XP and level-up frames |
| `save` | `false` | Force an immediate profile save instead of waiting for autosave |

`notify = false` is for XP the player should not be told about individually, for example
a background trickle or a batch correction. `save = true` is for XP that must not be lost
if the server dies in the next five minutes, for example a purchase.

## The result table

| Field | What it is |
|---|---|
| `ok` | Whether the write happened |
| `reason` | Why not: `InvalidPlayer`, `InvalidTeam`, `InvalidMode`, `MissingXPValue`, `NoTeam` |
| `mode` | `"add"`, `"remove"` or `"set"` |
| `teamName`, `amount` | What was asked for |
| `beforeXP`, `currentXP`, `xpChange` | XP before, after, and the delta actually applied |
| `beforeLevel`, `beforeLevelName` | The rank before |
| `currentLevel`, `currentLevelName` | The rank after |
| `nextLevelName`, `nextLevelXP` | What they are working towards |
| `levelChanged`, `levelUp` | Whether the rank moved, and whether it moved up |
| `info` | The full info table below |

`xpChange` is not always `amount`. Removing 500 XP from a player who has 200 changes it by
200, because the floor is zero.

## Reads

| Function | Returns |
|---|---|
| `GetRawXP(player, team)` | The stored number |
| `GetLevel(player, team)` | The 1-based rank index |
| `GetLevelName(player, team)` | The rank name |
| `GetNextLevelName(player, team)` | The next rank's name |
| `GetNextLevelXP(player, team)` | Total XP needed for it, `-1` at the top |
| `GetRemainingXP(player, team)` | XP still to go, `0` at the top |
| `GetLevelProgress(player, team)` | Percentage through the current rank, 0 to 100 |
| `GetLevelInfo(player, team)` | All of the above in one table |
| `GetRankText(player, team)` | A formatted `Level 3 - Sergeant` string |
| `GetPlaytime(player)` | Total seconds |
| `GetFormattedPlaytime(player)` | The same, as a readable string |
| `GetCurrentTeamName(player)` | Their team, or `nil` |
| `GetCurrentTeamInfo(player)` | `GetLevelInfo` for their current team |
| `GetCurrentLevel(player)`, `GetCurrentLevelName(player)` | Shorthands for the same |
| `GetAllXP(player)` | Every team's XP for that player |
| `GetTeamLevels(team)` | That team's ladder, as configured |
| `GetAllTeams()` | Every team name with a ladder |
| `IsValidTeam(team)` | Whether a ladder exists for it |
| `GetCatalog()` | The whole `Levels` config plus the team list |
| `GetLeaderboard(team)` | Online players on that team, sorted by XP |

<Callout type="note" title="GetLeaderboard is online players only">

It walks the currently connected players and sorts them. It is a scoreboard for the
running server, not an all-time leaderboard: there is no query across saved profiles.

For a global board, record XP into an `OrderedDataStore` of your own from the
`XPChanged` signal.

</Callout>

## Signals

```lua
LevelAPI.XPChanged:Connect(function(player, result)
    if result.levelUp then
        Announce.server(player.Name .. " reached " .. result.currentLevelName)
    end
end)
```

| Signal | Fires | Carries |
|---|---|---|
| `XPChanged` | Every XP write, including timed grants | The result table |
| `LevelChanged` | Only when the rank moved, up or down | The result table |
| `PlaytimeChanged` | When playtime is updated | The player and the new value |

`LevelChanged` fires after the tool sync, so by the time your handler runs the player is
already holding the new rank's tools.

## The player value objects

The system also mirrors its state onto the `Player` instance, which the client reads
directly:

```
Player
  LevelFolder
    LevelXPFolder
      Police          IntValue
      Solider         IntValue
    PlaytimeValue     IntValue
```

One `IntValue` per team with a ladder, rebuilt on join from the saved profile.

They are readable from client code, which is how the interface updates without a remote
call. Do not write to them: the server treats its own values as the source of truth and
an unmatched client-side change is meaningless anyway.

From server code, write through `LevelAPI` rather than setting these values. Setting the
value directly skips the rank evaluation, the tool sync, the notification and the save.
