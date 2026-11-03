---
title: Captcha
order: 307
description: When the captcha shows, as text or drawn on a map.
icon: scan-text
---

`protection.captcha`:

| Key | Default | Meaning |
|---|---|---|
| `when` | `off` | `off`, `register` (before every registration), `failures` (before the logins of a name with `after-failures` wrong passwords) or `both` |
| `after-failures` | `3` | Counted per name on this server, forgotten after `failure-memory-minutes` (60) of quiet or a right password |
| `length` | `5` | 3 to 7 characters |
| `characters` | `ABCDEFGHJKMNPQRSTUVWXYZ23456789` | Leaves out 0, O, 1, I and L, which are read as one another |
| `java` | `dialog` | `dialog` (the code as text) or `map` (drawn under noise on a map in the hand, typed with `/captcha`) |

The dialog stops a bot that only types commands but not one that reads the screen; the map is much harder to read. The
map needs `hold.hide-inventory` on, so it never takes an item's place; without it the dialog is shown. Bedrock players
read a form either way, and a player still connecting reads the dialog.

Each showing draws a new code, any answer spends it, and a reconnect starts over. Premium and Bedrock players never meet
it.

Related: [Protection](protection.md)
