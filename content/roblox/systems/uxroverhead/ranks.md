---
title: Ranks
order: 3
description: Matching a player to a title and its colours, and how priority resolves ties.
icon: award
---

A rank gives a player a title and three colours. `Shared/Config/Ranks.luau` is a list of
candidates, each with a condition.

```lua
return {
    {
        Priority = 5,
        RankName = "Recruit",
        RankColor = "00b4d8",
        NameColor = "ffffff",
        TeamColor = "0353a4",
        CheckTypes = { Team = { Name = "Police" }, GroupRank = { ID = 33106532, Rank = 1 } },
    },
    {
        Priority = 100,
        RankName = "Developer",
        RankColor = "00b4d8",
        NameColor = "ffffff",
        TeamColor = "00b4d8",
        CheckTypes = { PlayerId = { IDs = { 3057647029 } } },
    },
}
```

| Field | What it is |
|---|---|
| `Priority` | Higher wins when several match |
| `RankName` | The title, available as `{RankName}` |
| `RankColor` | Hex, no `#`. Available as `{RankColor}` |
| `NameColor` | The display name's colour |
| `TeamColor` | The team row's colour |
| `CheckTypes` | The condition. See [Conditions](conditions.md) |

## One rank wins

Every entry is evaluated and the **highest priority** match is used. A player who is both a
Police Officer and a developer shows as a developer, because 100 beats 6.

Give staff ranks high numbers and gameplay ranks low ones, leaving gaps: `5, 6, 7` for
in-game progression and `100, 200` for staff means you can insert a rank later without
renumbering.

## Colours override the team

`TeamColor` in a rank overrides the Roblox team colour for the team row. That lets two
teams share a colour scheme, or one team show a different colour at a senior rank.

A player who matches no rank falls back to:

| Field | Fallback |
|---|---|
| `RankName` | `Settings.RankUnrankedTitle`, `"Unranked"` |
| `RankColor` | Grey |
| `NameColor` | White |
| `TeamColor` | Their actual Roblox team colour, or white |

So an unranked player still gets a correct, readable nametag. That matters: most players on
most servers match nothing.

## Hex without the hash

```lua
RankColor = "00b4d8",
```

No leading `#`. That is the convention throughout this system's config files, and it
matches the uxrLevel system's `LevelColor`.

## The shipped list is an example

Five entries covering two teams and a developer id, matching the place this was built for.
The group id and the user id in it are not yours.

<Callout type="danger" title="Replace the shipped group and user ids before publishing">

`33106532` and `3057647029` are real ids belonging to somebody else. Leaving them in means
your ranks match nobody, or worse, match a stranger who happens to be in that group.

Every `CheckTypes` block in `Ranks.luau` and `Nametags.luau` needs reviewing before launch.

</Callout>

## Ranks and the level system

With the uxrLevel system present, a rank can be driven by level rather than by group:

```lua
{
    Priority = 10,
    RankName = "Veteran",
    RankColor = "f7c516",
    NameColor = "ffffff",
    TeamColor = "0353a4",
    CheckTypes = { Level = { Min = 10 } },
},
```

That gives you a nametag that changes as a player progresses, which is the natural pairing
of the two systems. See [Settings](settings.md) for how the integration is wired.

## Ranks are re-evaluated

The nametag is rebuilt when a player's character loads and when the things it depends on
change, subject to the caches described in [Conditions](conditions.md).

A player who switches team sees their rank update. A player who is promoted in a group sees
it within the group cache window.
