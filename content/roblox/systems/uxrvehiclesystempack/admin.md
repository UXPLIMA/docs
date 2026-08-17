---
title: The admin panel
order: 12
description: Who counts as an admin, what the panel can do, and what it cannot show you.
icon: shield
---

```lua
Admin = {
    Enabled = true,
    OpenKey = Enum.KeyCode.K,
    Ids = {},
    AllowGameCreator = true,
    GroupId = 0,
    GroupMinRank = 255,
},
```

Press K to open it. Three ways to be an admin, and any one of them is enough:

| Route | Config |
|---|---|
| On the list | `Ids = { 12345, 67890 }` |
| The game creator | `AllowGameCreator = true` |
| High enough in a group | `GroupId` and `GroupMinRank` |

Every action re-checks admin status on the server, so the panel is not the security
boundary. A non-admin invoking the same calls directly gets nothing back.

<Callout type="warning" title="Admin status is decided once, when the player joins">

The panel is only built if the player is an admin at the moment their interface loads.
Adding somebody to `Ids` or promoting them in the group takes effect on their next join,
not immediately.

The server-side checks are live, so a demoted admin loses their powers at once even though
the panel is still on their screen.

</Callout>

## What it does

| Action | Effect |
|---|---|
| List players | Everybody in the server |
| Inspect a player | Their balance and everything they own |
| Give or set coins | Add an amount, or set the balance outright |
| Give a vehicle | Adds it to their profile |
| Take a vehicle | Removes it from their profile |
| Wipe | Resets the profile to a fresh state |

A wipe clears owned vehicles, paint, plates, fuel levels, trunk contents and trunk access,
then re-grants `StarterVehicleId`. The balance is reset to `StartingBalance` only when the
system owns the balance; in leaderstats mode the player's money is left alone, because it is
your game's number and not this system's.

<Callout type="danger" title="A wipe cannot be undone">

There is no confirmation beyond the panel's own, no backup and no history. The profile is
overwritten in memory and written to the DataStore on the next save.

</Callout>

## What it will not show you correctly

The target card reads the internal profile balance rather than the live balance. In
leaderstats mode that number is always zero, no matter how much money the player has.

Giving and setting coins does work in leaderstats mode: those go through the balance
accessor and write the stat. It is only the number on the card that is wrong.

## Only in this server

Everything works on players who are present. There is no way to edit somebody who is
offline, or in another server, because the panel operates on the loaded profile in memory.

## Turning it off

`Admin.Enabled = false` refuses every admin call on the server and stops the panel being
built. That is the right switch for a published game where you do the administration
through your own tools.
