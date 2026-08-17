---
title: Access
order: 4
description: The seven factors, the presets, keycards, and the optional level check.
icon: key-round
---

Who may open a door is one field:

```lua
["1"] = { access = "Police", motion = { ... } },
```

Either the name of a preset in `Config/Permissions.luau`, or an inline table with the same
shape.

## The presets

```lua
return {
    Public = { noCheck = true },
    Police = { teams = { "Police" } },
    Owner = { playerIds = { 3057647029 } },
}
```

Three ship. Add your own: a preset is just a named access table, and naming the ones you
reuse is what keeps thirty doors readable.

Presets are resolved by name at check time, so editing `Police` changes every door and zone
that uses it.

## The seven factors

| Factor | Shape | Passes when |
|---|---|---|
| `noCheck` | `true` | Always. Skips every other check |
| `keycards` | `{ "Police Keycard" }` | The player holds a `Tool` with that exact name |
| `gamepasses` | `{ 12345678 }` | They own any one of the passes |
| `groups` | `{ [GROUP_ID] = { 5, 6, 7 } }` | Their rank in the group is one of the listed numbers |
| `teams` | `{ "Police", "Guard" }` | Their team name matches |
| `playerIds` | `{ 3057647029 }` | Their user id matches |
| `level` | See below | The uxrLevel check passes |

<Callout type="info" title="Factors combine as OR, never AND">

A player passes if they match **any** factor. There is no way to require two of them.

```lua
access = { teams = { "Police" }, keycards = { "Police Keycard" } },
```

That is a door for anybody on the Police team **or** anybody carrying the keycard, not
Police carrying a keycard.

To require a combination, encode it as one factor: a group rank that only combination
holders have, or a gamepass.

</Callout>

## Group ranks are exact

```lua
groups = { [33106532] = { 5, 6, 7 } },
```

The player's rank number must equal one of the listed numbers. There is no minimum-rank
form: rank 8 does not pass a door listing `{ 5, 6, 7 }`.

List every rank that should get through. It is more typing and it is unambiguous, which
matters when somebody is promoted into a rank nobody remembered to add.

## Keycards

```lua
keycards = { "Police Keycard", "Master Keycard" },
justCheckHand = false,
```

A keycard is a `Tool` found **by exact name**. Two places are searched:

| Location | Searched when |
|---|---|
| The character | Always. This is the equipped tool |
| The backpack | Only when `justCheckHand` is `false` |

`justCheckHand = true` on a door means the card must be held in hand. Use it for doors where
the act of presenting the card is part of the roleplay, and leave it off for doors that
should just work.

The name match is exact and case-sensitive. `Police Keycard` and `Police KeyCard` are
different cards.

## Gamepasses

```lua
gamepasses = { 12345678 },
```

Ownership is checked once per player per pass and cached for as long as they are in the
server. A player who buys the pass mid-session is not re-checked until they rejoin.

The check is wrapped so that a failed web call reads as "does not own" rather than
erroring. On a Roblox API outage, gamepass doors close.

## Teams

```lua
teams = { "Police", "Guard" },
```

Matched against the player's current `Team` name. A player with no team never matches.

<Callout type="warning" title="Two shipped zones spell it Solider">

`Config/Zones.luau` zones 1 and 2 list the team `"Solider"`, carried over verbatim from the
original build. Zones 3 and 4 spell it `"Soldier"`.

Team names are matched exactly, so those two spellings are two different teams. Pick one and
fix the other four entries to match your actual team.

</Callout>

## The level check

Optional, and only meaningful when [uxrLevel](../uxrlevel/) is installed. Without it the
check returns false and everything else keeps working.

```lua
level = { team = "Police", min = 2 },
```

| Field | Meaning |
|---|---|
| `team` | Which team's XP track to read |
| `currentTeam` | `true` reads the player's current team instead |
| `min`, `max` | Level bounds |
| `level` | An exact level number |
| `minXP`, `maxXP`, `xp` | The raw XP value instead of the level |
| `name`, `names` | Match the level's name |
| `playtime` | `{ min = 3600 }`, in seconds |

Within one rule the fields are ANDed: `{ team = "Police", min = 2, playtime = { min = 3600 } }`
means Police level 2 or higher **and** an hour of playtime.

A list of rules is ORed:

```lua
level = {
    { team = "Police", min = 2 },
    { team = "Soldier", min = 4 },
},
```

Police level 2 or Soldier level 4.

<Callout type="tip" title="This is the one place you get AND">

Every other factor is ORed against the rest. A single level rule with several fields is the
only way to express "this and that" in a door's access table.

</Callout>

## What a player sees

| Outcome | Feedback |
|---|---|
| Allowed | Green light, granted sound, the door moves |
| Refused | Red light, denied sound, the door does not move |
| Locked down | Red light, denied sound, and the light stays red |

The lights return to white after `LightFlashTime`, one second by default, unless the door is
under lockdown.

## The range check

```lua
InteractRange = 18,
```

The server measures from the player's `HumanoidRootPart` to the door's `HandlePart`, or its
first panel if there is none, and refuses anything further away.

That is what stops a modified client from opening every door on the map at once. Raise it
for very large doors where the prompt sits far from the handle.

## Rate limits

```lua
RateLimit = { door = 0.4, zone = 0.6 },
```

Seconds between accepted requests per player, per kind. A faster request is refused
outright, before the access check runs.
