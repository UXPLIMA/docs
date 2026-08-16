---
title: Presence
order: 9180
description: AFK, the online list, nicknames, /whois and the server-health readout.
icon: radio
---

Presence is the awareness layer: who is around, what they are called, and how the server is holding up. It owns
`/afk`, `/list`, nicknames, the `/whois` card and the `/gc` health readout. Most of it is for players
themselves; nicknames and the staff list are the staff-facing parts.

Module `presence` · enabled by default · `modules/presence/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/afk` (`/away`) | Toggle your AFK state. | `uxmessentials.afk.use` |
| `/gc` (`/lag`, `/tps`, `/mem`) | Show server health: TPS, uptime and memory. | `uxmessentials.gc.use` |
| `/list` (`/who`, `/online`, `/playerlist`) | List online players. | `uxmessentials.list.use` |
| `/nick` | Set or clear a player's display name. | `uxmessentials.nick.use` |
| `/presencesettings` | Open your personal presence settings panel. | `uxmessentials.presencesettings.use` |
| `/realname` | Look up a player's real name. | `uxmessentials.realname.use` |
| `/staff` | List online staff. | `uxmessentials.staff.use` |
| `/whois` | Show information about an online player. | `uxmessentials.whois.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.afk.use` | everyone | /afk [reason] to toggle your AFK state (auto-AFK also applies on idle). |
| `uxmessentials.gc.use` | op | /gc to show server health: TPS, uptime, memory and loaded chunks. |
| `uxmessentials.list.use` | everyone | /list to see who is online. |
| `uxmessentials.module.presence` | op | Hot-reload / inspect the presence module (AFK, nicknames and vanish state). |
| `uxmessentials.nick.others` | op | /nick \<player> \<name> to set another player's display name. |
| `uxmessentials.nick.use` | everyone | /nick \<name> \| off to set or clear your display name. |
| `uxmessentials.presence.gui` | op | Show the presence settings panel on the /uxmess gui hub. |
| `uxmessentials.presencesettings.use` | everyone | /presencesettings opens your personal presence settings panel. |
| `uxmessentials.realname.use` | everyone | /realname \<player> to look up a player's real account name. |
| `uxmessentials.staff.member` | op | Marks a player as staff so they appear in /staff. |
| `uxmessentials.staff.use` | op | /staff to list online staff members. |
| `uxmessentials.whois.use` | op | /whois \<player> to view an online player's account, identity and status. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `afk-idle-threshold-seconds` | `300` | idle time before a player is auto-marked AFK |
| `afk-sweep-seconds` | `15` | how often the auto-AFK sweep runs |
| `anti-afk.move-threshold-blocks` | `0.0` | horizontal distance that counts as a MOVE rather than a ROTATE |
| `anti-afk.disable-pickup-while-afk` | `false` | cancel item pickups for AFK players |
| `anti-afk.sleep-ignores-afk` | `false` | exclude AFK/vanished players from the sleep percentage |
| `anti-afk.ignored-activity` | `[]` | any of: move, rotate, chat, command, interact, fish |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_afk%` | Whether the player is away (yes/no). |
| `%uxmessentials_afk_duration%` | How long the player has been away, in the compact 1h30m form. |
| `%uxmessentials_presence_afk%` | Whether the player is away (yes/no). |
| `%uxmessentials_presence_afk_duration%` | How long the player has been away, in the compact 1h30m form. |
| `%uxmessentials_presence_afk_reason%` | The reason the player gave when they went away. |
| `%uxmessentials_presence_afk_since%` | The same away duration, under the spelling a config may prefer. |
| `%uxmessentials_presence_nickname%` | The player's nickname, or their name when they have not set one. |
| `%uxmessentials_presence_realname%` | The player's account name, whatever nickname they wear. |
| `%uxmessentials_presence_vanished%` | Whether the player is vanished (yes/no). |
| `%uxmessentials_vanished%` | Whether the player is vanished (yes/no). |
{/* /generated */}

## Notes

- **AFK is both automatic and manual.** A player toggles it with `/afk`, and the idle timer sets it for them;
  `uxmessentials.afk.kick.exempt` keeps a player from being kicked for it.
- **AFK feeds other modules.** One-player sleep ignores AFK players, and the tab list and nametags can mark
  them, because presence is where the state lives.
- **`uxmessentials.staff.member` is what makes an account count as staff** for `/staff` and staff-only
  broadcasts; it is a marker rather than a grant.
- **A nickname changes the display name, not the account.** `/realname` resolves one back, which is what keeps
  a nicknamed player findable.
- **`/gc` counts loaded chunks and entities across the whole server,** so prefer it as a spot check rather than
  something to run on a timer on a large world.

Related: [Vanish](vanish.md), [Staff](staff.md), [Messaging](messaging.md)
