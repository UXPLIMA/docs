---
title: Reference
order: 701
description: Every permission node of uxmAuth with its default and what it allows.
icon: list-checks
---

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmauth.login` | everyone | `/login` |
| `uxmauth.register` | everyone | `/register` |
| `uxmauth.logout` | everyone | `/logout` |
| `uxmauth.changepassword` | everyone | `/changepassword` |
| `uxmauth.claim` | everyone | `/claim` |
| `uxmauth.premium` | everyone | `/premium` |
| `uxmauth.cracked` | everyone | `/cracked` |
| `uxmauth.unregister` | everyone | `/unregister`, confirmed by password or code |
| `uxmauth.session` | everyone | `/session` |
| `uxmauth.captcha` | everyone | `/captcha` |
| `uxmauth.2fa` | everyone | `/2fa` |
| `uxmauth.recover` | everyone | `/recover` |
| `uxmauth.email` | everyone | `/email` |
| `uxmauth.language` | everyone | `/uxmauth lang`, `lang set`, `lang reset` |

A player who lost one of these still sees the command and is told `command.no-permission`. In the dialog waiting room,
`uxmauth.login` and `uxmauth.register` count only as the server's defaults: a permission plugin cannot be asked before the
player exists.

## Second factor

| Node | Default | Means |
|---|---|---|
| `uxmauth.2fa.required` | not declared | Named by `second-factor.required-permission`. A player holding it outright plays only with the app on. The default an operator gets does not count |

## Staff

| Node | Default | Allows |
|---|---|---|
| `uxmauth.admin.*` | op | Declared as the family. It lists no children: grant the nodes below, or use a permission plugin's wildcard |
| `uxmauth.admin.doctor` | op | `doctor` |
| `uxmauth.admin.version` | op | `/uxmauth`, `version` |
| `uxmauth.admin.reload` | op | `reload` |
| `uxmauth.admin.info` | op | `info` |
| `uxmauth.admin.history` | op | `history` |
| `uxmauth.admin.perf` | op | `perf` |
| `uxmauth.admin.forcelogin` | op | `forcelogin`; console only unless `staff.in-game` opens it |
| `uxmauth.admin.unban` | op | `unban` |
| `uxmauth.admin.unregister` | op | `unregister`; console only unless opened |
| `uxmauth.admin.setpassword` | op | `setpassword`; console only unless opened |
| `uxmauth.admin.forceregister` | op | `forceregister` |
| `uxmauth.admin.disable2fa` | op | `disable2fa`; console only unless opened |
| `uxmauth.admin.markpremium` | op | `markpremium`; console only unless opened |
| `uxmauth.admin.markcracked` | op | `markcracked`; console only unless opened |
| `uxmauth.admin.resolve` | op | `resolve`; console only unless opened |
| `uxmauth.admin.alerts` | op | Hearing failed logins, bans, shared addresses and staff actions |
| `uxmauth.admin.database` | op | `database move`, `uuidscheme`; console only whatever a player holds |
| `uxmauth.admin.language` | op | `lang set-player`, `lang reset-player` |

On Velocity there are no defaults: the console may run everything, and a player needs the node from a permission plugin.

Related: [Commands](../commands/overview.md)
