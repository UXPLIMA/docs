---
title: Teams and permissions
order: 2
description: The officer and target matrix, the eleven permissions, and free for all.
icon: users
---

`Config/Teams.luau` is a two-level table read as `TeamSettings[OfficerTeam][TargetTeam]`.

```lua
return {
    Police = {
        Civilian = {
            ActivationDistance = 8,
            Permissions = { ... },
            ...
        },
    },
}
```

One officer team, one target team, one entry describing everything the first may do to the
second.

<Callout type="info" title="An unlisted pair means no permission at all">

This is a whitelist, not a blacklist. Police to Police is not listed, so an officer cannot
cuff another officer. Civilian to anything is not listed, so a civilian holding a stolen
pair of handcuffs can do nothing with them.

That is the safety property worth keeping. Add pairs deliberately.

</Callout>

## Adding teams

| You want | Do |
|---|---|
| Police to act on a second target team | Copy the `Civilian = { ... }` block, rename it |
| A second officer team | Copy the `Police = { ... }` block, rename it |

Both blocks are complete on their own, which means duplicated settings. That is the trade:
each pair can have its own distances, sentence limits, charges and bail.

Every name here must match a `Team` in the `Teams` service exactly.

## The eleven permissions

```lua
Permissions = {
    HandcuffPermission = true,
    GrabPermission = true,
    TakedownPermission = true,
    SearchPermission = true,
    TakeToolPermission = true,
    RemoveToolPermission = true,
    Rights = true,
    ArrestPermission = true,
    JailPermission = true,
    GlobalJailPermission = true,
    PenaltyTemplates = true,
},
```

| Permission | Allows |
|---|---|
| `HandcuffPermission` | Cuff and uncuff |
| `GrabPermission` | Grab and let go |
| `TakedownPermission` | Force the suspect to the ground |
| `SearchPermission` | See the confiscated item list |
| `TakeToolPermission` | Move a confiscated tool into the officer's backpack |
| `RemoveToolPermission` | Destroy a confiscated tool |
| `Rights` | Read the rights speech |
| `ArrestPermission` | Open and submit the arrest form |
| `JailPermission` | Submit a normal sentence |
| `GlobalJailPermission` | Submit a global sentence |
| `PenaltyTemplates` | Open the charge list |

Each is checked on the server for the specific action being requested.

`TakeToolPermission` is the one to think about. An officer who can take a tool keeps it, so
it is also a way to move items between players. The free-for-all config turns both tool
permissions off by default for exactly that reason.

## Distance

```lua
ActivationDistance = 8,
```

Studs, measured between the two `HumanoidRootPart` positions, per team pair.

The same value gates both the on-screen billboard and the server check, so a player cannot
act at a distance by faking the UI.

## Sentence limits

```lua
MinJailTime = 5,
MaxJailTime = 100,
MinGlobalJailTime = 5,
MaxGlobalJailTime = 50,
```

Seconds. The typed number is clamped into the range rather than rejected, so an officer who
types 9999 gets the maximum.

Global sentences have their own, usually tighter, limits. See [Arrest and jail](jail.md) for
what makes a sentence global.

## The reason

```lua
MinReasonLength = 15,
MaxReasonLength = 100,
```

Character counts, and these **are** rejections rather than clamps: too short returns
`ReasonTooShort` and too long returns `ReasonTooLong`, with the message shown on the confirm
button.

## Charges

```lua
PenaltyList = {
    "Public Disturbance",
    "Evading Police",
    "Armed Assault",
    "Reckless Driving",
    "Murder",
    "Illegal Trading",
},
```

What an officer may tick in the arrest form. Add, remove or rename freely, per team pair.

The selected charges are joined with commas and stored on the profile, so they appear on the
suspect's HUD and in the arrest log.

## Rights text

```lua
RightsText = "You have the right to remain silent. ...",
```

Per team pair, so a Police pair and a Federal pair can read different speeches. It is shown
to everybody in the server as a chat bubble, on a cooldown set by
`Actions.RightCooldown`, 10 seconds.

## Jail teams

```lua
JailTeam = "Jailed",
UnJailedTeam = "Civilian",
```

Where the suspect goes while serving and where they return to. An empty string means the
team is left alone.

`UnJailedTeam` is recorded on the profile at arrest time, so a player who is released in a
later session still goes back to the right team.

## BailId

```lua
BailId = 1000,
```

A number linking a jailed player back to their team pair's bail settings, because by the
time they are jailed their team has changed and the pair can no longer be looked up.

<Callout type="warning" title="Every team pair needs a different BailId">

Give each pair its own number, at 1000 or above. The free-for-all entry uses 900 and must
also be different.

Two pairs sharing a BailId means one of them silently gets the other's bail price and
currency, because the lookup returns the first match.

</Callout>

## Free for all

`Config/FreeForAll.luau` has the same shape as one team pair, plus a switch:

```lua
Enabled = false,
```

Set it to `true` and `Config/Teams.luau` is ignored completely: everybody may act on
everybody, subject only to holding the tool.

The rest of the file is the settings that then apply to everyone. The shipped defaults are
deliberately more cautious than the Police entry:

| Setting | Police to Civilian | Free for all |
|---|---|---|
| `TakeToolPermission` | `true` | `false` |
| `RemoveToolPermission` | `true` | `false` |
| `GlobalJailPermission` | `true` | `false` |
| `CashBail` | `true` | `false` |
| `RobuxBail` | `true` | `false` |
| `JailTeam` | `"Jailed"` | `""` |

Free for all suits a game with no teams at all, which is why the jail teams default to
empty. A jailed player then keeps their team and is simply respawned with a countdown, so
you need a spawn location that puts them somewhere jail-like by other means.

## What the server checks, in order

Every action goes through the same gate:

| Check | Failure |
|---|---|
| Officer and target exist and are different | `BadTarget` |
| The officer has the handcuff tool equipped | `NeedTool` |
| A team pair exists for the two teams | `NoPermission` |
| That pair permits this action, or the officer is an admin | `NoPermission` |
| The target is not already somebody else's suspect | `NotOwned` |
| Both have a `HumanoidRootPart` | `BadTarget` |
| The distance is within `ActivationDistance` | `TooFar` |
| The officer is not repeating this action too fast | `Cooldown` |

Then the action's own state requirement is checked. See [Actions](actions.md).

Admins skip only the permission flag. The team pair must still exist, because it is where
the distance and sentence limits come from.
