---
title: Anti-glitch
order: 406
description: Three levels that stop a player placing and breaking the same block, and the bypass.
icon: shield-check
---

| Level | Where | Default | Does |
|---|---|---|---|
| One | `reverse = true` on a task | per task | Takes a count back when an event undoes what the task counted |
| Two | `anti-glitch.session` | on | Remembers every block a player placed and counts nothing for breaking it. Memory only, so it does not survive a restart; `remembered-blocks` (100000) caps it, oldest first |
| Three | `anti-glitch.coreprotect` | off | Asks CoreProtect about a block the memory does not know, off the thread that owns the block; `history-days` (30) is how far back, and an older block reads as natural |

Level three is off because it names a plugin the server may not run. Turned on without CoreProtect, the console says so
once and the other levels keep working. Pass experience from raw play reads the same events, so these levels cover it
too.

`anti-glitch.bypass-permission` (`uxmquests.bypass`) turns every level off for the player who holds it, and the
`spawn-reason` filter with it, so staff can test a quest. Leave it empty to give nobody a bypass.

Related: [config.conf](../config/config-conf.md#anti-glitch)
