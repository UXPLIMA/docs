---
title: Staff commands
order: 303
description: Every /uxmauth subcommand, its node, and the seven that run only from the console.
icon: shield
---

`/uxmauth`, alias `/auth`.

| Command | Node | What it does |
|---|---|---|
| `/uxmauth` | `uxmauth.admin.version` | Says what the command is |
| `/uxmauth version` | `uxmauth.admin.version` | The running version |
| `/uxmauth reload` | `uxmauth.admin.reload` | Reads the configuration and the content again |
| `/uxmauth doctor` | `uxmauth.admin.doctor` | One line per check |
| `/uxmauth info <player>` | `uxmauth.admin.info` | The [info window](../menus/info.md): dates, logins, factors, address and the other accounts on its addresses |
| `/uxmauth history <player>` | `uxmauth.admin.history` | The latest logins with time, address and way in |
| `/uxmauth perf` | `uxmauth.admin.perf` | Time spent hashing, querying the database and asking Mojang |
| `/uxmauth forcelogin <player>` | `uxmauth.admin.forcelogin` | Lets a waiting player in past the login, on whichever server they wait |
| `/uxmauth unban <address>` | `uxmauth.admin.unban` | Lifts the ban on an address and forgets its wrong passwords |
| `/uxmauth unregister <player>` | `uxmauth.admin.unregister` | Removes an account and everything kept under it; its player is sent away wherever they are |
| `/uxmauth setpassword <player> <password>` | `uxmauth.admin.setpassword` | Sets a password and ends the account's sessions |
| `/uxmauth forceregister <player> <password>` | `uxmauth.admin.forceregister` | Makes a password account under a name |
| `/uxmauth disable2fa <player>` | `uxmauth.admin.disable2fa` | Turns off every second factor of an account |
| `/uxmauth markpremium <player>` | `uxmauth.admin.markpremium` | Turns a cracked account premium when Mojang names a paid account under its name |
| `/uxmauth markcracked <player>` | `uxmauth.admin.markcracked` | Turns a premium account cracked |
| `/uxmauth resolve <player> <premium\|cracked>` | `uxmauth.admin.resolve` | Settles a name a cracked holder and a paid owner both want |
| `/uxmauth database move` | `uxmauth.admin.database` | Copies every account to `storage.move-to`; console only, while nobody plays |
| `/uxmauth uuidscheme <scheme>` | `uxmauth.admin.database` | Gives every account the id another UUID scheme gives it; console only, while nobody plays |
| `/uxmauth lang set-player <player> <language>` | `uxmauth.admin.language` | Sets another player's language |
| `/uxmauth lang reset-player <player>` | `uxmauth.admin.language` | Clears it |

## Console only

`forcelogin`, `unregister`, `setpassword`, `disable2fa`, `markpremium`, `markcracked` and `resolve` run only from the
console, whatever node a player holds, unless `staff.in-game` in `config.conf` lists them. A staff account somebody took
over then cannot unregister the other staff, or hand an account to whoever waits under its name. `database move` and
`uuidscheme` are console only whatever `staff.in-game` says.

On a network, the staff commands act on the account wherever its player waits.

Related: [Staff tools](../features/staff.md), [Permissions](../permissions/reference.md#staff)
