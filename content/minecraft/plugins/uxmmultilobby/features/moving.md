---
title: Moving
order: 402
description: What happens when a player clicks a lobby or names one, the checks, and the cooldown.
icon: arrow-right-left
---

A click in the window, `/lobby <id>` or `/quicklobby` asks the proxy to move the player. Before it does, the plugin
checks, in order:

| Check | Refused with |
|---|---|
| The lobby exists | `no-such-lobby` |
| The player is not on it already | `already-there` |
| The player holds its `permission` | `not-permitted` |
| It answered and is not stale | `unreachable` |
| It has room | `full` |
| The cooldown has passed | `cooldown` |

A move that passes says `connecting` and goes through the proxy. A locked lobby in the window is still a button: it
answers with the reason rather than doing nothing.

## Cooldown

`connect.cooldown-ms` (3000) is how long a player waits between two moves. A lobby switch costs the proxy a full
handshake, so a held-down key would be a small denial of service without it. `0` turns it off. A move refused for
another reason (full, offline, no permission) does not start the cooldown.

Related: [Outcomes](outcomes.md), [Load balancing](balancing.md)
