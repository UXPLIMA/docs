---
title: Migration commands
order: 122
description: Every /uxm migrate and /uxm db command, with what it changes and the permission it needs.
icon: terminal
---

`/uxm migrate` drives the one-time move from 3.x; `/uxm db` looks after the 4.x database schema. Both work
from the console. `/uxmclaims` is the same root as `/uxm`.

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm migrate status` | Shows the migration state, the copy progress, whether maintenance mode is on, failed attempts, whether `MIGRATION-HOLD` exists and the last report | `uxmclaims.admin` |
| `/uxm migrate report` | Prints the run line, the `E-` lines and failed checks of the newest `migration/report-*.txt` (up to 15 lines) | `uxmclaims.admin` |
| `/uxm migrate dry-run [keep]` | Copies the 3.x data into a throw-away target and writes `migration/dry-run-<time>.txt`; `keep` keeps the `uxmcdry<n>_` tables on a server database | `uxmclaims.admin` |
| `/uxm migrate start [options]` | Saves approvals, removes `MIGRATION-HOLD` and, on a held server, starts the copy now in maintenance mode | `uxmclaims.admin` |
| `/uxm migrate retry` | Resets the failed-attempt counter and removes the hold after three failures; restart to try again | `uxmclaims.admin` |
| `/uxm migrate restore-v3 [confirm\|--local]` | Puts the 3.x files back; see [Rollback](rollback.md) | `uxmclaims.admin` |
| `/uxm migrate cleanup-v3 [confirm\|force]` | Deletes the 3.x tables and files for good | `uxmclaims.admin` |
| `/uxm db status` | The schema version and state of every module | `uxmclaims.admin.db` |
| `/uxm db repair <module> [confirm]` | Clears failed migration rows of a module after you fixed its tables by hand | `uxmclaims.admin.db` |
| `/uxm db baseline <module> <version> [confirm]` | Marks V1 to `<version>` of a module as applied, for tables restored without their history | `uxmclaims.admin.db` |
| `/uxm db remap-world <old> <new> [confirm]` | Moves every land, chunk, warp, block and spawn of a world to another world id | `uxmclaims.admin.db` |

Without `confirm`, the commands that change data only explain what they will do.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin` | op | Every `/uxm migrate` command (and the other `/uxm` admin commands) |
| `uxmclaims.admin.db` | op | Every `/uxm db` command. It is a separate node: `uxmclaims.admin` alone does not grant it |
| `uxmclaims.migration.bypass` | op | Joining while a maintenance import runs |

The kernel does not register these nodes with Bukkit, so they default to op. Grant them by name in your
permissions plugin.

## /uxm migrate in detail

**`status`** works in every state. When the start halted (`E-SRC-OLD`, `E-BACKUP`, `E-DISK-SPACE`), no
command is registered at all; the lock-down then answers `/uxm migrate status` and `/uxm migrate start` itself,
for ops, `uxmclaims.admin` and the console.

**`dry-run`** needs a pending migration (files converted, database not yet copied) and an open database. It
runs on a database thread and logs where the report is. Only one dry run runs at a time.

**`start`** takes these options, in any order:

| Option | Effect |
|---|---|
| `--accept-defects <check>[,<check>]` | Approves exactly the objects the last failed check listed in `migration/defects.yml`. Checks: `V3`, `V4` (`V5` and `V11` are accepted as names but list nothing) |
| `--accept-defects <check>:<id>` | Approves one object |
| `--accept-drift` | Copies a 3.x schema that is not at version 6 or has other checksums |
| `--vault-slots=legacy` | Vault items keep their 3.x slot order when `claim_vault.yml` cannot be read |

The approvals go into `migration/approvals.yml` and are removed when the migration activates. On a server held
by `MIGRATION-HOLD`, `start` begins the copy at once in maintenance mode. Otherwise it removes the hold and the
copy runs on the next restart.

**`retry`** is for the hold written after three failed attempts. It deletes `migration/attempts.yml` and
`MIGRATION-HOLD`.

**`cleanup-v3`** refuses until the migration (`migrated-at` in `data/state.yml`) is 7 days old and a
`backups/pre-4.0-*` folder exists; `force` skips both checks. On MySQL, MariaDB and PostgreSQL it drops the
`uxmclaims_*` tables and the two 3.x views but leaves `schema_migrations`. It deletes `data/claims.db` and
`legacy/v3/`. Afterwards `restore-v3` no longer works; the pre-4.0 backup stays.

## /uxm db in detail

**`status`** prints one line per module: `V<applied>/<known>`, the state, whether the module is running and
the last change. States:

| State | Meaning |
|---|---|
| `OK` | Every step applied |
| `PENDING (n)` | n steps wait for the next start |
| `FAILED_MIGRATION` | A step failed earlier; fix the table, then `repair` |
| `CHECKSUM_MISMATCH` | An applied step file was edited; `repair` accepts the jar's file |
| `DB_NEWER` | The database has a version this jar does not know: an older jar on newer data |
| `GAP` | The jar's steps are not numbered without holes (a plugin bug) |
| `UNREADABLE` | The step files or the history could not be read |

**`repair`** deletes the module's failed history rows and sets edited checksums to the jar's value, under the
migration lock. Check the tables first. The module migrates again on the next start.

**`baseline`** writes history rows for V1 to `<version>` without running them. Use it only for tables you
restored from a dump that had no `schema_history` rows.

**`remap-world`** takes world names uxmClaims has seen or UUIDs. `<old>` may be a world that is no longer
loaded; `<new>` may be a loaded world. Use it when a world was recreated with a new UUID, or for lands the 3.x
import placed under a placeholder id because their world was not loaded then. Restart afterwards.

`repair` and `baseline` write what they changed to `migration/<repair|baseline>-<time>.txt`.

## Notes

- The `/uxm migrate` replies and `/uxm modules` are English only; they are not in the language files yet.
- `commands.yml` has local names for `/uxm db` but none for the `/uxm migrate` subcommands.

Related: [From 3.x](from-3x.md), [Rollback](rollback.md), [Database](../configuration/database.md)
