---
title: Settings
order: 9
description: The eight config files, the global switches, and the on-screen text.
icon: sliders-horizontal
---

## The config files

| File | Holds |
|---|---|
| `Shared/Config/Settings.luau` | The switches on this page |
| `Shared/Config/Teams.luau` | The officer and target matrix |
| `Shared/Config/FreeForAll.luau` | The no-teams alternative |
| `Shared/Config/Actions.luau` | Keybinds, speeds, cooldowns |
| `Shared/Config/Jail.luau` | Leave protection and fallback teams |
| `Shared/Config/Admin.luau` | Staff and chat commands |
| `Shared/Config/Golden.luau` | The carry tool |
| `Shared/Config/Localization.luau` | Every string players see |
| `Server/Config/Webhook.luau` | The arrest log. Server side on purpose |

## Settings

| Field | Default | What it does |
|---|---|---|
| `Debug` | `false` | Extra `[uxrHS]` lines in Output |
| `StudioMode` | `"auto"` | Whether jail time saves. See below |
| `DataStoreName` | `"uxrHS_Jail_v2"` | Where jail data is stored |
| `HandcuffToolName` | `"Handcuff"` | The exact `Tool` name officers must hold |
| `DisableComputerMobileUI` | `false` | Hide the on-screen button bar on computers |

## StudioMode

| Value | Behaviour |
|---|---|
| `"auto"` | Saves in a published game, fresh slate every Studio run |
| `false` | Always saves. Needs API services enabled in Game Settings |
| `true` | Never saves |

`"auto"` is the right default for building: you can arrest yourself repeatedly without an
old sentence following you into the next test.

Switch to `false` only when you specifically want to test persistence in Studio.

## DataStoreName

```lua
DataStoreName = "uxrHS_Jail_v2",
```

Changing it to a new name wipes everybody's jail data, which is a legitimate way to give
your game a clean slate after a rules change.

Two places sharing a name share sentences, so a player jailed in your main game is still
jailed when they enter your lobby, if the lobby also runs this system.

## The tool name

```lua
HandcuffToolName = "Handcuff",
```

Matched letter for letter, capitals included. It is checked on the server for every action,
so renaming the tool without renaming this setting disables the whole system quietly.

## The on-screen button bar

```lua
DisableComputerMobileUI = false,
```

The name is precise: it hides the button bar **on computers**. Touch devices always see it,
because on a phone it is the only way to act.

| Value | Computer | Phone |
|---|---|---|
| `false` | Shown | Shown |
| `true` | Hidden | Shown |

Set it to `true` if your officers prefer the keyboard and the bar is in the way. The
billboard above the suspect's head is unaffected either way.

## Localization

`Config/Localization.luau` holds every string a player reads. Translate the values; never
rename the keys.

| Group | Contains |
|---|---|
| Top level | `None`, `EmptyReason`, `NoPermission` |
| Jail HUD | `TimeRemaining`, `TimesUp`, `ArrestingOfficer`, `Infractions` |
| `Reasons` | The refusal messages, keyed by reason code |
| `TimeFormat` | The countdown units |

### Tokens

Some strings contain `$Tokens` replaced at runtime:

| Token | Becomes |
|---|---|
| `$Time` | The remaining sentence, formatted |
| `$OfficerName` | Who made the arrest |
| `$Infractions` | The charge list |
| `$Price` | A developer product's live Robux price, in bail labels |

Move a token around the sentence freely. Renaming or deleting one leaves the literal text on
screen.

### Time format

```lua
TimeFormat = { Day = "d ", Hour = "h ", Minute = "m ", Second = "s " },
```

Change the letters to translate. Keep the trailing spaces, or the countdown reads `1h5m3s`.

### Reason codes

Every refusal returns a code, and the code is looked up here for the message:

| Code | Default |
|---|---|
| `NoPermission` | `No permission` |
| `NoTool` | `No such tool` |
| `NeedTool` | `Hold your handcuffs first` |
| `BadState` | `You can't do that right now` |
| `BadTarget` | `Invalid target` |
| `BadTime` | `Invalid jail time` |
| `ReasonTooShort` | `Reason is too short` |
| `ReasonTooLong` | `Reason is too long` |
| `TooFar` | `Too far away` |
| `Cooldown` | `Please wait a moment` |
| `NotOwned` | `Not your suspect` |
| `StillJailed` | `Still serving your sentence` |
| `NotJailed` | `Not jailed` |
| `NoProfile` | `Data not loaded yet` |
| `NoBail` | `Bail isn't available` |
| `Insufficient` | `Not enough money` |
| `TargetNotFound` | `Player not found` |
| `AmbiguousName` | `Name matches several players` |

`NotOwned` is the one worth rewording for your game: it means somebody else has already
cuffed that person, which `Not your suspect` says clearly enough for a police game and less
clearly for anything else.

## The interface

The client GUI is `uxrHandcuffSystemGui` in `StarterGui`. Its top-level frames:

| Frame | Purpose |
|---|---|
| `MobileFrame.ListFrame` | The on-screen action bar |
| `ArrestFrame` | The arrest form |
| `InfractionsFrame` | The charge picker |
| `SearchFrame` | The confiscated item list |
| `ArrestInfoFrame` | The jailed player's HUD and bail buttons |

Plus two billboards cloned onto every character: `HandcuffBillboard` on the root part, and
`HeadBillboard` on the head.

Restyle any of them. The instance names are looked up individually and a missing one is
skipped rather than erroring, which means a renamed label shows up as a quietly missing
piece of text rather than a crash.

## Where to start

| You want | Edit |
|---|---|
| Different teams | `Config/Teams.luau` |
| No teams at all | `Config/FreeForAll.luau`, set `Enabled = true` |
| Different keys | `Config/Actions.luau` |
| Longer sentences | `MaxJailTime` on each team pair |
| Staff powers | `Config/Admin.luau` |
| Another language | `Config/Localization.luau` |
| An arrest log | `Server/Config/Webhook.luau` |
