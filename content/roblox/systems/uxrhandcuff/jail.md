---
title: Arrest and jail
order: 4
description: The arrest form, global sentences, the countdown, persistence and leave protection.
icon: gavel
---

## The arrest form

With a suspect grabbed, `P` opens `ArrestFrame`:

| Field | Instance | Rule |
|---|---|---|
| Suspect | `SuspectFrame.TitleText` | Filled in, not editable |
| Time | `TimeFrame.TextBox` | Digits only, clamped to the team pair's limits |
| Normal or global | `JailFrame` and `GlobalJailFrame` checkboxes | One or the other |
| Charges | `InfractionsFrame`, opening `InfractionsFrame` list | From the pair's `PenaltyList` |
| Reason | `ReasonFrame.TextBox` | Length checked against the pair's limits |

The time box strips anything that is not a digit as you type, so a typed sentence is always
a number by the time it is submitted.

The form closes itself if the suspect stops being grabbed, which covers them dying, being
released or the officer being killed mid-form.

## Charges

The charge list opens only when `PenaltyTemplates` is permitted for that team pair. Each row
toggles between `SELECT` and `REMOVE`, and the chosen names are joined with commas into a
single string.

Leaving them all unticked stores `None`.

## Reason

A blank reason becomes `Reason Not Provided` before validation. That string is 19
characters, so it passes the shipped `MinReasonLength` of 15 and a blank reason is accepted.

Set `MinReasonLength` above 19 if you want officers to be forced to type something.

## Normal and global sentences

Two sentence types, two permissions, two sets of limits:

| | Normal | Global |
|---|---|---|
| Permission | `JailPermission` | `GlobalJailPermission` |
| Shipped limits | 5 to 100 seconds | 5 to 50 seconds |

Both save to the DataStore and both survive a rejoin. The distinction is one you define: the
system treats them identically apart from the limits, the permission, and a flag in the
arrest log.

<Callout type="info" title="Global does not mean cross-server">

Both sentence types are stored on the same per-player profile and both persist. Nothing here
propagates a sentence to a server the player is not in.

Use the global flag as your game's "serious offence" tier, with tighter limits and a
permission only senior officers hold.

</Callout>

## What happens on arrest

| Step | |
|---|---|
| 1 | The sentence is clamped and the reason validated |
| 2 | The profile records the release time, officer, charges, reason, teams and BailId |
| 3 | The profile is saved to the DataStore |
| 4 | The arrest is posted to the webhook, if enabled for that pair |
| 5 | Confiscated tools are destroyed and the restraint cleared |
| 6 | The player is moved to `JailTeam` and respawned |

The respawn is what puts them in the cell, via that team's spawn point.

## The jail HUD

`ArrestInfoFrame` on the jailed player's screen, updating once a second:

| Instance | Shows |
|---|---|
| `TimeLabel` | The countdown, or `Time Remaining: Time's Up` |
| `PlayerLabel` | The arresting officer |
| `InfractionsLabel` | The charges |
| `ReasonLabel` | The typed reason |
| `BailFrame` | The bail buttons, if any. See [Bail](bail.md) |

Every string comes from `Config/Localization.luau`, with `$Time`, `$OfficerName` and
`$Infractions` substituted at display time.

## Release

When the countdown reaches zero the client asks the server to release, and the server checks
the time itself before agreeing. There is no trusting the client's clock.

Release clears the jail fields, saves, restores `UnJailedTeam` and respawns the player.

The `UnJailedTeam` is the one recorded at arrest time, so a player arrested as a Civilian
returns to Civilian even if you have since edited the config.

## Persistence

```lua
DataStoreName = "uxrHS_Jail_v2",
StudioMode = "auto",
```

The profile holds the release timestamp, so a player who serves their sentence while
offline is free when they return. One who leaves halfway through comes back to the
remainder.

| `StudioMode` | Behaviour |
|---|---|
| `"auto"` | Saves in a published game, fresh slate every Studio run |
| `false` | Always saves, including Studio. Needs API services enabled |
| `true` | Never saves. Every join is clean |

Profiles are saved on a 60-second loop, when a player leaves, and on server shutdown.

<Callout type="warning" title="A failed load is never saved over">

If the DataStore read fails, the profile is marked as not loaded and every save is refused
until the player rejoins.

That is the right trade: a player briefly appears to have no sentence rather than having
their real sentence overwritten with an empty one.

</Callout>

## Leaving while cuffed

```lua
CuffLeaveProtection = 300,
RejoinGraceSeconds = 15,
LeaveReason = "Left the game while handcuffed",
DefaultJailTeam = "Jailed",
DefaultUnJailedTeam = "Civilian",
```

A player who logs off while cuffed or grabbed is given a five-minute sentence, applied the
moment they rejoin. `0` turns the whole feature off.

The teams come from the arresting officer's team pair when that officer is still around, and
from the two defaults when they are not.

`RejoinGraceSeconds` closes the other loophole: a sentence that expired within the last 15
seconds still sends the player back to jail on rejoin, so leaving and instantly returning
does not skip the last moments.

## What a jailed player can still do

Nothing in this system restricts a jailed player beyond moving them to a team and respawning
them. They can walk, use tools they are given after the arrest, and be cuffed again.

Everything else about what a cell means is your build and your team's permissions.
