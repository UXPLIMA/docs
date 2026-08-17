---
title: Permissions
order: 70
icon: shield-check
---

Seven permissions, all defaulting to operators.

| Permission | Grants |
|---|---|
| `dragonevent.admin` | Every admin command |
| `dragonevent.fly` | `/dragon fly`, and automatic flight when `auto-flight` is on |
| `dragonevent.joinevent` | Joining at all, when `settings.permission-required` is on |
| `dragonevent.joincommand` | `/dragon join`, when `join-command.require-permission` is on |
| `dragonevent.usecmds` | Bypasses `command-blocking` entirely |
| `dragonevent.teleport.bypass` | Bypasses the block on teleporting out of the event world |
| `dragonevent.fly.bypass` | Bypasses flight being disabled on join and on finish |

## What players need

Nothing. `/dragon`, `/dragon leave` and `/dragon stats` require no permission, and joining through
the portal is open unless you turn on `permission-required`.

The two conditional permissions only apply when their setting is on:

- `dragonevent.joinevent` matters only with `settings.permission-required: true`
- `dragonevent.joincommand` matters only with `join-command.enabled: true` and
  `require-permission: true`

## Flight as a perk

`dragonevent.fly` is the one permission worth thinking about as a reward. It grants `/dragon fly`
and, with `auto-flight: true`, flight from the moment a player enters.

It is a real advantage in a dragon fight, not a cosmetic one — a flying player reaches the dragon
whenever they want and never falls. If your leaderboard pays out meaningfully, giving flight to a
donor rank is giving them the leaderboard.

## The two bypasses

`dragonevent.teleport.bypass` and `dragonevent.fly.bypass` exist for staff who need to move around
and keep flying while an event runs. Both weaken the event's rules for whoever holds them.

`dragonevent.teleport.bypass` in particular lets a player teleport out of the event world while still
holding a session. Keep it on staff groups only.

## dragonevent.usecmds

Bypasses `command-blocking` completely. A player with it can use `/home`, `/tpa` or any shop command
mid-fight — which is precisely what the blocking exists to prevent.

Staff need it to moderate an event. Nobody else should have it.

## A staff group

```
dragonevent.admin
dragonevent.usecmds
dragonevent.teleport.bypass
dragonevent.fly
dragonevent.fly.bypass
```

Everything needed to run, watch and fix an event from inside it.

## A donor group

```
dragonevent.joincommand
```

Skipping the walk to the portal is a fair perk. `dragonevent.fly` is not — see above.
