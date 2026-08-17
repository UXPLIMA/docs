---
title: Moderation
order: 6
description: Bans, mutes, warnings with an auto-action, notes and the two kinds of jail.
icon: gavel
---

## Bans

Bans go through Roblox's own ban API rather than a kick loop of the system's own, which
means they are enforced by the platform, cover alternate accounts as far as Roblox can tell,
and apply across the whole universe.

| Command | Rank | Does |
|---|---|---|
| `ban` | HeadAdmin | Permanent |
| `tempban` | HeadAdmin | For a duration |
| `directBan` | HeadAdmin | By username or user id, works while they are offline |
| `banall` | Owner | Everybody below you |
| `unban` | HeadAdmin | Lifts a ban |

Every ban carries two reasons: a display reason the player sees, built from the message
templates, and a private reason naming the admin who issued it.

```lua
BanDisplayReason     = "You are banned from this game. Reason: $reason",
TempBanDisplayReason = "You are temporarily banned from this game. Reason: $reason | Duration: $duration",
```

<Callout type="warning" title="Bans apply to the whole universe">

`ApplyToUniverse` is set, so a ban issued in one place bans the player from every place in
the experience.

That is almost always what you want, and it is worth knowing before you hand the ban command
to somebody who only administers one of your places.

</Callout>

`checkban` looks a user's Roblox ban status up without changing anything, which is the
command to reach for before you argue with somebody about whether they were banned here.

## Mutes

| Command | Rank | Does |
|---|---|---|
| `mute` | Admin | Permanent |
| `tempmute` | Admin | For a duration |
| `unmute` | Admin | Lifts it |
| `muteall` | HeadAdmin | Everybody at once |
| `unmuteall` | HeadAdmin | Lifts them all |

Mutes are the system's own, stored on the punishment profile with the reason and the admin
who issued it. A muted player is muted again the moment they rejoin, checked on join
against the stored expiry.

`punish` is the soft option: one warning plus a fifteen minute mute, at Mod rank, for the
cases that do not deserve a moderator's full attention.

## Warnings

```lua
WarnThreshold       = 3,
WarnAutoAction      = "kick",
WarnAutoBanDuration = 86400 * 7,
```

| Command | Does |
|---|---|
| `warn` | Adds a warning with a reason |
| `unwarn` | Removes the most recent one |
| `warns` | Shows the history |

When the count reaches `WarnThreshold`, `WarnAutoAction` fires:

| Value | Effect |
|---|---|
| `"none"` | Record only |
| `"kick"` | Kick on the threshold-th warning |
| `"tempban"` | Ban for `WarnAutoBanDuration` seconds |
| `"ban"` | Permanent ban |

`WarnThreshold = 0` disables the automatic action entirely and leaves warnings as a record.

<Callout type="tip" title="Three strikes is a policy, not a default to leave alone">

The shipped setup kicks on the third warning, which is gentle. `"tempban"` with a day or a
week is the setting most roleplay games end up on, because a kick is not a consequence to
somebody who was leaving anyway.

Whatever you choose, tell your moderators: they are the ones issuing the third warning.

</Callout>

Warnings are stored per user id and work on players who are offline, so a moderator
reviewing a report the next day can still act.

## Notes

| Command | Does |
|---|---|
| `note` | Leaves an admin note on a player, offline included |
| `unnote` | Removes one by index |
| `notes` | Lists them |

Notes carry no penalty. They are the memory of your moderation team: "argued with staff on
the 3rd", "claims to be a friend of the owner". A player never sees them.

## Jails

Two kinds, and the difference matters.

| Command | Rank | Cell | Survives |
|---|---|---|---|
| `jail` | Admin | A private cell cloned from `Storage/Builds/JailCell` | The session |
| `sendjail` | Admin | The shared `PublicCell` you place in the world | A rejoin |
| `unjail` | Admin | Releases from either | |

`jail` builds the cell around the player and destroys it on release. `sendjail` stands them
on the public cell's spawn part and, because the jail state is saved when they leave and
restored when they return, follows them across sessions.

Both are swept every five seconds for expiry, and again when a player joins or respawns.

<Callout type="warning" title="Both cells are yours to build">

`Storage/Builds` and the world folder ship empty. Without a `JailCell` model containing a
`SpawnPart`, the jail command tells the admin the model is missing and does nothing;
`sendjail` says the same about `PublicCell`.

A cell that is not actually a sealed box is a jail a player can walk out of. The system
places them, it does not contain them.

</Callout>

## Chat spying

`chatHijacker` copies a player's chat messages to the admin who ran it, and
`unchathijack` stops it. Head Admin and above.

<Callout type="danger" title="This is surveillance, and it is silent">

The target is not told, and nothing on their screen changes. Whether that is acceptable
depends on your game and on the laws that apply to your players.

Keep it at Head Admin or raise it further, and make sure your moderation team knows when it
is appropriate to use. The action is logged, so the log is your accountability.

</Callout>

## Where punishments are stored

| Store | Holds |
|---|---|
| `PlayerPunishmentData_v2` | Bans, mutes, warnings, notes, punishment history |
| `PlayerJailData` | Jail state per user |
| `uxrAPS_PermRanks_v1` | Persistent rank overrides |

<Callout type="warning" title="Two of those names are not namespaced">

`PlayerPunishmentData_v2` and `PlayerJailData` are generic names with nothing identifying
this system in them.

If another system in your game happens to use the same DataStore name, the two will
overwrite each other's rows. Check before you install a second admin or moderation system,
and rename here if you find a collision.

</Callout>

## What the target sees

| Action | They get |
|---|---|
| Kick | The kick message with the reason |
| Ban | Roblox's own ban screen with the display reason |
| Mute | Their chat stops working, silently |
| Warning | Nothing by itself, unless your own code reacts |
| Jail | They are standing in a cell |

Nothing here announces itself to the rest of the server. Announcements are a separate
decision, and the messaging commands are how you make it.
