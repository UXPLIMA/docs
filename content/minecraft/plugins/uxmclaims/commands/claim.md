---
title: Claim commands
order: 202
description: The root command, the lifecycle subcommands, flags, spawn, vault and chat.
icon: land-plot
---

## The bare command

| Command | What it does |
|---|---|
| `/claim` | Context aware — see below |
| `/claim menu` | Always opens the personal menu, ignoring context |

With `generalSettings.smartCommand: true`, `/claim` on its own decides what you meant:

| You are standing | Result |
|---|---|
| In a claim you are a member of | The claim management menu |
| In a claim you are not a member of | Your personal menu |
| On free land touching a claim you own | Buys the chunk and extends that claim |
| On free land anywhere else | Confirmation, then a new claim |

Set `smartCommand: false` and `/claim` behaves exactly like `/claim menu`.

## Lifecycle

| Command | What it does | Ability node |
|---|---|---|
| `/claim create [name]` | Create a claim on this chunk | `uxmclaims.ability.claim.create` |
| `/claim delete` | Delete the current claim — confirms first | `uxmclaims.ability.claim.delete` |
| `/claim rename <name>` | Rename it | `uxmclaims.ability.claim.rename` |
| `/claim extend <duration>` | Push the expiry date out | `uxmclaims.ability.claim.reschedule` |
| `/claim transfer <player>` | Hand ownership to a member — confirms first | `uxmclaims.ability.claim.transfer` |
| `/claim leave` | Leave a claim you are a member of | — |

Without a name, a new claim is named from `claimSettings.defaultName`, which is
`Claim of %player.name%` out of the box.

`extend` takes a compound duration: `30m`, `12h`, `7d`, `1d12h30m`. Units are `d`, `h`, `m`, `s` and
the parser is case-insensitive. An input that matches nothing is refused with
*"Invalid duration format! Example: 1d12h30m"*.

`transfer` refuses three cases up front: you are not the owner, the target is yourself, or the target
is not already a member. Invite them first.

## Selection

| Command | What it does |
|---|---|
| `/claim select` | Opens a picker listing your claims |
| `/claim select <name>` | Selects that claim for subsequent commands |
| `/claim list` | Opens the list of claims you own or belong to |

Selection is what lets you run `/claim role create …` while standing somewhere else entirely.

## Members and bans, the short forms

| Command | What it does | Ability node |
|---|---|---|
| `/claim trust <player>` | Send an invite | `uxmclaims.ability.member.invite` |
| `/claim kick <player>` | Remove a member — confirms first | `uxmclaims.ability.member.kick` |
| `/claim ban <player>` | Ban a player, immediately | `uxmclaims.ability.member.ban` |
| `/claim unban <player>` | Lift a ban | `uxmclaims.ability.member.unban` |

`/claim trust` does not add anyone directly — it sends an invitation the target must accept. The
longer forms under [`/claim member`](members.md) do the same work with more options.

## Flags

| Command | What it does | Ability node |
|---|---|---|
| `/claim setflag <flag> <true\|false>` | Set one flag explicitly | `uxmclaims.ability.claim.flag.<FLAG>` |
| `/claim toggleflag <flag>` | Flip one flag | `uxmclaims.ability.claim.flag.<FLAG>` |

Flag names are the enum names, case-insensitive: `PVP`, `TNT_EXPLOSIONS`, `FIRE_SPREAD`. Tab
completion offers all 32. An unknown name answers *"Flag could not be found."* The full list is in
[Flags](../protection/flags.md).

The ability node is per flag, so `uxmclaims.ability.claim.flag.PVP` grants exactly one of them, and
`uxmclaims.ability.claim.flag.*` grants all.

## Spawn, teleport and the vault

| Command | What it does | Ability node |
|---|---|---|
| `/claim setspawn` | Set the claim spawn to where you stand | `uxmclaims.ability.claim.relocate` |
| `/claim spawn` | Teleport to the current claim's spawn | — |
| `/claim spawn <name>` | Teleport to a warp or a claim by name | — |
| `/claim vault` | Open the shared vault | `uxmclaims.ability.vault.access` |
| `/claim warps` | Open the public warp list | — |

`/claim spawn <name>` searches in a fixed order: public warps first, then warps in claims you own,
then warps in claims you are a member of, and finally claims by name. The first match wins — so a
public warp named `shop` shadows your own claim named `shop`.

Teleports wait `uxmclaims.delay.teleport` seconds, `3` by default.

## Chat

| Command | What it does |
|---|---|
| `/claim chat` | Toggle claim chat mode on or off |
| `/claim chat <message>` | Send one message to members without toggling |

Requires the `CLAIM_CHAT` role permission. In toggle mode everything you type goes to online members
until you toggle back. The format is `claimChatFormat` in `messages.yml`. If nobody else with the
permission is online you get *"No online members in this claim."*

## Utility

| Command | What it does |
|---|---|
| `/claim showborders` | Toggle the border overlay |
| `/claim invites` | Open your invitation inbox |
| `/claim confirm` | Confirm the pending action |
| `/claim cancel` | Cancel the pending action |
| `/claim reload` | Reload every config file · needs `uxmclaims.admin` |
