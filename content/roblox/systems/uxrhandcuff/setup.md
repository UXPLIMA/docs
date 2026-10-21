---
title: Setup
order: 1
description: Install, make the teams, hand out the tool, and make your first arrest.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrHandcuffSystem` into
**ServerScriptService** and press Play once.

## 1. Make the teams

The shipped config expects three teams in the `Teams` service:

| Team | Role |
|---|---|
| `Police` | The officers |
| `Civilian` | The suspects |
| `Jailed` | Where a jailed player is put |

Names are matched exactly, capital letters included. Add them before you test, or nothing
will be permitted, because a missing team pair means no permission.

## 2. Put the tool in StarterPack

A `Tool` named exactly **`Handcuff`**, in `StarterPack` or given out by your own script.

The server checks for it on every single action. An officer without it in hand gets
`Hold your handcuffs first` and nothing happens.

<Callout type="warning" title="The tool must be equipped, not just carried">

The check looks for the tool inside the player's **character**, which is where an equipped
tool lives. A handcuff sitting in the backpack does not count.

That is deliberate: the officer visibly has their cuffs out.

</Callout>

## 3. Press Play with two players

Use the Studio two-player local server. Put one on Police and one on Civilian.

Walk the officer within 8 studs of the civilian. A billboard appears above the civilian's
head with a `Cuff` button, and a matching button bar at the bottom of the screen.

## 4. Cuff, grab, search

| Key | Does |
|---|---|
| `C` | Cuff. Their tools are taken and they slow to a walk |
| `G` | Grab. They are welded in front of you and you can walk them anywhere |
| `T` | Search. A list of what you took off them |
| `F` | Takedown. They drop to the floor and cannot move |
| `R` | Read them their rights, as a speech bubble |
| `P` | Open the arrest form |

The billboard changes with each state, so the officer always sees only what is available.

## 5. Make an arrest

With the suspect grabbed, press `P`. The form asks for:

| Field | Rule |
|---|---|
| Time | Between `MinJailTime` and `MaxJailTime`, 5 to 100 seconds by default |
| Charges | Ticked from the team's `PenaltyList` |
| Reason | Between 15 and 100 characters by default |

Confirm and they respawn on the `Jailed` team with a countdown on screen. When it runs out
they are released to `Civilian` automatically.

<Callout type="tip" title="The reason length limits are there for a reason">

`MinReasonLength = 15` stops officers typing `asd`, and the maximum stops essays that break
your webhook layout. They are the two most commonly loosened settings, and the two most
commonly regretted.

</Callout>

## 6. Build a jail

Nothing in this system moves a player to a location. The jail is a **team**, so build a cell
and set that team's spawn point inside it.

The player is respawned when jailed and again when released, so spawn points are what put
them where they belong.

## 7. Set up bail, or turn it off

The shipped `Civilian` entry has cash bail at 400 of a leaderstats value named `Cash`, and a
Robux bail pointing at a developer product that is not yours.

```lua
RobuxBailId = 3252720675,
```

<Callout type="danger" title="Replace or clear the shipped Robux product id">

That id is a real developer product belonging to somebody else. Leaving it in means your
jailed players are prompted to buy somebody else's product, and the purchase does not
release them because the receipt arrives in a different game.

Set it to your own product id, or set `RobuxBailId = 0` to hide the button.

</Callout>

See [Bail](bail.md).

## 8. Decide on admins

`Config/Admin.luau` ships with `Enabled = false` and empty lists. Turn it on and add
yourself if you want the chat commands. See [Admins and chat commands](admin.md).

## 9. Publish

Jail time saves to a DataStore, so a published game is where you test persistence. In
Studio, `StudioMode = "auto"` gives every play session a clean slate, which is what you want
while building.
