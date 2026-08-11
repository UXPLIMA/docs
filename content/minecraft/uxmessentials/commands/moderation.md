---
title: Moderation
order: 1030
---

## Moderation Commands

The `moderation` module is your full punishment and investigation toolkit: mutes, bans, jails, kicks, warnings, and the lookups that back them up. Every verb is a **Brigadier** command, so tab completion suggests players, jails, and durations as you type, and you only ever see the actions your rank can run.

Punishments are stored in the database (never in world data), so they survive rollbacks and restarts, and every entry records who issued it and why.

<Callout type="tip" title="The `/mod` GUI and picker → confirm flows">

You rarely need to memorise flags. `/mod` opens the moderation management GUI, and **many bare commands open a picker → confirm GUI**: run `/ban Steve` with no reason and you get a reason picker followed by a confirmation screen before anything is written. Type the full command with a reason to skip straight to the action.

</Callout>

---

## Duration & the silent flag

Timed punishments take a **duration** as a compact time token, for example `30m`, `2h`, `1d`, or `7d`. `/mute` and `/warn` treat the duration as optional (omit it for a permanent action); `/tempmute`, `/tempban`, `/tempbanip`, and `/tempwarn` require one.

The **`-s` silent flag** suppresses the public broadcast so only staff see the action. It is parsed as the **leading token inside the reason**, so it comes right before the reason text:

```
/ban Griefer -s Repeated grief, handled quietly
/mute Spammer 1h Chat spam
/mute Spammer 1h -s Chat spam, silent
```

<Callout type="info" title="Duration caps and exemptions">

A staff member can be limited in how long a punishment they may issue via the numbered nodes `uxmessentials.moderation.ban.maxduration.<seconds>` and `uxmessentials.moderation.mute.maxduration.<seconds>`: the highest granted tier wins, and no node means no cap. Players holding `uxmessentials.moderation.exempt` cannot be targeted, and staff with `uxmessentials.moderation.broadcast.receive` see silent actions.

</Callout>

---

## Mutes

| Command | Description | Permission |
|---------|-------------|------------|
| `/mute <player> [duration] [-s] [reason]` | Mute a player; permanent if no duration | `uxmessentials.moderation.mute` |
| `/tempmute <player> <duration> [-s] [reason]` | Timed mute (duration required) | `uxmessentials.moderation.mute` |
| `/unmute <player>` | Lift an active mute | `uxmessentials.moderation.unmute` |
| `/mutehistory <player>` | Review a player's past mutes | `uxmessentials.moderation.mute` |
| `/mutelist` | List everyone currently muted | `uxmessentials.moderation.mutelist` |

## Bans

