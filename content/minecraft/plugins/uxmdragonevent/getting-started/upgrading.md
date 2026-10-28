---
title: Upgrading to 2.6.3
order: 14
description: What to check before moving a 2.6.2 server to uxmDragonEvent 2.6.3, and what behaves differently afterwards.
icon: circle-arrow-up
---

The configuration carries over. On the first start, new settings and messages are added to
`config.yml` with their defaults; nothing you changed is reset.

## Before you start

1. Check the server: 2.6.3 runs on 1.21.11 and 26.x.x only. Older versions are no longer supported.
2. Pick the jar for your server from the three in the download. See
   [Installation](installation.md#which-jar).
3. On Folia, or on 26.x.x, install a world plugin first. See
   [Installation](installation.md#world-plugins).
4. Back up `plugins/uxmDragonEvent/`, then stop the server and replace the jar. Do not upgrade while
   an event is running: stopping the server ends it without rewards.

## What changes

| Area | 2.6.2 | 2.6.3 |
|---|---|---|
| `/dragon reset` | Rebuilt the event world | Removed. The world is rebuilt before every event by itself |
| Starting an event | A second start while the world was being rebuilt ran a second event | Refused with `already-starting`. Staff see `starting` while the world is built, and `start-failed` if it cannot be |
| Commands with a plugin prefix | `/essentials:fly` slipped past the list | Blocked during an event by the new `block-namespaced-commands: true` |
| Hitting the dragon | Anyone in the event world counted | Only players in the event can damage it. Staff watching are refused |
| Damage on the killing blow | The whole hit was counted | Only the health the dragon had left is counted |
| `item-blacklist` | Applied to everyone on the server | Applies only to players in the event |
| Dragon health above the server's limit | The dragon failed to spawn | The health is lowered to the limit and the console warns once |
| `%dragonevent_next%` and `_last%` | Shown in the server's timezone | Shown in `auto-start.timezone` |
| World plugins | Worlds | Worlds or MyWorlds. See [Installation](installation.md#world-plugins) |

## After the first start

Run one event by hand before the schedule takes over: `/dragon start`, join it, then `/dragon stop`.
The console names the world plugin it is using on the `Loading platform support` line.

If your `command-blocking` whitelist relied on prefixed commands such as `/essentials:spawn`, list
the plain command instead, or set `block-namespaced-commands: false`. See
[Commands](../commands.md#commands-during-an-event).

Related: [Installation](installation.md), [Troubleshooting](../troubleshooting.md)
