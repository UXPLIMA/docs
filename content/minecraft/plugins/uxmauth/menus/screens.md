---
title: Screens
order: 501
description: The register, login, replace, captcha, code, recover and pin dialogs, and their Bedrock forms.
icon: app-window
---

Every screen is a dialog, written in [screens.conf](../config/screens-conf.md). A text that starts with `@` is a line of
the messages folder, read in the player's own language; any other text is yours, in MiniMessage. A Bedrock player gets a
native form with the same fields.

| Screen | Shows | Fields and buttons |
|---|---|---|
| `register` | To a player with no account | Password, confirm, rules to accept, remember me, submit, cancel |
| `login` | To a player with an account | Password, remember me, "this name is mine" (claim), "forgot password", submit, cancel |
| `replace` | A right password that no longer meets the rules | New password, confirm |
| `captcha` | When the captcha asks | The code (`<code>` in a body line), the answer |
| `code` | The second step | The app's code |
| `recover` | After "forgot password" | The mailed code, a new password; the other button goes back to login |
| `pin` | With `pin-pad = true` | Ten keys, erase, enter, remember; the masked digits under the pad |

| Button | Shown while |
|---|---|
| Remember me | `sessions.mode` is `chosen`; empty takes the box off |
| This name is mine | The server can prove names through Mojang and the rule takes a claim |
| Forgot password | Mail is on |

Reorder a body, add a line to it, or point a label at your own words: nothing is fixed by the code.

Related: [The waiting room](../features/waiting-room.md), [screens.conf](../config/screens-conf.md)
