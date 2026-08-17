---
title: Commands
order: 20
icon: terminal
---

One root command with many aliases: `/dragon`, `/dragonevent`, `/dragon-event`, `/dragonadmin`,
`/uxmdragon`, `/uxmdragonevent`, `/udragon`, `/uxdragon`. Everything below works under any of them.

## Player commands

| Command | What it does | Permission |
|---|---|---|
| `/dragon` | Help, or the admin help if you have `dragonevent.admin` | — |
| `/dragon help` | The same help | — |
| `/dragon join` | Joins the event | `dragonevent.joincommand`, when required |
| `/dragon leave` | Leaves the event and returns to the lobby | — |
| `/dragon fly` | Toggles flight inside the event world | `dragonevent.fly` |
| `/dragon stats` | Your event statistics | — |

`/dragon join` only exists if `settings.join-command.enabled` is `true`. It is off by default: the
portal is the intended entrance, and a join command lets players enter from anywhere on the server.

`/dragon fly` refuses outside the event world and tells you so. It needs `dragonevent.fly`, the same
permission `auto-flight` uses.

`/dragon leave` returns the player to the lobby spawn. With `price-required-for-every-join: true`
they pay again to come back, which is what stops leaving being a free reset.

## Admin commands

All require `dragonevent.admin`.

| Command | What it does |
|---|---|
| `/dragon start` | Starts an event now |
| `/dragon stop` | Ends the running event |
| `/dragon setlobbyspawn` | Sets where players return to |
| `/dragon setcustomspawn` | Sets where players arrive in a custom world |
| `/dragon setdragonspawn` | Sets where the dragon spawns in a custom world |
| `/dragon save` | Writes spawn points and statistics to disk |
| `/dragon reload` | Reloads the configuration files |
| `/dragon teleport` | Teleports you to the event spawn without joining |
| `/dragon reset` | Resets the event world |
| `/dragon refresh` | Rebuilds the all-time leaderboards |
| `/dragon spawndebug` | Reports the spawn locations the plugin currently holds |

`/dragon teleport` puts you in the event world as a spectator in practice — you are not in the event,
so you have no session, no damage tracking and no rewards. It requires an event to be running.

`/dragon spawndebug` is the first thing to run when an event refuses to start: it tells you which
spawn point is missing.

`/dragon refresh` re-reads `data/stats.yml` and rebuilds the leaderboard cache. Needed after editing
that file by hand; otherwise the leaderboards refresh on their own.

<Callout type="warning" title="/dragon reset rebuilds the event world">

It restores the event world to its starting state. Anything players built or dropped there is gone.
This is normal between events — it is how the arena comes back clean — but do not run it while an
event is in progress.

</Callout>

## Commands during an event

Players in the event are subject to `command-blocking`:

```yaml
command-blocking:
  disable-all-commands: true
  list-type: WHITELIST
  list:
    - "/example"
```

`/dragon` subcommands are always allowed, whatever the configuration says — a player must always be
able to leave. Anyone with `dragonevent.usecmds` bypasses the restriction entirely.

`disable-all-commands: true` with a `WHITELIST` is the safe shape: nothing works except what you
list. The alternative — a blacklist — means every command you forget about is a way out of the
arena with your event inventory.
