---
title: Permissions
order: 70
description: The seven permissions, what each one really allows, and which are safe to give away.
icon: shield-check
---

Seven permissions, all given to operators by default.

| Permission | Grants |
|---|---|
| `dragonevent.admin` | Every admin command, and entering the event world without joining |
| `dragonevent.fly` | `/dragon fly`, and flight on arrival when `auto-flight` is on |
| `dragonevent.joinevent` | Joining at all, when `settings.permission-required` is on |
| `dragonevent.joincommand` | `/dragon join`, when `join-command.require-permission` is on |
| `dragonevent.usecmds` | Every command during an event, ignoring `command-blocking` |
| `dragonevent.teleport.bypass` | Teleporting into the event world without joining |
| `dragonevent.fly.bypass` | Keeping your own flight when you join, leave or the event ends |

## What players need

Nothing. `/dragon`, `/dragon leave` and `/dragon stats` need no permission, and the entrance is open
unless you turn on `permission-required`.

The two join permissions only matter when their setting is on:

- `dragonevent.joinevent` only with `settings.permission-required: true`
- `dragonevent.joincommand` only with `join-command.enabled: true` and `require-permission: true`

## Flight as a perk

`dragonevent.fly` is the one permission to think about before giving it as a reward. It grants
`/dragon fly` and, with `auto-flight: true`, flight from the moment a player arrives. In a dragon
fight that is a real advantage, not a cosmetic one. See [Joining](features/joining.md#flight).

## The two bypasses

`dragonevent.teleport.bypass` lets a player who is not in the event teleport into the event world,
for example to watch or to moderate. It does not let a player in the event teleport out: nobody can,
and players leave with `/dragon leave`. A player in the event world without joining cannot damage the
dragon.

`dragonevent.fly.bypass` stops the plugin from taking flight away. Without it, flight a player
brought from another plugin is removed when they arrive, and flight is turned off when they leave or
the event ends.

## dragonevent.usecmds

Lets a player in the event use any command: `/home`, `/tpa`, a shop. That is exactly what
`command-blocking` exists to prevent. Staff need it to moderate an event from inside; nobody else
should have it.

## A staff group

```
dragonevent.admin
dragonevent.usecmds
dragonevent.fly
dragonevent.fly.bypass
```

Everything needed to run, watch and fix an event. `dragonevent.admin` already covers entering the
event world without joining.

## A donor group

```
dragonevent.joincommand
```

Skipping the walk to the entrance is a fair perk. `dragonevent.fly` is not.
