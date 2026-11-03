---
title: "Guide"
order: 10
description: "How uxmBackup works, how to set it up and what each file holds."
---

uxmBackup takes backups from inside the server: worlds, files and databases go into one zip archive with a
manifest of every file and its checksum, and the archive goes to one or more storages. Each backup is a named set
with its own schedule, and every storage keeps its own retention rules.

## Install

Put `uxmBackup-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `theme.conf`, `menus/archives.conf` and the message files into `plugins/uxmBackup/`. Out of the
box the `full` set runs every night at 04:00 and the `light` set every six hours, both to the `local` storage in
`plugins/uxmBackup/archives`. `/uxmbackup doctor` says whether everything is in order.

After an edit, `/uxmbackup reload` reads the files again. A file that does not parse keeps the running settings and
the console says why. Any value may be written as `${NAME}` to read it from the environment, which keeps passwords
out of the file.

## Sets

A set under `sets` says what one backup takes:

| Key | Meaning |
|---|---|
| `worlds` | World names. The world is saved to disk before it is read. |
| `paths` | Files and folders, relative to the server folder. |
| `storages` | Ids from the `storages` block the archive goes to. |
| `schedule` | A cron expression or a period. Empty: only on `/uxmbackup run <set>`. |
| `excludes` | Globs left out, added to the global `excludes` list. |
| `databases` | Databases dumped through their own engine, each with `jdbc-url`, `username`, `password`. |

A schedule is either five cron fields, minute hour day-of-month month day-of-week, or a period such as `30m`, `6h`
or `1d`. A period counts from the last run; a cron does not drift with restarts. In cron, a step counts from the
first value of its field: `*/2` in the day of month is the 1st, 3rd, 5th, and `*/3` in the month is January, April,
July and October. Sunday is `0` or `7`.

An exclude is a glob matched against the path inside the archive, without regard to case: `*` stays inside one
folder, `**` crosses folders, and `**/` may also match no folder at all, so `**/*.lock` leaves out lock files at
any depth. A name without a wildcard, such as `logs`, leaves out the folder and everything in it. The plugin
never backs up its own archive folder.

## Storages

Each block under `storages` has an id, a `type` and `enabled`. The types are `local` (a `folder`), `sftp`, `s3`
(any S3 endpoint: MinIO, Backblaze B2, Wasabi, Hetzner), `webdav` and `ftps`; `config.conf` ships a commented
example of each.

Every storage has three retention limits. Whichever is reached first removes the oldest archives:

| Key | Meaning |
|---|---|
| `keep-count` | How many archives stay. |
| `keep-size-mb` | How much space they may take together. |
| `keep-days` | How old an archive may get. |

`0` means the limit is off, not "keep none". The newest archive is never removed, whatever the limits say, so a
server that was off for a month still has its last copy when it starts again.

## Other settings

| Key | Default | Meaning |
|---|---|---|
| `work-folder` | `"work"` | Where an archive is built before it is handed to a storage. |
| `compression-level` | `5` | 0 stores, 9 packs hardest. Worlds are already compressed, so a high level gains little. |
| `free-space-floor-mb` | `2048` | A run is refused when it would leave less free disk than this. |
| `progress.every-seconds` | `15` | How often the console reports a running backup. |
| `participants.timeout-seconds` | `30` | How long other plugins get to write their state before a set is read. |
| `databases.allow-external-commands` | `false` | Lets a database source name `mysqldump` or `pg_dump` as its `command`. |
| `shutdown` | off | Runs `set` when the server stops, for at most `limit-seconds`. |
| `discord.webhook-url` | `""` | Posts a message when a run ends; `on-success` and `on-failure` choose when. |
| `effects` | empty | Actions for `started`, `done`, `partial`, `failed` and `refused`: `[broadcast]`, `[console]`, `[sound]`, `[message]` lines. |
| `storage` | SQLite | Where the history of runs is kept: `sqlite`, `mysql`, `mariadb` or `postgresql`. |

## Restoring

A restore runs only from the console, in three levels:

1. `/uxmbackup restore <id>` changes nothing. It says what an in-place restore would replace and gives a token.
2. `/uxmbackup restore <id> --to <folder>` extracts the archive beside the server and touches nothing live.
3. `/uxmbackup restore <id> --token <token> --passphrase <passphrase>` puts the archive back in place.

Levels 1 and 3 are refused while any player is online, and both need `restore.passphrase-hash`. Run `/uxmbackup hash <passphrase>` in the console and paste the
line it prints into `config.conf`. The token lasts
`restore.token-minutes`. A database dump in the archive is written to `restore.database-folder` and never applied
to a running database.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmbackup` |  | Back the server up. |
| `/uxmbackup version` | `uxmbackup.status` | Read the version this server runs. |
| `/uxmbackup reload` | `uxmbackup.reload` | Read the configuration and the content again. |
| `/uxmbackup run <set>` | `uxmbackup.run` | Run one backup set now. |
| `/uxmbackup list [limit]` | `uxmbackup.list` | The backups this server has, newest first. |
| `/uxmbackup info <id>` | `uxmbackup.list` | What one backup holds and how big it is. |
| `/uxmbackup verify <id>` | `uxmbackup.verify` | Read one backup back and say whether it is whole. |
| `/uxmbackup prune` | `uxmbackup.prune` | Take the backups the retention rules no longer keep. |
| `/uxmbackup storages` | `uxmbackup.status` | Where this server writes its backups, and whether each answers. |
| `/uxmbackup status` | `uxmbackup.status` | What is running now and what is due next. |
| `/uxmbackup menu` | `uxmbackup.menu` | Open the window of archives. |
| `/uxmbackup restore <id> [--to <to>] [--token <token>] [--passphrase <passphrase>] [--in-place]` | `uxmbackup.restore` | The three levels of putting an archive back. |
| `/uxmbackup hash <passphrase>` | `uxmbackup.restore` | Turn a passphrase into the line that goes into config.conf. |
| `/uxmbackup doctor` | `uxmbackup.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/uxmbackup lang` | `uxmbackup.language` | Say what language this player reads, and what else this server has. Players only. |
| `/uxmbackup lang set <language>` | `uxmbackup.language` | Read this plugin in one language, whatever the client is set to. Players only. |
| `/uxmbackup lang reset` | `uxmbackup.language` | Follow the client again, as a player who never chose anything does. Players only. |
| `/uxmbackup lang set-player <player> <language>` | `uxmbackup.player-language` | Set the language another player reads. |
| `/uxmbackup lang reset-player <player>` | `uxmbackup.player-language` | Clear the language another player chose. |

Every node is listed with its default in [reference.md](reference.md). Everything except
`uxmbackup.language` is for operators.

## Examples

**A nightly copy to another machine.** Uncomment the `sftp` example, give it `enabled = true`, a `folder` and a
`key-file`, and add its id to the `storages` list of the `full` set.

**Player data every hour, kept for two days.** Add a set `players { worlds = [], paths = ["world/playerdata"],
storages = ["local"], schedule = "1h" }` and give the `local` storage `keep-days = 2` if nothing else uses it.

**Say in chat when a backup fails.** Set `effects.failed = ["[broadcast] <red>The <set> backup failed: <detail>"]`.
