---
title: Moderation
order: 9120
description: Bans, mutes, jails, warnings with escalation, freeze, lockdown and the disciplinary history behind them.
---

Moderation is the punishment toolkit: bans and tempbans, mutes, IP bans, jails, warnings that escalate, freezes,
a lockdown switch, and the history that ties them together. Every punishment is a database record holding who
issued it, against whom, why and until when, so punishments survive a world rollback and, on a network, apply on
every backend. The same actions are reachable from `/mod` as a menu.

Module `moderation` · enabled by default · `modules/moderation/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/alts` | List accounts sharing a player's IP. | `uxmessentials.moderation.seen` |
| `/ban` | Permanently ban a player (prefix the reason with -s to ban silently). | `uxmessentials.moderation.ban` |
| `/banhistory` | Review a player's ban history. | `uxmessentials.moderation.ban` |
| `/banip` | Ban a player or IP by address. | `uxmessentials.moderation.banip` |
| `/banlist` | Review currently banned players. | `uxmessentials.moderation.banlist` |
| `/checkban` | Check whether a player is banned. | `uxmessentials.moderation.check` |
| `/checkmute` | Check whether a player is muted. | `uxmessentials.moderation.check` |
| `/commandspy` (`/cspy`) | Watch the commands other players run. | `uxmessentials.moderation.commandspy` |
| `/freeze` | Pin a player in place pending review. | `uxmessentials.moderation.freeze` |
| `/history` | Review a player's full sanction history. | `uxmessentials.moderation.history` |
| `/jail` | Confine a player to a jail, optionally for a duration. | `uxmessentials.moderation.jail` |
| `/jailedplayers` | List the players currently jailed. | `uxmessentials.moderation.jail` |
| `/jails` | List the configured jails. | `uxmessentials.moderation.jail` |
| `/kick` | Kick a player (prefix the reason with -s to kick silently). | `uxmessentials.moderation.kick` |
| `/kickall` | Kick all non-exempt players. | `uxmessentials.moderation.kick` |
| `/lockdown` | Lock the server so only bypass holders can join. | `uxmessentials.moderation.lockdown` |
| `/mod` | Open the moderation management GUI (active punishments + per-player history). | `uxmessentials.moderation.gui` |
| `/modstats` | Show staff punishment analytics. | `uxmessentials.moderation.stats` |
| `/mute` | Mute a player, optionally for a duration (prefix the reason with -s to mute silently). | `uxmessentials.moderation.mute` |
| `/mutehistory` | Review a player's mute history. | `uxmessentials.moderation.mute` |
| `/mutelist` | Review currently muted players. | `uxmessentials.moderation.mutelist` |
| `/punish` | Apply a configured punishment template to a player. | `uxmessentials.moderation.templates` |
| `/sanction` | Show a player's current mute, jail, ban and warning summary. | `uxmessentials.moderation.sanction` |
| `/seen` | Last-seen / last-IP lookup, surfaces alts. | `uxmessentials.moderation.seen` |
| `/seenip` | Last-seen / last-IP lookup, surfaces alts. | `uxmessentials.moderation.seen` |
| `/setjail` | Define a jail at your location. | `uxmessentials.moderation.jail` |
| `/staffhistory` | Review the sanctions a staff member issued. | `uxmessentials.moderation.staffhistory` |
| `/staffrollback` | Revoke a staff member's still-active sanctions. | `uxmessentials.moderation.staffrollback` |
| `/sudo` | Run a command as another player. | `uxmessentials.moderation.sudo` |
| `/tempban` | Ban a player for a duration (prefix the reason with -s to ban silently). | `uxmessentials.moderation.tempban` |
| `/tempbanip` | Ban an IP for a duration. | `uxmessentials.moderation.banip` |
| `/tempmute` | Mute a player for a duration (prefix the reason with -s to mute silently). | `uxmessentials.moderation.mute` |
| `/tempwarn` | Warn a player for a duration (prefix the reason with -s to warn silently). | `uxmessentials.moderation.warn` |
| `/togglejail` | Toggle a player's jail: release if jailed, otherwise jail them. | `uxmessentials.moderation.togglejail` |
| `/unban` | Lift a player's permanent ban. | `uxmessentials.moderation.ban` |
| `/unbanip` | Lift an IP ban. | `uxmessentials.moderation.banip` |
| `/unfreeze` | Pin a player in place pending review. | `uxmessentials.moderation.freeze` |
| `/unjail` | /unjail \<player>. | `uxmessentials.moderation.unjail` |
| `/unmute` | /unmute \<player>. | `uxmessentials.moderation.unmute` |
| `/unwarn` | Clear a player's warnings. | `uxmessentials.moderation.warn` |
| `/warn` | Warn a player (prefix the reason with -s to warn silently). | `uxmessentials.moderation.warn` |
| `/warns` | Issue (standing or timed), review and clear warning history. | `uxmessentials.moderation.warn` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.moderation.<sanction>.maxduration.<seconds>` | op | The longest ban, mute or jail you may hand out, in seconds; the largest tier held wins. |
| `uxmessentials.moderation.ban` | op | /ban \<player> [reason] and /unban \<player>: permanent UUID ban and its lift; /banhistory \<player> reviews a player's full ban/unban history. |
| `uxmessentials.moderation.banip` | op | /banip \<player\|ip> [reason] / /unbanip \<ip>: IP ban with stored-IP alt detection. |
| `uxmessentials.moderation.banlist` | op | /banlist to review the players currently banned. |
| `uxmessentials.moderation.broadcast.receive` | op | Receive the staff sanction broadcast: the one-line announcement a non-silent /ban /mute /kick /warn emits. The -s flag (or broadcast.silent-by-default) suppresses it. Duration tiers ride numbered nodes: uxmessentials.moderation.ban.maxduration.\<seconds> and uxmessentials.moderation.mute.maxduration.\<seconds> cap how long a ban/mute that holder may issue (highest held wins; no node = unlimited). |
| `uxmessentials.moderation.check` | op | /checkban \<player> and /checkmute \<player>: report whether a player is currently banned or muted. |
| `uxmessentials.moderation.commandspy` | op | /commandspy (/cspy) to watch the commands other players run. |
| `uxmessentials.moderation.exempt` | op | Cannot be muted/jailed/tempbanned/kicked/warned/IP-banned/frozen by lower staff. |
| `uxmessentials.moderation.freeze` | op | /freeze \<player> / /unfreeze \<player>: pin a player in place pending review. |
| `uxmessentials.moderation.gui` | op | /mod opens the moderation management GUI (active punishments + per-player history). |
| `uxmessentials.moderation.history` | op | /history \<player>: review a player's full disciplinary record (ban/mute/warn/kick) newest-first. |
| `uxmessentials.moderation.jail` | op | /jail \<player> \<jail> [duration] [reason]; /jails lists configured jails; /jailedplayers lists who is jailed; /setjail \<name> defines a jail at your location; /jail del \<name> removes a defined jail. |
| `uxmessentials.moderation.kick` | op | /kick \<player> [reason]; /kickall [reason] to clear non-exempt players. |
| `uxmessentials.moderation.lockdown` | op | /lockdown [on\|off]: refuse every login except holders of the lockdown bypass; the flag survives restart. |
| `uxmessentials.moderation.lockdown.bypass` | op | Join the server while it is locked down (/lockdown). Held by staff who must stay reachable during a lockdown. |
| `uxmessentials.moderation.mute` | op | /mute \<player> [duration] [reason] (/tempmute is the explicit duration alias); /mutehistory \<player> reviews a player's full mute/unmute history. |
| `uxmessentials.moderation.mutelist` | op | /mutelist to review the players currently muted. |
| `uxmessentials.moderation.sanction` | op | /sanction \<player>: aggregated read-only punishment summary: current mute, jail, ban state and active warning count. |
| `uxmessentials.moderation.seen` | op | /seen, /seenip and /alts \<player>: last-seen / last-IP lookup, surfaces alts. |
| `uxmessentials.moderation.staffhistory` | op | /staffhistory \<staff>: review the sanctions a staff member has issued, newest-first. |
| `uxmessentials.moderation.staffrollback` | op | /staffrollback \<staff> [limit]: revoke a staff member's still-active sanctions (un-ban/un-mute/clear-warns the targets they sanctioned). |
| `uxmessentials.moderation.stats` | op | /modstats [staff] [days]: staff punishment analytics: a most-active-staff leaderboard or a single staff member's breakdown, optionally over the last N days. |
| `uxmessentials.moderation.sudo` | op | /sudo \<player> \<command>: run a command as another player. |
| `uxmessentials.moderation.tempban` | op | /tempban \<player> \<duration> [reason]. |
| `uxmessentials.moderation.templates` | op | /punish \<player> \<template>: apply a configured punishment template (a preset reason + optional duration) as a ban or tempban. |
| `uxmessentials.moderation.togglejail` | op | /togglejail \<player> [jail] [reason]: release the target if jailed, otherwise jail them in the named jail (or the first configured jail). |
| `uxmessentials.moderation.unjail` | op | /unjail \<player>. |
| `uxmessentials.moderation.unmute` | op | /unmute \<player>. |
| `uxmessentials.moderation.warn` | op | /warn \<player> [reason], /tempwarn \<player> \<duration> [reason], /warns \<player> and /unwarn \<player>: issue (standing or timed), review and clear warning history. |
| `uxmessentials.module.moderation` | op | Hot-reload / inspect the moderation module (bans, mutes, jails, warnings and history). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `jails` | `[]` | names of defined jails, e.g. ["spawnjail"] |
| `jail-countdown` | `"online-only"` | online-only (counts only while online) \| wall-clock |
| `muted-blocked-commands` | `["me", "msg", "tell", "w", "whisper", "mail", "helpop", "r", "reply"]` |  |
| `warnings.actions` | `["3:tempmute:1h", "5:tempban:1d"]` |  |
| `broadcast.silent-by-default` | `false` |  |
| `address-strictness` | `"NORMAL"` | NORMAL \| STRICT |
| `discord-notify` | `false` | Discord notification: when true, each successful punishment additionally emits a formatted "who punished whom" line (staff, target, type, reason, duration) on the shared audit channel that the optional Discord bridge (uxmessentials-discord) forwards, so staff see readable, name-based punishment notices in Discord. Off by default, and a no-op when no bridge is installed (the line simply lands in the audit log). |
| `censor-ip-addresses` | `false` | Privacy: when true, /seenip masks the address it shows (e.g. 203.*.*.* for IPv4, 2001:* for IPv6), keeping the alt accounts (which /seenip and /alts list by name, never by IP) visible to staff without exposing the raw address. Off by default so staff see full addresses; turn it on to reduce how much PII the moderation commands surface. The stored IP history is unaffected; this only changes what /seenip renders in chat. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_jailed%` | Whether the player is jailed (yes/no). |
| `%uxmessentials_moderation_ban_issuer%` | Who banned the player. |
| `%uxmessentials_moderation_ban_reason%` | Why the player was banned. |
| `%uxmessentials_moderation_ban_remaining%` | How long is left on the ban, in the compact 1d2h form. |
| `%uxmessentials_moderation_ban_remaining_formatted%` | The same remaining ban, under the spelling a config may prefer. |
| `%uxmessentials_moderation_banned%` | Whether the player is banned (yes/no). |
| `%uxmessentials_moderation_frozen%` | Whether the player is frozen in place by staff (yes/no). |
| `%uxmessentials_moderation_jail_issuer%` | Who jailed the player. |
| `%uxmessentials_moderation_jail_name%` | The named jail the player is held in. |
| `%uxmessentials_moderation_jail_online_only%` | Whether the jail counts down on online time rather than the wall clock (yes/no). |
| `%uxmessentials_moderation_jail_reason%` | Why the player was jailed. |
| `%uxmessentials_moderation_jail_remaining%` | How long is left on the jail, in whole seconds; permanent when only an unjail lifts it. |
| `%uxmessentials_moderation_jail_remaining_formatted%` | The same remaining jail, in the compact 1d2h form. |
| `%uxmessentials_moderation_jailed%` | Whether the player is jailed (yes/no). |
| `%uxmessentials_moderation_mute_issuer%` | Who muted the player. |
| `%uxmessentials_moderation_mute_reason%` | Why the player was muted. |
| `%uxmessentials_moderation_mute_remaining%` | How long is left on the mute, in the compact 1d2h form. |
| `%uxmessentials_moderation_mute_remaining_formatted%` | The same remaining mute, under the spelling a config may prefer. |
| `%uxmessentials_moderation_muted%` | Whether the player is muted (yes/no). |
| `%uxmessentials_moderation_warns%` | How many warnings the player carries. |
| `%uxmessentials_muted%` | Whether the player is muted (yes/no). |
{/* /generated */}

