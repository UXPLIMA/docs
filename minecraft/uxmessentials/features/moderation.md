---
title: Moderation
order: 1300
description: 'The moderation module is a full punishment toolkit: mutes, bans, tempbans,
  IP-bans, jails, warnings with escalation, freezes, and a complete disciplinary history
  — all backed by the database, all with silent variants, and all reachable from a
  /mod GUI as well as the command line.'
---

Because records live in the database, punishments **survive world rollbacks** and, on
a network, sync across every backend server.

---

## What It Covers

| Area | Commands |
|------|----------|
| **Mutes** | `/mute`, `/tempmute`, `/unmute`, `/mutehistory`, `/mutelist` |
| **Bans** | `/ban`, `/tempban`, `/unban`, `/banhistory`, `/banlist` |
| **IP bans** | `/banip`, `/tempbanip`, `/unbanip` |
| **Jails** | `/jail`, `/unjail`, `/togglejail`, `/setjail`, `/jails`, `/jailedplayers` |
| **Warnings** | `/warn`, `/tempwarn`, `/unwarn`, `/warns` |
| **Kicks** | `/kick`, `/kickall` |
| **Freeze** | `/freeze`, `/unfreeze` |
| **History & review** | `/history`, `/checkban`, `/checkmute`, `/sanction` |
| **Staff oversight** | `/staffhistory`, `/staffrollback`, `/commandspy`, `/sudo` |
| **Analytics & templates** | `/modstats`, `/punish` |
| **Intel** | `/seen`, `/seenip`, `/alts` |
| **Server control** | `/lockdown`, `/mod` (GUI) |

Many bare commands (no reason given) open a **picker → confirm GUI** rather than
acting instantly, which makes accidental punishments hard to fire.

---

## How Punishments Work

Every punishment is a database record: who issued it, against whom, why, when, and
(for timed sanctions) when it expires. `/history <player>` reads the whole trail back.

**Silent mode.** Add `-s` to a punishment to suppress its public broadcast — the
action still happens and is still logged, but chat stays quiet. The `-s` token is
parsed at the front of the reason:

```
/ban Griefer -s using x-ray
```

Whether punishments broadcast at all is set by `broadcast.silent-by-default`; staff
who should *see* those broadcasts need `uxmessentials.moderation.broadcast.receive`.

**Durations.** Timed variants (`/tempban`, `/tempmute`, `/tempwarn`, `/tempbanip`)
take a duration like `1h`, `7d`, `30m`. You can cap how long any given staffer may
issue with the max-duration tier nodes below.

**Naming the target.** A punishment lands on anyone who has joined the server before,
online or not, and the name may be typed in any case: `/ban griefer` reaches the
account that joined as `Griefer`. This holds on offline-mode (cracked) servers too,
where the server's own name cache is not consulted; the plugin keeps its own index of
the name each account last joined under. See
[Offline mode](../getting-started/offline-mode.md) for how that index is sized and
filled.

---

## Warning Escalation

Warnings can trigger automatic follow-up punishments. The `warnings.actions` list
maps a warning count to an action:

```hocon
# modules/moderation/config.conf
warnings {
  actions = ["3:tempmute:1h", "5:tempban:1d"]
}
```

Here a player's 3rd warning auto-applies a one-hour tempmute, and their 5th an
one-day tempban. Escalation runs off the player's live warning count, so `/unwarn`
lowering the count changes what the next warning triggers.

---

## Jails, Freeze & Lockdown

- **Jails** are named locations you define with `/setjail <name>`. `/jail <player> <jail> [duration] [reason]` confines a player there; `/togglejail` flips them in and out. `jail-countdown` decides whether a timed jail counts down only while the player is online or on the wall clock.
- **Freeze** (`/freeze`, `/unfreeze`) pins a player in place for questioning without a formal punishment.
- **Lockdown** (`/lockdown on|off`) refuses all logins except holders of `uxmessentials.moderation.lockdown.bypass` — a fast "close the server" switch.

---

## History, Checks & Intel

| Command | Reports |
|---------|---------|
| `/history <player>` | The player's full disciplinary record |
| `/checkban <player>` · `/checkmute <player>` | Current ban / mute state |
| `/sanction <player>` | An aggregated, read-only punishment summary |
| `/seen <player>` · `/seenip <player>` | Last seen / last IP |
| `/alts <player>` | Known alternate accounts |
| `/staffhistory <staff>` | Sanctions **issued by** a staff member |
| `/staffrollback <staff> [limit]` | Revoke a staff member's active sanctions |

`/staffrollback` is the accountability tool: if a staffer goes rogue or makes a batch
of bad calls, it lifts their still-active punishments in one sweep.

---

## Punishment Analytics

`/modstats` turns the punishment history into numbers. Run it bare for a
**server-wide, most-active-staff leaderboard**; give it a staff name for that one
member's own breakdown:

```
/modstats                 # server-wide leaderboard
/modstats Steve           # Steve's own punishment breakdown
/modstats 7               # server-wide, last 7 days
/modstats Steve 30        # Steve, last 30 days
```

A bare number is read as a day window, so `/modstats 7` scopes the leaderboard to the
last week. Every report counts the four punitive actions — **bans** (permanent, timed
and IP), **mutes** (permanent and timed), **warns** (standing and timed) and
**kicks** — and the leaderboard orders staff by total punishments, busiest first.

The read runs off the tick thread and is bounded, so it never stalls the server even
on a long history. Names resolve to each staffer's *current* name, so a rename since
the punishment still shows correctly. Gated by `uxmessentials.moderation.stats`.

<Callout type="note" title="Jails, unbans and unmutes don't count">

