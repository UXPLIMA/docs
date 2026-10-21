---
title: Ranks and permissions
order: 2
description: The rank ladder, the five ways to hold a rank, immunity flags and overrides.
icon: users
---

## The ladder

```lua
Permissions.Ranks = {
    { Name = "Owner",     DisplayName = "Owner",      Level = 5, Color = "#EF233C", Flags = { … } },
    { Name = "HeadAdmin", DisplayName = "Head Admin", Level = 4, Color = "#9B59B6", Flags = { … } },
    { Name = "Admin",     DisplayName = "Admin",      Level = 3, Color = "#3498DB", Flags = { … } },
    { Name = "Mod",       DisplayName = "Mod",        Level = 2, Color = "#1ABC9C", Flags = {} },
    { Name = "VIP",       DisplayName = "VIP",        Level = 1, Color = "#F1C40F", Flags = {} },
    { Name = "NonAdmin",  DisplayName = "Non Admin",  Level = 0, Color = "#95A5A6", Flags = {} },
}
```

| Field | Does |
|---|---|
| `Name` | What commands and config reference |
| `DisplayName` | What players see |
| `Level` | The ordering. Higher is more powerful |
| `Color` | The badge colour |
| `Flags` | Immunities, described below |

Add and remove ranks freely. A command's permission is a rank name, and the check is
"my level is at least that rank's level", so inserting a rank between two others works
without touching a single command.

## Who holds a rank

```lua
Permissions.Assignments = {
    Admin = {
        Players    = { 12345678 },
        Gamepasses = { 987654321 },
        Assets     = { 1122334455 },
        Groups     = { {5018901, 100} },
        Teams      = { "Staff" },
    },
}
```

| Source | Matches when |
|---|---|
| `Players` | Their user id is in the list |
| `Gamepasses` | They own any pass in the list |
| `Assets` | They own any asset in the list |
| `Groups` | Their rank in the group matches |
| `Teams` | They are on one of the named teams |

Any single match is enough. When several ranks match, the highest level wins.

<Callout type="danger" title="Group ranks are matched exactly, not as a minimum">

`Groups = { {5018901, 100} }` grants the rank to players who are **exactly** rank 100 in
that group. Rank 101 does not match, and neither does 255.

This trips people up constantly, because most Roblox systems treat a group rank as a floor.
List every rank number you want to grant:

```lua
Groups = { {5018901, 100}, {5018901, 150}, {5018901, 255} },
```

</Callout>

## The special grants

```lua
Permissions.AutoRankOwner      = true
Permissions.FriendsRank        = nil
Permissions.VipServerOwnerRank = nil
Permissions.FreeAdminRank      = "NonAdmin"
```

| Setting | Effect |
|---|---|
| `AutoRankOwner` | The game creator is Owner, checked before anything else |
| `FriendsRank` | The creator's friends hold this rank |
| `VipServerOwnerRank` | Whoever paid for a private server holds this rank inside it |
| `FreeAdminRank` | The floor rank every joining player gets |

Set any of the last three to `nil` to disable it, or to a rank name to enable it.

<Callout type="warning" title="FreeAdminRank is a floor, not a default">

It is added to the match list for every player, so setting it to `"Mod"` gives moderator
powers to everybody in the game. It ships as `"NonAdmin"`, which is the harmless value.

`"VIP"` is the interesting one: it turns the VIP tier into a free-for-all baseline, which
is only what you want if VIP commands are toys.

</Callout>

## Immunity flags

Flags are defender-side. They are read on the **target** of an action, not the actor.

```lua
{ Name = "Owner", Level = 5, Flags = {
    Bannable = false, Kickable = false, Mutable = false,
    Jailable = false, Warnable = false,
} },
```

| Flag | Blocks |
|---|---|
| `Bannable` | Bans and temp bans |
| `Kickable` | Kicks |
| `Mutable` | Mutes |
| `Jailable` | Jails |
| `Warnable` | Warnings |
| `Killable` | Kills |
| `Configurable` | Configuration changes |

A missing flag means allowed. `false` means denied, and the denial is absolute: another
Owner cannot ban an Owner either.

<Callout type="warning" title="Only some flags are wired up">

`Bannable`, `Kickable`, `Mutable`, `Warnable` and `Jailable` are checked by the moderation
commands, and `Configurable` by four of the chat-tag commands.

`Killable` is listed in the config comment but nothing asks about it: the kill, damage,
explosion and fling commands do not consult the flags at all. An Owner can be killed by a
Mod.

Adding the check is one line in the command implementation, and the flag machinery is
already generic enough to accept any name.

</Callout>

The shipped ladder reads as a sensible policy: Owners are untouchable, Head Admins can be
jailed but not banned or kicked, Admins can be kicked and muted but not banned, and Mods and
below have no protection at all.

<Callout type="tip" title="Flags are the answer to the admin war problem">

Two admins of equal rank banning each other is the classic failure of an open admin system.
Setting `Bannable = false` on every staff rank ends it, and leaves the ban button working
for the people it is meant for.

Extend the list freely: the check is generic, so a flag named for any action works as soon
as a command asks about it.

</Callout>

## Overrides

Two levels, both applied on top of the assignments.

| Command | Does | Survives |
|---|---|---|
| `u!rank <player> <rank>` | Sets a rank for this server | Until the server restarts |
| `u!permrank <player> <rank>` | Sets a rank permanently | Saved to a DataStore |
| `u!unrank <player>` | Clears the runtime override | |
| `u!unpermrank <player>` | Clears the saved override | |

All four are Owner-only. The saved override is stored in `uxrAPS_PermRanks_v1` and loaded
when the player joins.

<Callout type="info" title="An override beats every assignment, in both directions">

It is checked before the assignment list, so it can promote somebody who matches nothing and
demote somebody who matches Admin.

The exception is `AutoRankOwner`: the game creator is checked first and cannot be demoted by
an override.

</Callout>

## Page gating

```lua
Permissions.NavSeeRank      = "NonAdmin"
Permissions.CommandBarRank  = "VIP"
Permissions.LogsViewRank    = "Mod"
Permissions.PunishViewRank  = "Mod"
Permissions.PostMessageRank = "Mod"
Permissions.GlobalPostRank  = "Admin"
```

| Setting | Controls |
|---|---|
| `NavSeeRank` | Whether the panel loads at all |
| `CommandBarRank` | The command bar popup |
| `LogsViewRank` | The Logs page |
| `PunishViewRank` | The punishment views |
| `PostMessageRank` | Posting a message to this server |
| `GlobalPostRank` | Posting across every server |

`NavSeeRank = "NonAdmin"` means everybody's panel loads, which is why everybody sees the
welcome toast. Raise it to `"Mod"` if you want the panel to be invisible to players.

<Callout type="info" title="Gating hides, the server decides">

Each of these hides part of the interface, and every one of them is also enforced on the
server when the request arrives. A player who bypasses the interface gets a refusal and a
warning in the server log.

</Callout>

## Where a rank is read

| Place | Uses |
|---|---|
| The panel | Which pages and commands to show |
| The dispatcher | Whether the actor may run the command |
| Immunity checks | Whether the target may be acted on |
| The badge | The colour and display name next to a name |

`Permissions.luau` is replicated, so a client can read the whole rank table including the
user ids in it. That is fine for a rank list and worth knowing before you put anything else
in that file.
