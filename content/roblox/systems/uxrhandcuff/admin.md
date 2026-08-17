---
title: Admins and chat commands
order: 7
description: Who counts as staff, the six commands, and what admins can bypass.
icon: shield
---

Admins skip the per-action permission flags, so they can cuff and jail regardless of teams.
They also get six chat commands.

The whole feature is off until you turn it on.

```lua
Enabled = false,
```

## Who is an admin

Three ways, checked in order, any of which is enough:

```lua
UserPermission = {
    -- 123456789,
},
TeamPermission = {
    -- "Staff",
},
GroupPermission = {
    -- [12345] = { 255, 254 },
},
```

| List | Matches |
|---|---|
| `UserPermission` | Roblox user ids |
| `TeamPermission` | Team names |
| `GroupPermission` | A group id mapped to the rank numbers that count |

Group ranks are **exact numbers**, not minimums. List every rank that should have access.

All three ship empty and commented out, so nothing is granted by accident.

## What admins bypass, and what they do not

| | |
|---|---|
| Bypassed | The `Permissions` flags on the team pair |
| Still enforced | A team pair must exist between the two players |
| Still enforced | The tool must be equipped, for keybind actions |
| Still enforced | Distance, restraint state, ownership and the rate limit |

<Callout type="info" title="Admin status alone does not make an officer universal">

The team pair is where `ActivationDistance` and the sentence limits come from, so a pair
between the admin's team and the target's team has to exist for the keybind actions to work
at all.

The **chat commands** are the way around that: they do not go through the team lookup, the
tool check or the distance check.

</Callout>

## The commands

```lua
Commands = {
    Enabled = true,
    Cuff = "cuff",
    Uncuff = "uncuff",
    Grab = "grab",
    Ungrab = "ungrab",
    Jail = "jail",
    Unjail = "unjail",
},
```

| Command | Does |
|---|---|
| `/cuff <player>` | Cuff from anywhere, any team |
| `/uncuff <player>` | Release, regardless of who cuffed them |
| `/grab <player>` | Drag a cuffed player along |
| `/ungrab <player>` | Stop dragging. They stay cuffed |
| `/jail <player> [seconds] [reason...]` | Jail directly |
| `/unjail <player>` | End a sentence early |

Rename any alias if it clashes with another admin system in your game. An alias set to an
empty string is not registered.

`Commands.Enabled = false` keeps admin permissions while removing the chat commands.

## Player names

Names are matched by **prefix**, against both the username and the display name, and are
case-insensitive.

| Result | Reply |
|---|---|
| No match | `Player not found` |
| Several matches | `Name matches several players` |
| Yourself | `Invalid target` |

Type enough letters to be unambiguous. `/jail bo` on a server with `bob` and `bones` is
refused rather than guessed.

## The state rules still apply

Chat commands respect the same state machine as the keybinds:

| Command | Needs |
|---|---|
| `/cuff` | Free |
| `/uncuff` | Cuffed or grabbed |
| `/grab` | Cuffed |
| `/ungrab` | Grabbed |

`/jail` does not: it works on any player in any state.

## /jail

```lua
Jail = {
    DefaultSeconds = 300,
    MaxSeconds = 86400,
    JailTeam = "",
    UnJailedTeam = "",
    SendWebhook = true,
},
```

An admin jail uses these instead of the per-team-pair settings, because an admin can jail
anybody.

| Field | |
|---|---|
| `DefaultSeconds` | Used when no duration is typed. Five minutes |
| `MaxSeconds` | The cap. One day |
| `JailTeam` | Where they go. `""` leaves their team alone |
| `UnJailedTeam` | Where they return. `""` leaves their team alone |
| `SendWebhook` | Admin jails also go to the arrest log |

Both team names ship empty, so an admin jail out of the box gives a countdown without moving
the player. Fill them in to match your jail team if you want `/jail` to work like an arrest.

The `BailId` of an admin jail is `0`, which matches no team pair, so admin jails have no
bail. That is deliberate: a staff sentence should not be buyable.

## Parsing

```
/jail bob 600 Repeated exploiting
```

The second word is read as a duration only if it is entirely digits. Everything after it is
the reason.

```
/jail bob Repeated exploiting
```

No digits, so the whole rest is the reason and the sentence is `DefaultSeconds`.

That means a reason starting with a number is read as a duration. Put the number later in
the sentence.

## Requirements

<Callout type="warning" title="Chat commands need TextChatService">

They are registered as `TextChatCommand` instances, which only exist under the modern chat.

A game still on the legacy chat gets a warning in Output at startup and no commands. Admin
permissions themselves still work.

Switch the game to `TextChatService` in Game Settings if you want them.

</Callout>

## Feedback

Every command replies to the admin who typed it, with a message from
`Config/Localization.luau`:

| Code | Default text |
|---|---|
| `AdminCuffed` | `Player cuffed` |
| `AdminUncuffed` | `Player released` |
| `AdminGrabbed` | `Player grabbed` |
| `AdminUngrabbed` | `Player ungrabbed` |
| `AdminJailed` | `Player jailed` |
| `AdminUnjailed` | `Player unjailed` |

Failures use the same reason codes as everything else. Translate the text; keep the keys.
