---
title: Sessions
order: 303
description: Skipping the password from the same address, the three modes, and what ends a session.
icon: history
---

`sessions` in [config.conf](../config/config-conf.md#sessions):

| `mode` | Remembers |
|---|---|
| `off` | Nobody: every join asks the password |
| `everyone` | Every login |
| `chosen` (default) | Only a player who ticked "remember me" in the dialog, used the Bedrock toggle, or typed `/session on` |

A player back from the same address within `minutes` (60) skips the password. A password change, `/logout`,
`/session off` and a new address end the session. `skip-second-factor` (`false`) decides whether a session also skips
the app code.

A connection through a shared-address filter never gets a session; see
[Protection](protection.md#shared-addresses).

Related: [Second factor](second-factor.md)
