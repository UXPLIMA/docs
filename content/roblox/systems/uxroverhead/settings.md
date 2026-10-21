---
title: Settings
order: 7
description: The general toggles, the caches, and the uxrLevel integration.
icon: sliders-horizontal
---

`Shared/Config/Settings.luau` is small. Most of this system's configuration is in the other
three files.

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrOH]` lines in Output |
| `HideRobloxDisplayTag` | `true` | Hide the default Roblox name and health display |
| `AFKModule` | `true` | Window-focus AFK detection |
| `AFKBodyTransparency` | `true` | AFK players turn transparent |
| `TeamUnrankedTitle` | `"Unranked"` | Shown when a player has no team |
| `RankUnrankedTitle` | `"Unranked"` | Shown when no rank matches |
| `GroupCacheTTL` | `300` | Seconds to cache group, rank and friendship checks. `0` disables |
| `GamepassCacheTTL` | `60` | Seconds to cache gamepass checks. `0` disables |
| `LevelSystemName` | See below | The uxrLevel package name in `ReplicatedStorage` |

## The four configuration files

| File | Holds |
|---|---|
| `Config/Layout.luau` | The rows, their order and their text |
| `Config/Ranks.luau` | Rank candidates, their conditions and colours |
| `Config/Nametags.luau` | Device icons, country flags, the chat icon, custom badges |
| `Config/Settings.luau` | This page |

## The uxrLevel integration

```lua
LevelSystemName = "uxrLevelSystem#RS@4.3",
```

The name of the level system's folder in `ReplicatedStorage`. When a folder of that name is
present, five things become available:

| Available | Used by |
|---|---|
| `{Level}` | Layout text |
| `{LevelName}` | Layout text |
| `Level`, `Rank` checks | Ranks and tags |
| `LevelName`, `RankName` checks | Ranks and tags |
| `Playtime` check | Ranks and tags |

Without it, those placeholders resolve to empty strings and those checks always fail. The
system runs normally otherwise: this is an optional integration, not a dependency.

<Callout type="warning" title="The shipped LevelSystemName is a versioned name that may not match your install">

The default is `"uxrLevelSystem#RS@4.3"`, which carries a version suffix. The uxrLevel
system documented in this section replicates itself as plain `uxrLevelSystem`.

If your level placeholders are empty and your level checks never match, this is the first
thing to check. Set it to the name that actually appears under `ReplicatedStorage` when
both systems are running.

</Callout>

See [uxrLevel](../uxrlevel/) for that system.

## The unranked titles

```lua
TeamUnrankedTitle = "Unranked",
RankUnrankedTitle = "Unranked",
```

Two separate strings for two separate cases: no team, and no matching rank. They ship the
same and are worth differentiating: `"No Team"` and `"Civilian"` say more than `"Unranked"`
twice.

## Cache tuning

| Setting | Lower means | Higher means |
|---|---|---|
| `GroupCacheTTL` | Promotions show sooner | Fewer web calls |
| `GamepassCacheTTL` | Purchases show sooner | Fewer web calls |

`0` disables caching, which puts a Roblox web call in the path of every check of that kind.
On a full server with several group-conditioned tags, that is a lot of calls and a
noticeable delay.

The shipped values, five minutes and one minute, are sensible for almost every game. See
[Conditions](conditions.md).

## Debug

Turn it on while building ranks and tags: it logs what matched and what did not, which is
the fastest way to find out that a condition names the wrong group.

Turn it off before release. Nametag rebuilds happen for every player on every respawn.
