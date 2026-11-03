---
title: Info
order: 402
description: One account as staff read it, its logins and the other accounts on its addresses.
icon: id-card
---

`/uxmauth info <player>` (`/uxmauth bilgi`), node `uxmauth.admin.info`. Five rows; `menus/info.conf`. A Bedrock player
gets a form with the same facts.

| Slot | Holds |
|---|---|
| 11 | The account: kind (password, premium or Bedrock), when it registered, the address it registered from |
| 13 | The last login: when, from where, and how many logins the history keeps |
| 15 | Security: whether it asks an app, backup codes left, whether the code is mailed, the masked mail address |
| 19 to 25 | The latest logins: when, from where, the way in; a grey dye when the history keeps none |
| 28 to 34 | Other accounts seen on its addresses; a click opens that account in this window; a grey dye when none |
| 40 | Close |

## Tokens

`%info_name%`, `%info_kind%`, `%info_registered%`, `%info_origin%`, `%info_last%`, `%info_seen%`, `%info_logins%`,
`%info_app%`, `%info_backup%`, `%info_mail%`, `%info_address%`; in the login list `%login_date%`, `%login_address%`,
`%login_method%`; in the other accounts list `%alt_name%`.

## Conditions and actions

| Name | Is |
|---|---|
| `uxmauth:no-logins` | A condition: the history keeps no login of this account |
| `uxmauth:no-alts` | A condition: no other account was seen on its addresses |
| `uxmauth:open-alt` | Opens the clicked account in this window |

Every window also knows `close`, `open:<menu>`, `command:<line>`, `message:<line>` and
`sound:<name> <volume> <pitch>`.

Related: [Staff tools](../features/staff.md)
