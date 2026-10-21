---
title: Settings
order: 11
description: The config files, the messages, the folder layout, the remotes and the data stores.
icon: sliders-horizontal
---

## The files

| File | Holds | Replicated |
|---|---|---|
| `Shared/Config/Settings.luau` | Prefix, rate limits, warning policy, messages | Yes |
| `Shared/Config/Permissions.luau` | Ranks, assignments, page gating | Yes |
| `Shared/Config/Commands.luau` | Every command's metadata | Yes |
| `Server/Config/Webhooks.luau` | Discord webhook URLs | No |
| `Server/SharedSecret.luau` | The cross-server secret | No |

<Callout type="danger" title="Only the two files under Server are secret">

Everything in `Shared/Config` is copied into `ReplicatedStorage`, so every client can read
it: the rank list, the admin user ids, the group ids and every command definition.

That is acceptable for a rank system, since none of it grants anything by itself and the
server re-checks every request. It is not acceptable for a webhook URL or the cross-server
secret, which is why those two live in `Server/`.

</Callout>

## Settings

```lua
Debug = true,
Prefix = "u!",
SplitKey = " ",
CommandDebounce = 0.25,
CommandFrequencies = { shutdown = 5, nuke = 3, explosion = 1, vote = 10, permrank = 2, rank = 1 },
WarnThreshold = 3,
WarnAutoAction = "kick",
WarnAutoBanDuration = 86400 * 7,
WelcomeNotification = true,
```

| Field | Does |
|---|---|
| `Debug` | Informational logging to Output |
| `Prefix` | The cosmetic command prefix |
| `CommandDebounce` | Seconds between two runs of a command by one player |
| `CommandFrequencies` | Per-command overrides |
| `WarnThreshold` | Warnings before the automatic action |
| `WarnAutoAction` | `"none"`, `"kick"`, `"tempban"` or `"ban"` |
| `WarnAutoBanDuration` | Seconds, used by `"tempban"` |
| `WelcomeNotification` | The two join toasts |

Changing the prefix changes the chat commands too, because they are registered from it at
startup.

## The private server blacklist

```lua
VIPServerCommandBlacklist = {
    "permrank", "unpermrank",
    "ban", "tempban", "unban",
    "globalservermessage",
    "savemap", "loadmap",
},
```

Commands in this list are refused inside a paid private server, even for an Owner. The
check requires both a private server id and a private server owner, so it applies to paid
private servers and not to reserved servers your own code creates.

<Callout type="tip" title="This list is the answer to private-server abuse">

A player who buys a private server owns that instance socially, and giving them a rank
inside it is a common and reasonable thing to do with `VipServerOwnerRank`.

The blacklist is what keeps that from leaking out: they can moderate their own server, and
they cannot issue a permanent rank, a universe-wide ban or a global announcement from
inside it.

Add anything you would not want a paying customer doing on their own turf.

</Callout>

## Messages

```lua
Messages = {
    ServerClosed         = "The server has been shut down.",
    ServerCloseWarning   = "The server is being shut down by the administrator.",
    LockdownKick         = "A lockdown has been activated on this server …",
    LockedKick           = "This server is locked. You cannot join.",
    KickMessage          = "You Are Kicked From The Game\nReason: $reason\n",
    BanDisplayReason     = "You are banned from this game. Reason: $reason",
    TempBanDisplayReason = "You are temporarily banned from this game. Reason: $reason | Duration: $duration",
    BanSuccess           = "Player permanently banned successfully",
    BanFail              = "Ban failed: $error",
    TempBanSuccess       = "Player temporarily banned for $duration",
    TempBanFail          = "Temporary ban failed: $error",
}
```

`$reason`, `$duration` and `$error` are replaced at display time. These are the lines a
punished player actually reads, so they are worth writing in your game's own voice.

## Localization

```lua
Localization = { Self = "me", All = "all", Other = "other" },
```

Renames the three targeting keywords. The English words keep working alongside whatever you
set. See [Choosing targets](targets.md).

## The folder layout

| Location | Contains |
|---|---|
| `ServerScriptService/uxrAdminPanelSystem` | The package. The only design-time copy |
| `ReplicatedStorage/uxrAdminPanelSystem` | `Shared`, the remotes, and the server list folder |
| `ServerStorage/uxrAdminPanelSystem` | `Storage`: Tools, Hats, Skybox, Builds, MapSave |
| `Workspace/uxrAdminPanelSystem/Builds` | Runtime builds such as jail cells |
| `StarterGui/uxrAdminPanelGui` | The panel |

Everything except the first is recreated at runtime. Edits to the replicas are lost on the
next server start.

## The remotes

One RemoteEvent and one RemoteFunction under `Core/apEvents`, multiplexed by a leading
string.

| Direction | Carries |
|---|---|
| Client to server, event | Command dispatch, posts, votes, chat and messages |
| Client to server, function | Logs, punishment data, permission lookups |
| Server to client, event | Notifications, mutes, blur, spectate, fly, noclip |

Every handler that does anything privileged re-checks the caller's rank and logs a warning
when it refuses. The panel's own gating is a convenience on top of that.

## The data stores

| Store | Holds |
|---|---|
| `PlayerPunishmentData_v2` | Bans, mutes, warnings, notes, punishment history |
| `PlayerJailData` | Jail state |
| `uxrAPS_PermRanks_v1` | Persistent rank overrides |

Bans themselves are held by Roblox, not here; the punishment store keeps the reasons and the
history alongside them.

<Callout type="warning" title="Two store names are generic">

`PlayerPunishmentData_v2` and `PlayerJailData` carry nothing identifying this system.
Another system using the same name would share the rows.

Renaming them is safe before you go live and destructive afterwards, so decide early.

</Callout>

## Adding a command

Two files, always both:

```lua
-- Shared/Config/Commands.luau
Commands.myCmd = {
    Aliases = {"mc"},
    Category = "Utility",
    Permission = "Mod",
    Description = "What it does.",
    Args = {
        { name = "target", type = "Players", default = "me" },
        { name = "amount", type = "Number", default = 10, min = 1, max = 100 },
    },
    Log = true,
    Webhook = false,
    Confirm = false,
}
```

```lua
-- Server/Commands/Utility.luau
myCmd = function(ctx, args)
    -- args.target is one player, args.amount is a clamped number
end,
```

| Metadata field | Does |
|---|---|
| `Aliases` | Alternate names, registered in chat too |
| `Category` | Which browser group it appears in, and which file holds the code |
| `Permission` | A rank name, or a list of rank names for any-of |
| `Args` | The typed argument list, which also builds the composer form |
| `Log` | Write an action log entry |
| `Webhook` | Relay to Discord |
| `Confirm` | Ask for confirmation in the panel |

The code runs once per resolved target, with `args.target` already narrowed to a single
player. Nothing needs registering: the lookup tables are rebuilt on every load.

## Where to start

| You want | Edit |
|---|---|
| To take ownership | `Permissions.Assignments.Owner` |
| Cross-server messages to be yours | `Server/SharedSecret.luau` |
| A different prefix | `Settings.Prefix` |
| Harsher warnings | `Settings.WarnAutoAction` |
| Staff who cannot be banned | `Permissions.Ranks[].Flags` |
| A quieter panel | `Settings.WelcomeNotification`, `Permissions.NavSeeRank` |
| A record that survives restarts | `Server/Config/Webhooks.luau` |