## Notes

- **`-s` is a leading token, not a trailing flag.** `/ban Griefer -s using x-ray` punishes silently: the action
  still happens and is still logged, chat just stays quiet. `broadcast.silent-by-default` flips the default, and
  staff who should see silent broadcasts need the receive node.
- **A bare `/ban <player>` opens the confirm menu.** Supplying a reason acts immediately.
- **Warnings can punish by themselves.** `warnings.actions` maps a count to an action, for example
  `["3:tempmute:1h", "5:tempban:1d"]`. Escalation reads the live count, so `/unwarn` changes what the next
  warning triggers.
- **How long a staff member may punish for is a numbered tier**, so a junior moderator can be capped at an hour
  while a senior one is not.
- **`/staffrollback <staff>` lifts a staff member's still-active punishments in one sweep**, which is the tool
  for a bad batch of calls or a rogue moderator. `/staffhistory` shows what one staff member has issued.
- **`/alts` reads the whole connection history,** not just the current address, so an account that shared an
  address months ago still surfaces.
- **Lockdown is the fast close-the-server switch:** `/lockdown on` refuses every login except holders of the
  bypass node.
- **Jails are named locations** you place with `/setjail`. `jail-countdown` decides whether a timed jail counts
  down only while the player is online or on the wall clock.

Related: [Staff](staff.md), [Security](security.md), [Vanish](vanish.md)
