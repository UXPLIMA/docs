---
title: Invites
order: 1304
description: Asking a player to join a land, with a role, before they become a member.
icon: mail
---

The invites module makes joining a land a two-step affair: a manager sends an invite, and the player accepts or declines it. An invite can carry a role, and it lapses after a set time. Players open their invites with `/claim invites`.

Module `invites` · enabled by default · `modules/invites/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim invite send <player> [role]` | Invites a player to the land, with the member role or the role named | `uxmclaims.ability.member.invite` |
| `/claim invite revoke <player>` | Takes back an invite the land sent | `uxmclaims.ability.member.revoke` |
| `/claim invite sent` | Opens the open invites of the land | `uxmclaims.ability.member.revoke` |
| `/claim accept [land]`, `/claim invite accept` | Accepts an invite | none |
| `/claim deny [land]`, `/claim invite reject` | Declines an invite | none |
| `/claim invites`, `/claim invite inbox` | Opens your invites | none |
| `/claim invite toggle` | Turns receiving invites on or off | none |

`send`, `revoke` and `sent` act on the land you stand in, else your only land. `accept` and `deny` without a land take your only invite, or open the inbox when you have several.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.ability.member.invite` | everyone | Invite players to lands you manage |
| `uxmclaims.ability.member.revoke` | everyone | Take back invites on lands you manage |
| `uxmclaims.admin.invites` | op | Manage invites on every land |

| Role permission | Allows |
|---|---|
| `invites:manage` (3.x `MANAGE_INVITES`) | Send and take back invites for the land |

| Limit or cost | Default | Applies to |
|---|---|---|
| `uxmclaims.limit.invite.<n>` | 10, highest wins | Open invites one land may have, read from the owner when online |
| `uxmclaims.delay.invite.<n>` | 0, lowest wins | Seconds between two invites a player sends |
| Cost `invites:invite` | 0 | Sending an invite; the price may step up with the land's member count |

## Settings

| Key | Default | What it does |
|---|---|---|
| `expire-after` | `7d` | How long an invite stays open; `never` keeps it until answered |
| `max-received-per-player` | `10` | Most open invites one player can hold |
| `notify-online-target` | `true` | Tell the invited player straight away when online |
| `notify-on-join` | `true` | Tell players about their open invites when they join |
| `receive-invites-by-default` | `true` | Whether players who never used `/claim invite toggle` receive invites |
| `purge-interval` | `5m` | How often lapsed invites are removed from the database (at least 30 seconds) |

`/uxm reload` applies changes.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_invites_count%` | Open invites the player's current land has sent |
| `%uxmclaims_invites_received%` | Open invites the player holds |
| `%uxmclaims_invites_enabled%` | `true` when the player receives invites |

## Notes

- With this module on, `/claim trust <player>` without a duration sends an invite instead of adding the member. A timed trust still adds the member at once.
- An invite is refused for a player who is already a member, is banned from the land (with [Bans](bans.md) on), already has an open invite there, or has invites turned off.
- A manager who is not the owner cannot invite with a role ranked above their own. The owner and visitor roles cannot be given by invite.

Related: [Lands](lands.md), [Bans](bans.md), [Inbox](inbox.md)
