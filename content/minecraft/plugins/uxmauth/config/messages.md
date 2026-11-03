---
title: Messages
order: 610
description: How a line is written, and the blocks every screen, reply and effect reads.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default language, then to the line the
code carries.

| Block | Holds |
|---|---|
| `screen.*` | Every dialog: titles, bodies, fields, buttons |
| `join.*`, `reminder.*`, `welcome.*` | The effects' shipped lines |
| `rules.*` | The rules lines of the register screen |
| `change.*` | `/changepassword` |
| `menu.info.*` | The info window |
| Refusals and kicks | A name taken, a password too short, a ban, a timeout, a wrong code |
| `admin.*` | The staff commands, the doctor, alerts |
| `command.*` | The command layer |
| `language.*` | `/uxmauth lang` |

Related: [Languages](languages.md), [screens.conf](screens-conf.md)