| Command | Description | Permission |
|---------|-------------|------------|
| `/ban <player> [-s] [reason]` | Permanent ban | `uxmessentials.moderation.ban` |
| `/tempban <player> <duration> [-s] [reason]` | Timed ban | `uxmessentials.moderation.tempban` |
| `/unban <player>` | Lift a ban | `uxmessentials.moderation.ban` |
| `/banhistory <player>` | Review a player's past bans | `uxmessentials.moderation.ban` |
| `/banlist` | List active bans | `uxmessentials.moderation.banlist` |
| `/banip <player\|ip> [reason]` | Ban an IP address (or a player's IP) | `uxmessentials.moderation.banip` |
| `/tempbanip <player\|ip> <duration> [reason]` | Timed IP ban | `uxmessentials.moderation.banip` |
| `/unbanip <ip>` | Lift an IP ban | `uxmessentials.moderation.banip` |

## Jails

| Command | Description | Permission |
|---------|-------------|------------|
| `/jail <player> <jail> [duration] [reason]` | Confine a player to a named jail | `uxmessentials.moderation.jail` |
| `/unjail <player>` | Release a jailed player | `uxmessentials.moderation.unjail` |
| `/togglejail <player> [jail] [reason]` | Jail the player if free, release if jailed | `uxmessentials.moderation.togglejail` |
| `/jails` | List defined jails | `uxmessentials.moderation.jail` |
| `/jailedplayers` | List currently jailed players | `uxmessentials.moderation.jail` |
| `/setjail <name>` | Define a jail at your location | `uxmessentials.moderation.jail` |
| `/jail del <name>` | Remove a jail definition | `uxmessentials.moderation.jail` |

## Kicks

| Command | Description | Permission |
|---------|-------------|------------|
| `/kick <player> [-s] [reason]` | Disconnect a player | `uxmessentials.moderation.kick` |
| `/kickall [reason]` | Kick every non-exempt player | `uxmessentials.moderation.kick` |

## Warns

| Command | Description | Permission |
|---------|-------------|------------|
| `/warn <player> [-s] [reason]` | Issue a warning (escalates per policy) | `uxmessentials.moderation.warn` |
| `/tempwarn <player> <duration> [-s] [reason]` | Warning that expires after the duration | `uxmessentials.moderation.warn` |
| `/unwarn <player>` | Clear a player's warnings | `uxmessentials.moderation.warn` |
| `/warns <player>` | Review a player's warnings | `uxmessentials.moderation.warn` |

## Investigation

Read-only lookups for building a case before you act.

| Command | Description | Permission |
|---------|-------------|------------|
| `/history <player>` | Full disciplinary record | `uxmessentials.moderation.history` |
| `/checkban <player>` | Report a player's ban state | `uxmessentials.moderation.check` |
| `/checkmute <player>` | Report a player's mute state | `uxmessentials.moderation.check` |
| `/sanction <player>` | Aggregated read-only punishment summary | `uxmessentials.moderation.sanction` |
| `/seen <player>` | Last-seen timestamp | `uxmessentials.moderation.seen` |
| `/seenip <player>` | Last-known IP address | `uxmessentials.moderation.seen` |
| `/alts <player>` | Known alternate accounts | `uxmessentials.moderation.seen` |

## Staff Tools

| Command | Description | Permission |
|---------|-------------|------------|
| `/freeze <player>` | Pin a player in place | `uxmessentials.moderation.freeze` |
| `/unfreeze <player>` | Release a frozen player | `uxmessentials.moderation.freeze` |
| `/commandspy` (`cspy`) | Watch other players' commands as they run them | `uxmessentials.moderation.commandspy` |
| `/lockdown [on\|off]` | Refuse logins except for bypass holders | `uxmessentials.moderation.lockdown` |
| `/sudo <player> <command>` | Run a command as another player | `uxmessentials.moderation.sudo` |
| `/staffhistory <staff>` | Sanctions a staff member has issued | `uxmessentials.moderation.staffhistory` |
| `/staffrollback <staff> [limit]` | Revoke a staff member's active sanctions | `uxmessentials.moderation.staffrollback` |
| `/modstats [staff] [days]` | Staff punishment analytics: leaderboard or per-staff breakdown | `uxmessentials.moderation.stats` |
| `/punish <player> <template>` | Apply a configured punishment template | `uxmessentials.moderation.templates` |
| `/mod` | Open the moderation management GUI | `uxmessentials.moderation.gui` |

<Callout type="warning" title="`/lockdown` locks the door">

While lockdown is on, only players holding `uxmessentials.moderation.lockdown.bypass` can join. Grant yourself the bypass before enabling it, or you may lock yourself out on the next reconnect.

</Callout>

---

## Examples

### Timed and silent punishments

```
/tempmute Spammer 2h Chat spam
/tempban Cheater 7d x-ray
/ban Griefer -s Repeat offender, handled quietly
```

### Jails

```
/setjail cells
/jail Griefer cells 1d Griefing spawn
/togglejail Griefer
```

### Investigate before you act

```
/history Suspect
/alts Suspect
/checkban Suspect
```

---

<Callout type="note" title="Rename or disable any of these">

Every command literal above can be renamed, re-aliased, or switched off in `commands/commands.conf` without touching permissions, see [Renaming Commands](../config/commands-conf.md). The permission nodes are keyed to each command's stable id, so grants keep working after a rename.

</Callout>

## Next Steps

- [🛡️ Moderation Feature Guide](../features/moderation.md) - How escalation, jails, and history work in depth
- [🔐 Permission Reference](../permissions/reference.md) - Duration caps, exempt nodes, and every moderation node
- [🕵️ Staff & Presence](staff-presence.md) - Vanish, staff chat, and `/freeze`'s companion tools
