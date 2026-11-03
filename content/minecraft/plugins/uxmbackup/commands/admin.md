---
title: Operator commands
order: 202
description: Every /uxmbackup subcommand, its permission, its arguments and what it answers.
icon: shield
---

`/uxmbackup`, aliases `/backup` and `/uxmb`. Every node is `op` by default.

| Command | Permission | What it does |
|---|---|---|
| `/uxmbackup` | `uxmbackup.status` | Says to add `help` |
| `/uxmbackup version` | `uxmbackup.status` | The running version |
| `/uxmbackup reload` | `uxmbackup.reload` | Reads the configuration and the windows again |
| `/uxmbackup run <set>` | `uxmbackup.run` | Runs one set now. `run.unknown-set` for a name no set has |
| `/uxmbackup list [limit]` | `uxmbackup.list` | The runs, newest first: id, state, size, when. `limit` 1 to 100, default 10 |
| `/uxmbackup info <id>` | `uxmbackup.list` | What one archive holds and how big it is |
| `/uxmbackup verify <id>` | `uxmbackup.verify` | Reads one archive back and says whether it is whole |
| `/uxmbackup prune` | `uxmbackup.prune` | Applies every storage's retention now, oldest first |
| `/uxmbackup storages` | `uxmbackup.status` | Every storage: whether it answers, how many archives, how much space |
| `/uxmbackup status` | `uxmbackup.status` | What is running now and what is due next |
| `/uxmbackup menu` | `uxmbackup.menu` | Opens the [archives window](../menus/archives.md). Player only |
| `/uxmbackup restore <id> [--to <folder>] [--token <token>] [--passphrase <passphrase>] [--in-place]` | `uxmbackup.restore` | The three levels of putting an archive back. Console only |
| `/uxmbackup hash <passphrase>` | `uxmbackup.restore` | Turns a passphrase into the line for `restore.passphrase-hash`. Console only |
| `/uxmbackup doctor` | `uxmbackup.doctor` | The health checks, one line each |
| `/uxmbackup lang` | `uxmbackup.language` | The language you read, and the choices. Player only |
| `/uxmbackup lang set <language>` | `uxmbackup.language` | Chooses a language. Player only |
| `/uxmbackup lang reset` | `uxmbackup.language` | Follows the client again. Player only |
| `/uxmbackup lang set-player <player> <language>` | `uxmbackup.player-language` | Sets another player's language |
| `/uxmbackup lang reset-player <player>` | `uxmbackup.player-language` | Clears another player's choice |

## run

The answer is `run.started`, then one of:

| Answer | When |
|---|---|
| `run.done` | The archive reached every storage, with its id, size and time |
| `run.partial` | It was written and did not reach every storage; the console holds why |
| `run.failed` | With the reason |
| `run.refused` | Another run is in flight, or the disk would drop below `free-space-floor-mb` |

## verify

`verify.running`, then `verify.whole` with the number of files checked, or `verify.broken` with how many are missing and
how many are damaged.

## restore

| Form | Level |
|---|---|
| `restore <id>` or `restore <id> --in-place` | 1: the plan and a token |
| `restore <id> --to <folder>` | 2: extract only |
| `restore <id> --token <token> --passphrase <passphrase>` | 3: in place |

See [Restoring](../features/restoring.md) for every refusal.

## Reload

A reload is swapped in whole; a file that does not parse keeps the running settings, answers `admin.reload-failed`,
and the console says why. `storage` and `commands.conf` need a restart.

Related: [Overview](overview.md), [Permissions](../permissions/reference.md)
