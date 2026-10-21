---
title: Levels and teams
order: 2
description: Building a rank ladder, the XP timer, and what happens at the top.
icon: list-ordered
---

`Shared/Config/Levels.luau` is the file you will spend your time in. It holds the timing,
the notification toggles, the player chat command, and one ladder per team.

## The XP timer

| Field | Default | What it does |
|---|---|---|
| `XPTime` | `120` | Seconds between automatic XP grants |
| `MaximumXPsKeepGaining` | `true` | Keep granting XP after the last rank |

One loop runs on the server. Every `XPTime` seconds it walks the online players and gives
each one the `XPGiveAmount` **of the rank they currently hold**, on the team they are
currently on.

That is the lever that makes early ranks fast and late ranks slow, or the reverse: the
amount is a property of the rank, not a global rate.

With `MaximumXPsKeepGaining = false`, a player at the last rank stops accruing entirely.
Set it that way if your XP number is a rank and nothing else; leave it `true` if anything
in your game reads raw XP as a score.

## A ladder

```lua
LevelTeams = {
    Police = {
        {
            LevelName = "Cadet",
            LevelColor = "00b4d8",
            XPRequirement = 0,
            XPGiveAmount = 75,
            LevelTools = { "Level 1 Tool" },
        },
        {
            LevelName = "Officer",
            LevelColor = "00849e",
            XPRequirement = 500,
            XPGiveAmount = 100,
            LevelTools = { "Level 1 Tool", "Level 2 Tool" },
        },
    },
},
```

| Field | What it is |
|---|---|
| `LevelName` | The rank name shown everywhere |
| `LevelColor` | A hex colour with no leading `#` |
| `XPRequirement` | **Total** XP needed to hold this rank |
| `XPGiveAmount` | XP granted every `XPTime` seconds while at this rank |
| `LevelTools` | Tool names given while at this rank |

## XPRequirement is cumulative

`XPRequirement` is the total the player must have reached, not the cost of that step.
Officer at `500` means 500 lifetime XP on that team, not 500 more than Cadet.

Write ladders in ascending order and start the first entry at `0`. A player whose XP is
below the first requirement still holds the first rank: rank one is the floor, not a gap.

There is no length limit. Four ranks ship; forty work the same way.

## The top of the ladder

A player at or above the last `XPRequirement` holds the last rank, their progress reads
100 percent, and there is no next rank to count towards.

<Callout type="note" title="The max-rank label is a literal in the code">

At the top of a ladder the next-rank name is reported as `Maksimum` and the interface
prints `(Maksimum)` beside the XP total. That string lives in
`Shared/Lib/LevelMath.luau` and in the notification module, not in a settings file, so
changing it means editing those two places.

</Callout>

## Notifications

```lua
ShowNotifications = {
    OnXPGain = true,
    OnLevelUp = true,
},
```

`OnXPGain` shows the small frame with the progress bar animating from the old XP to the
new. `OnLevelUp` shows the larger one naming the rank that was just left and the one just
reached.

Both are per-event and can be suppressed from your own code per call. See
[The scripting API](api.md).

## The player chat command

```lua
GuiCommand = {
    Enabled = true,
    Aliases = { "!xp", "!showxp" },
    Permission = { "Police", "Doctor" },
},
```

Any alias in the list opens the player's own XP frame. It does not open the staff panel.

`Permission` is a list of team names. The single entry `"all"` allows everybody:

```lua
Permission = { "all" },
```

The check is exact string equality against the player's current team, so a player with no
team is refused unless `"all"` is present.

## Per-team XP is stored per team

A player carries one XP number **per team that has a ladder**, all in the same profile.
Moving from Police to Soldier does not convert, transfer or reset anything: it changes
which number the interface is reading.

That is why the admin panel asks which team it is editing, and why almost every API
function takes a team name.
