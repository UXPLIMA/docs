---
title: Mail files
order: 609
description: The three mails, one file per kind and language.
icon: mail
---

`mail/<kind>_<language>.txt`:

| Kind | Sent when |
|---|---|
| `verify` | `/email add`: the code that confirms an address |
| `login-code` | A login, for a player who chose the code by mail |
| `recovery` | "Forgot password" or `/recover`: the code that sets a new password |

`en`, `tr`, `de`, `es`, `fr`, `pt` and `ru` ship. Write `verify_it.txt` and Italian players read Italian. Nothing is
sent while `mail.enabled` is false.

Related: [Mail and recovery](../features/mail.md)