Analytics only tallies what the sanction history records as a punishment: ban,
mute, warn and kick. **Jails are not counted** — they are never written to the
sanction history the report reads — and lifts (`/unban`, `/unmute`) are reversals,
not punishments, so they never appear either. A staffer who only lifted sanctions
won't show up on the leaderboard at all.

</Callout>

---

## Punishment Templates

Templates are named punishment presets — a fixed reason plus an optional duration —
that a staffer applies in one command instead of retyping the reason each time:

```
/punish Griefer griefing
```

You define them under `templates` in the module config. A template **with** a duration
lands a `/tempban`; one **without** lands a permanent `/ban`:

```hocon
# modules/moderation/config.conf
templates {
  griefing { reason = "Griefing", duration = "7d" }
  cheating { reason = "Cheating" }               # no duration → permanent ban
}
```

`/punish` is only a shortcut into the normal ban path: the exempt gate, the kick, the
audit line, the history row, the duration cap and the broadcast all behave exactly as
if you had typed `/ban` or `/tempban` yourself. Template names tab-complete and match
case-insensitively (`/punish Griefer Griefing` finds `griefing`); an unknown name is
refused. Whether the punishment broadcasts follows `broadcast.silent-by-default`.

**Templates ship empty** (`templates {}`) — none are defined out of the box, and
`/punish` needs its own `uxmessentials.moderation.templates` node. A template with a
blank reason or an unparseable duration is skipped at load with a log warning, so a
typo can never produce a broken preset.

---

## Discord Notifications

Set `discord-notify = true` and every successful punishment also emits a readable,
name-based line — *who punished whom, the type, reason and duration* — onto the shared
audit channel:

```hocon
# modules/moderation/config.conf
discord-notify = false   # set true to emit punishment notices on the audit channel
```

The optional [Discord bridge](discord-link.md) (`uxmessentials-discord`) forwards that
channel, so with the bridge installed your staff read clean, name-based punishment
notices in Discord rather than the raw UUID-keyed operator audit. **Off by default**,
and a no-op when no bridge is present — the line simply lands in the audit log like
any other, so turning it on never depends on the Discord jar.

Only the single-target punitive verbs notify — **ban, tempban, mute, jail, warn and
kick**. Lifts, freezes, IP bans, mass kicks and infrastructure events (lockdown, jail
definitions) stay off the notice channel.

---

## The `/mod` GUI

`/mod` opens the moderation management GUI — the same operations as the commands,
arranged as a point-and-click panel, gated by `uxmessentials.moderation.gui`.

---

## Permissions & Caps

Most nodes are staff-default (`op`). Each verb has its own node; the notable
cross-cutting ones:

| Node | Meaning |
|------|---------|
| `uxmessentials.moderation.exempt` | **Target-side immunity** — this player cannot be punished |
| `uxmessentials.moderation.broadcast.receive` | Receives punishment broadcasts |
| `uxmessentials.moderation.stats` | `/modstats` staff punishment analytics |
| `uxmessentials.moderation.templates` | `/punish` — apply a configured template |
| `uxmessentials.moderation.lockdown.bypass` | Can log in during a lockdown |
| `uxmessentials.moderation.ban.maxduration.<seconds>` | Longest ban this holder may issue |
| `uxmessentials.moderation.mute.maxduration.<seconds>` | Longest mute this holder may issue |
| `uxmessentials.module.moderation` | Reload / inspect the module |

The `maxduration.<seconds>` tiers are open-ended numbered nodes: the highest granted
value wins, and **no** node means unlimited. Grant
`uxmessentials.moderation.ban.maxduration.604800` to cap a junior rank at seven-day
bans.

<Callout type="warning" title="Give trusted ranks the exempt node">

`uxmessentials.moderation.exempt` makes a player un-punishable. Grant it to admins
and console-driven automation, but audit it — anyone with it is immune to
`/ban`, `/mute`, `/jail`, and the rest.

</Callout>

---

## Key Settings

```hocon
# modules/moderation/config.conf
enabled = true
jails = []                       # defined via /setjail; persisted here
jail-countdown = "online-only"   # online-only | wall-clock
muted-blocked-commands = []      # commands a muted player also can't run
warnings { actions = ["3:tempmute:1h", "5:tempban:1d"] }
broadcast { silent-by-default = false }
address-strictness = "..."       # how strictly IPs are matched
censor-ip-addresses = true       # hide raw IPs in output
templates {}                     # named /punish presets (empty = none defined)
discord-notify = false           # emit punishment notices on the audit channel
```

---

## Importing Existing Bans

Coming from another punishment plugin? uxmEssentials imports **LiteBans** — bans,
IP-bans, mutes and warns — over a direct JDBC read of its database:

```
/uxmess import litebans --dry-run
/uxmess import litebans
```

Always dry-run first. See [Migrating from EssentialsX](../getting-started/migration.md)
for the full importer workflow, conflict policy, and auto-detection notes.

<Callout type="note" title="LibertyBans is not supported">

Only LiteBans has an import path today. LibertyBans is a reserved id with no
implementation — there is no LibertyBans importer.

</Callout>

---

## Tips & Gotchas

- **`-s` is a leading token**, not a trailing flag — put it before the reason text.
- Bare `/ban <player>` (no reason) opens the confirm GUI; supply a reason to act immediately.
- On a network, mutes and bans propagate to every backend via the cross-server bus, so a ban on one server is a ban everywhere.
- `/commandspy` (alias `cspy`) lets you watch other players' command usage live — handy during an investigation.

---

## Next Steps

- [📟 Moderation Commands](../commands/moderation.md) - Every verb and flag
- [🔐 Permission Reference](../permissions/reference.md) - Exempt, caps, and per-verb nodes
- [⚙️ Per-Module Config](../config/per-module.md) - Jails, escalation, and broadcasts
- [📦 Migrating from EssentialsX](../getting-started/migration.md) - Import LiteBans records
