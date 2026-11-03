---
title: Reference
order: 701
description: Every %uxmauth_...% placeholder and what it answers.
icon: list
---

Published through PlaceholderAPI under `uxmauth` when it is installed. Each answers for the player asked about, from
memory, on whatever thread asks. A player who has not logged in reads `false` for `authenticated` and nothing for the
rest.

| Placeholder | Answer |
|---|---|
| `%uxmauth_authenticated%` | `true` or `false` |
| `%uxmauth_method%` | How the session got in: `password`, `registered`, `premium`, `bedrock`, `session`, `forced` or `network` |
| `%uxmauth_premium%` | `true` or `false` |
| `%uxmauth_kind%` | `premium`, `cracked` or `bedrock` |
| `%uxmauth_2fa%` | `true` or `false`: an app, or a code by mail |
| `%uxmauth_registered%` | When the account was made, in `placeholders.date-format` |
| `%uxmauth_last_login%` | When the session logged in, in `placeholders.date-format` |

`%uxmauth_time_left%`, the seconds a waiting player has before the timeout, works only inside `effects` lines.

## Examples

```
Tab: %uxmauth_kind%
Scoreboard: Member since %uxmauth_registered%
```

Related: [Effects](../features/effects.md)
