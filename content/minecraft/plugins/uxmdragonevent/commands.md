---
title: Commands
order: 20
description: Player and admin commands, and which commands players may use during an event.
icon: terminal
---

One root command with many aliases: `/dragon`, `/dragonevent`, `/dragon-event`, `/dragonadmin`,
`/uxmdragon`, `/uxmdragonevent`, `/udragon`, `/uxdragon`. Everything below works under any of them.

## Player commands

| Command | What it does | Permission |
|---|---|---|
| `/dragon` or `/dragon help` | The help message, plus the admin part for staff | None |
| `/dragon join` | Joins the running event | `dragonevent.joincommand`, when required |
| `/dragon leave` | Leaves the event and goes to the lobby spawn | None |
| `/dragon fly` | Turns flight on or off inside the event | `dragonevent.fly` |
| `/dragon stats` | Your all-time event statistics | None |

`/dragon join` answers `join-via-portal` unless `settings.join-command.enabled` is `true`. See
[Joining](features/joining.md#the-command).

`/dragon fly` only works for a player who is in the event and standing in the event world.

## Admin commands

All require `dragonevent.admin`.

| Command | What it does |
|---|---|
| `/dragon start` | Rebuilds the event world and starts an event |
| `/dragon stop` | Ends the running event with no rewards |
| `/dragon reload` | Reloads the files. Ends a running event with no rewards |
| `/dragon reload bot` | The same, and reconnects the Discord bot |
| `/dragon setlobbyspawn` | Sets where players are sent when they leave or the event ends |
| `/dragon setcustomspawn` | Sets where players arrive in a custom or vanilla event world |
| `/dragon setdragonspawn` | Sets where the dragon appears in a custom or vanilla event world |
| `/dragon save` | Writes the spawn points, the schedule's record and the statistics to disk |
| `/dragon refresh` | Rebuilds the all-time leaderboards from the saved statistics |
| `/dragon teleport` | Takes you to the event spawn without joining. Needs a running event |
| `/dragon spawndebug` | Shows the spawn point of the world you are standing in |

### Starting and stopping

`/dragon start` does not count toward `auto-start.max-per-month`, and works whether the schedule is
on or off. The world is rebuilt first, so the event begins a few seconds after you are told
`starting`.

`/dragon stop` ends the event at once. Nobody gets rewards, no wins or losses are recorded, and
everyone is told `force-stop`. `/dragon reload` and stopping the server end a running event the same
way, so do neither during an event players care about.

### Reloading

`/dragon reload` reads `config.yml`, `rewards.yml` and `discord.yml` again, rebuilds the event world
and reloads the schedule. A changed bot token, or turning Discord on or off, needs
`/dragon reload bot`.

### Watching an event

`/dragon teleport` puts you in the event world without a session: you are not on the leaderboard,
your hits on the dragon are refused, and you can teleport out freely.

### Saving and refreshing

The plugin saves by itself every 10 minutes, when an event ends and on shutdown. `/dragon save` is
for before a manual restart or a backup.

`/dragon refresh` reads the statistics as they were last saved. To bring the all-time leaderboards
up to the minute, run `/dragon save` first.

## Commands during an event

Players in the event are held to `command-blocking`:

```yaml
command-blocking:
  disable-all-commands: true
  block-namespaced-commands: true
  list-type: WHITELIST
  list:
    - "/example"
```

| Setting | Effect |
|---|---|
| `disable-all-commands: true` | Every command is refused. The list is not read |
| `disable-all-commands: false` | The list decides, as `list-type` says |
| `list-type: WHITELIST` | Only the commands in the list are allowed |
| `list-type: BLACKLIST` | The commands in the list are refused, everything else is allowed |
| `block-namespaced-commands: true` | A command written with a plugin prefix, such as `/essentials:spawn`, is always refused, even when the plain command is allowed |

`/dragon` and all its aliases are always allowed, so a player can always leave. Anyone with
`dragonevent.usecmds` ignores all of this.

To allow a few commands, set `disable-all-commands: false` and keep the whitelist:

```yaml
command-blocking:
  disable-all-commands: false
  block-namespaced-commands: true
  list-type: WHITELIST
  list:
    - "/msg"
    - "/r"
```

The list is matched against the first word exactly as the player types it: with the slash, and with
the same capital letters. Each alias is a separate entry: allowing `/msg` does not allow `/tell`.

A blacklist has to name every teleport, home, warp and shop command on the server, including the
ones a plugin adds next month. A whitelist only has to name what you want to allow.
