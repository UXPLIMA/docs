---
title: The admin panel
order: 5
description: Who counts as staff, the three actions, and where the check actually happens.
icon: shield
---

Staff open a panel with a chat command and can add, remove or set another player's XP on a
chosen team.

## Opening it

```lua
AdminCommand = "!xppanel",
```

In `Shared/Config/Admin.luau`. A player types it in chat; if they pass the permission
check the panel opens. Anyone else typing it gets nothing, with no message.

## Who is staff

Three independent checks. Passing any one of them is enough.

```lua
AdminPanelPermission = {
    TeamPermission = { "Administrator" },
    PlayerIdPermission = { 1234567 },
    GroupPermission = {
        [33106532] = { 255, 244 },
    },
},
```

| Check | Matches on | Notes |
|---|---|---|
| `TeamPermission` | The player's current `Team.Name` | Exact match. Changing team changes access |
| `PlayerIdPermission` | Roblox user id | Numbers, not usernames |
| `GroupPermission` | `[groupId] = { rank, … }` | **Exact ranks**, not a minimum |

<Callout type="warning" title="Group ranks are matched exactly, not as a floor">

`[33106532] = { 255 }` grants rank 255 and nobody else. Rank 254 is not staff. To allow a
range, list every rank in it: `{ 255, 254, 253 }`.

</Callout>

<Callout type="danger" title="Replace the shipped placeholder ids">

`PlayerIdPermission` ships as `{ 123123, 123123 }` and `GroupPermission` names two example
groups. Those are placeholders from the template, not safe defaults. Delete them and put
your own in before you publish, or you are shipping a permission list you have not read.

</Callout>

## The three actions

| Action | Effect |
|---|---|
| Add XP | Increases the target's XP on the selected team |
| Remove XP | Decreases it, floored at zero |
| Set XP | Writes an exact value |

Each one takes effect immediately, notifies the target the same way a normal XP gain
would, re-syncs tools if the rank changed, and forces a save rather than waiting for the
autosave.

The amount field accepts digits only; anything else is stripped as it is typed, and an
empty field falls back to `1`.

## Where the security is

The client hides the panel button from non-staff. That is presentation, not protection.

The server re-runs the same permission check on every action it receives, and refuses with
`[uxrLS]: unauthorized XP modification attempt by <name>` in Output. A player who
reproduces the remote call by hand is refused there.

Both amounts and target names are re-resolved server-side too: the target must be an
online player, and the team must be one that player actually has an XP value for.

## Auditing

The refusal warning names the player who tried. There is no built-in log of successful
staff actions.

If you need one, the `XPChanged` signal carries everything an audit line needs, including
the before and after values. See [The scripting API](api.md).
