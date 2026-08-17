---
title: Conditions
order: 5
description: The eighteen check types, how they combine, and the caches behind them.
icon: funnel
---

Ranks and icon tags are both matched by a `CheckTypes` table. Every entry in it must pass;
this is an AND, not an OR.

```lua
CheckTypes = {
    Team = { Name = "Police" },
    GroupRank = { ID = 33106532, Rank = 254 },
},
```

That matches a player who is on the Police team **and** holds rank 254 in that group.

For an OR, use one of the multi checks, or write two entries.

## The check types

### Group and rank

| Check | Data | Passes when |
|---|---|---|
| `Group` | `{ ID = n }` | The player is in the group |
| `GroupRank` | `{ ID = n, Rank = r }` | Their rank is exactly `r` |
| `GroupRank` | `{ ID = n, MinRank = a, MaxRank = b }` | Their rank is in the range and above 0 |
| `MultiGroup` | `{ IDs = { n, n } }` | They are in **any** of the groups |
| `MultiGroupRank` | `{ Groups = { {ID=…, Rank=…}, … } }` | **Any** of the group-rank checks passes |

`Rank` and `MinRank`/`MaxRank` are alternatives. `Rank` is exact; the range form is
inclusive and also requires a non-zero rank, so it never matches a non-member.

### Team

| Check | Data | Passes when |
|---|---|---|
| `Team` | `{ Name = "Police" }` | They are on that team |
| `Team` | `{ Names = { "Police", "Fire" } }` | They are on **any** of them |

A player with no team fails both.

### Identity

| Check | Data | Passes when |
|---|---|---|
| `PlayerId` | `{ IDs = { n, n } }` | Their user id is in the list |
| `Owner` | `true` | They own the experience |
| `Premium` | `true` | They have Roblox Premium |
| `Verified` | `true` | They have the verified badge |
| `AccountAge` | `{ Min = 365 }` | Their account is at least that many days old |
| `FriendWith` | `{ ID = n }` | They are friends with that user |

`AccountAge` is a genuinely useful anti-alt tag: a "new here" badge under 7 days, or a
veteran badge over 1000.

### Purchases

| Check | Data | Passes when |
|---|---|---|
| `Gamepass` | `{ ID = n }` | They own that gamepass |

### Level system

These need the uxrLevel system present. Without it they always fail.

| Check | Data | Passes when |
|---|---|---|
| `Level` | `{ Min = a, Max = b }` | Their level is in range. Either bound may be omitted |
| `LevelName` | `{ Names = { "Sergeant" } }` | Their level name is in the list |
| `Rank` | `{ Min = n }` | Their level is at least `n` |
| `RankName` | `{ Names = { … } }` | The same as `LevelName` |
| `Playtime` | `{ Min = seconds }` | Their total playtime is at least that |

`Rank` and `RankName` are aliases kept for readability: a "rank" reads better than a
"level" on a roleplay server. See [Settings](settings.md) for the integration.

## Caching

Group and gamepass checks call Roblox web endpoints, which are slow and rate limited.
Results are cached:

```lua
GroupCacheTTL = 300,
GamepassCacheTTL = 60,
```

| Setting | Default | Caches |
|---|---|---|
| `GroupCacheTTL` | `300` | Group membership, rank and friendship, for five minutes |
| `GamepassCacheTTL` | `60` | Gamepass ownership, for one minute |

`0` disables caching for that class of check.

<Callout type="warning" title="A player who buys a gamepass waits up to a minute for their tag">

The gamepass cache is what stops the system asking Roblox on every nametag refresh, and it
is also why a purchase does not show instantly.

One minute is a reasonable compromise. Lowering it to `0` makes the tag immediate and puts
a web call in the refresh path for every tagged player, which on a busy server is a lot of
calls.

</Callout>

Group membership changes are slower still at five minutes, which is fine: a promotion is
not something a player expects to see the same second.

## Order of evaluation

Checks run in table order and stop at the first failure. Putting a cheap local check first
avoids a web call:

```lua
CheckTypes = {
    Team = { Name = "Police" },          -- free
    Gamepass = { ID = 12345 },           -- a web call, only when on the team
},
```

That is worth doing on a tag that most players will not match.

## An unknown check fails

A `CheckTypes` entry naming a check that does not exist returns false, so the whole
condition fails. A typo therefore makes a tag never appear rather than always appear, which
is the safer direction.
