---
title: Setup
order: 1
description: Match the team names, put the tools in, and watch a rank change happen.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrLevelSystem` into
**ServerScriptService** and press Play once.

Then three things have to line up before the system does anything visible.

## 1. Team names must match exactly

Every key under `LevelTeams` in `Shared/Config/Levels.luau` is a Roblox `Team.Name`.

```lua
LevelTeams = {
    Police = { … },
    Solider = { … },
},
```

If your `Teams` service has a team called `Police`, that ladder applies to it. If it does
not, that ladder is simply never used and no error is raised.

<Callout type="warning" title="The shipped file spells one team Solider">

The second shipped ladder is keyed `Solider`, matching the place it was built for. If
your team is spelled `Soldier`, rename the key. A mismatch is silent: the players on that
team gain no XP, get no tools, and see no rank.

</Callout>

A player on a team with no ladder is ignored by the system. That is the supported way to
have unranked teams.

## 2. Put the reward tools in place

`Storage/Tools` inside the package holds the `Tool` instances named by `LevelTools`. The
names must match exactly, spaces included. See [Tool rewards](tools.md).

If you are not giving tools, set `LevelTools = {}` on every level and skip this.

## 3. Speed the clock up to test

```lua
XPTime = 5,
```

Five seconds between XP grants. Press Play, join a team with a ladder, and watch the
level frame move. Put it back to something sane, `120` ships as the default, before you
publish.

## 4. Make yourself staff

In `Shared/Config/Admin.luau`, put your own user id in:

```lua
PlayerIdPermission = { 1234567 },
```

The file ships with two placeholder ids that belong to nobody. Replace them, do not add
to them. Then type `!xppanel` in chat to open the panel and correct your own XP.

## 5. Check saving

`Settings.StudioMode` ships as `"auto"`, which means **no saving in Studio** and saving in
the real game. That is the right default: it stops test sessions writing junk into live
profiles.

To confirm saving works, publish and join the real experience. In Studio, expect every
session to start from zero.

## 6. Set the real numbers before publishing

| Setting | File | Test value | Live value |
|---|---|---|---|
| `XPTime` | `Levels.luau` | `5` | `120` |
| `Debug` | `Settings.luau` | `true` | `false` |
| `DataStoreName` | `Settings.luau` | anything | `LevelData_v0` |
| `PlayerIdPermission` | `Admin.luau` | your id | your staff |
