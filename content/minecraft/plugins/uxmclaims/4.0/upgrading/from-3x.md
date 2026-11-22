---
title: From 3.x
order: 121
description: Step by step from a 3.7 server to 4.0, and what the first start does to your folder and database.
icon: database-zap
---

The 4.0 jar converts a 3.7.x plugin folder on its first start. The old files and tables stay where they are
until you remove them, so [Rollback](rollback.md) remains possible.

## Before you start

| Requirement | Why | What stops otherwise |
|---|---|---|
| The data was last written by 3.7.x (`data/metadata.yml` version 370 or higher) | Older layouts are not converted | `E-SRC-OLD`: nothing changes, old claims stay locked |
| The 3.x schema is at version 6 with the 3.7.0 checksums | A hand-edited schema could copy wrong | `E-SCHEMA-DRIFT`, unless you approve with `--accept-drift` |
| Free disk space: twice the plugin folder (without `libs/`, `backups/`, `migration/`) plus the database, plus 50 MB | The backup and the converted copy sit next to the old data | `E-DISK-SPACE`: nothing changes |
| Every 3.x server on the same database is stopped | A running 3.x server would keep writing | `E-LIVE-NODE`: the copy waits until it is stopped |
| A 4.0 license key | Carried over from `generalSettings.licenseKey` to `license.key` | The plugin does not start |

## Step 1: check on 3.x

Update 3.x to 3.7.1 first and run `/claim admin v4check` (permission `uxmclaims.admin`). It only reads, off the
server thread, and reports what the importer will meet:

| Check | What to do |
|---|---|
| Schema version and stored checksums | Start 3.7.x once so the schema reaches version 6 |
| Duplicate member, ban and invite rows | Remove the duplicates |
| Claims without a block row or a vault row | Expect them in the report's synthesized or dropped counts |
| Warp and role names that differ only in case within a claim | Rename one of them |
| MySQL session and global time zone against the JVM | Use UTC, or dates written by 3.x shift by the offset |
| Free disk space | Make room |

It ends with "Ready for uxmClaims 4.x" or a count of checks to fix. 3.7.1 also brings the safety guard (see
below), which is why it is the version to leave 3.x from.

## Step 2: back up and swap the jar

1. Take your own backup of the plugin folder and the database. 4.0 makes one too, but yours does not depend on it.
2. Stop the server and every other server on the same database.
3. Remove the 3.x jar from `plugins/` and put the 4.0 jar in. A 3.x jar left next to it is logged as `E-OLD-JAR`.
4. Start the server.

## What the first start does

| Step | Result on disk |
|---|---|
| Backup | `backups/pre-4.0-<time>/files.zip` (the folder, without `libs/`, `backups/` and the SQLite file) and `BACKUP-INFO.txt`. It holds the license key and database password: keep it private |
| File conversion | 3.x files converted into `migration/staging/<run>/`, then the 3.x `config.yml`, `roles.yml`, `entitlements.yml`, `aliases.yml`, `messages.yml`, `webhooks.yml`, `menu/`, `lang/` and `data/metadata.yml` move to `legacy/v3/` and the 4.x files take their place |
| Tombstone | `data/metadata.yml` becomes `version: 999`, which stops 3.x from patching the 4.x files. `migration/pending.yml` marks the database copy as still to do |
| Database backup | `backups/pre-4.0-<time>/db/`: a copy of the SQLite file, or a gzip dump of every `uxmclaims_*` table |
| Copy | The 3.x rows go into the 4.x tables (`uxmc_*`), page by page (`migration.batch-size` rows per transaction) |
| Verification | Counts, id sets, one main chunk per land, owner memberships, vault items and a behaviour sample are compared. Any failure blocks activation |
| Activation | `data/state.yml` is written, `active_dataset = v4` is set in `uxmc_meta`, and on MySQL/PostgreSQL the 3.x lease row is held for good. `legacy/v3/RESTORE-V3.txt` explains the manual way back |

The 3.x tables (`uxmclaims_*`) and `data/claims.db` are never changed. The modules then load the imported
data on the same start. Until activation, the old claims are locked down: everyone except ops is refused
building, interacting, buckets, entity damage and explosions inside 3.x chunks, and land commands are paused.

## Normal and maintenance import

The importer counts the 3.x rows and estimates the copy time (about 40,000 rows a second from SQLite, 8,000
from a remote database). At or below `migration.sync-threshold-seconds` (20 by default) it copies during
start-up, before anyone can join, and no restart is needed.

Above it, the copy runs in the background in maintenance mode:

- The old claims stay locked down.
- Players without `uxmclaims.migration.bypass` (ops have it) cannot join; they are told the server is moving
  its land data. Players already online stay.
- Progress goes to the log and to `/uxm migrate status`.
- When the copy is done, the report says `I-RESTART-REQUIRED`. The server is not restarted for you. Until
  you restart, the lock-down and the join guard stay.

A copy that stops half way (crash, stop) goes on from its last committed page on the next start.

## Holding the migration and the dry run

Create an empty file named `MIGRATION-HOLD` in `plugins/uxmClaims/` before the first 4.0 start to convert the
files but not copy the database. The old claims stay locked down while you look. Then:

1. `/uxm migrate dry-run` copies the 3.x data into a throw-away target and writes `migration/dry-run-<time>.txt`.
2. `/uxm migrate start` starts the copy at once in maintenance mode. Restart when it is done.

## Reading the report

Each run writes `migration/report-<time>.txt`; `/uxm migrate report` prints its errors. Beside timings it has
a `== Data ==` table (source, target, dropped and synthesized rows per entity), the module switches derived
from your 3.x settings and every message by code.

| Prefix | Meaning |
|---|---|
| `E-` | Error: blocks activation |
| `W-` | Warning: the copy went on, read it |
| `I-` | Information |

| Code | Meaning |
|---|---|
| `I-ADMIN-BYPASS` | The 3.x admin bypass was kept, see [Behaviour changes](behaviour-changes.md) |
| `I-ECONOMY-VAULT-MISSING` | Economy written off because Vault is missing |
| `I-DROPPED` | A 3.x key with no 4.0 target, kept in `legacy/v3/config.yml` |
| `W-EFFECTIVE-DEFAULT` | A missing 3.x key, written with the value 3.x used |
| `W-MYSQL-TZ` | Dates may be shifted by the MySQL time zone |
| `E-VERIFY-*` | A verification check failed |

## When it fails

A failed copy empties the new tables, counts the attempt in `migration/attempts.yml` and keeps the old claims
locked down. The next start tries again. After three failures the importer writes `MIGRATION-HOLD`; fix the
cause and run `/uxm migrate retry`, then restart.

Some failures can be approved with `/uxm migrate start <options>`, kept for the next start:

| Option | Approves |
|---|---|
| `--accept-defects V3` | Exactly the lands the last failed V3 check (lands without one main chunk) listed in `migration/defects.yml` |
| `--accept-defects V4` | Exactly the lands whose owner had no owner membership |
| `--accept-defects V3:<land id>` | One object |
| `--accept-drift` | A 3.x schema at another version or with other checksums |
| `--vault-slots=legacy` | An unreadable `claim_vault.yml`: vault items keep the order of their 3.x slot numbers |

Loss and behaviour checks can never be approved. See [Migration commands](migration-commands.md).

## Several servers on one database

3.x and 4.0 both allow one active server per database (unless you use [network mode](../network-mode.md)).

1. Stop every server on the database.
2. Install 4.0 on one server and let it migrate.
3. Give the other servers the migrated plugin folder of the first one before their first 4.0 start (leave out
   `data/instance.id` and `data/license.cache`). `data/state.yml` tells 4.0 the data is already moved.

<Callout type="danger" title="Do not start 4.0 on a second server with its own 3.x folder">

The importer does not yet see that another server already moved the shared database. A second server that
starts 4.0 on its own unconverted 3.x folder copies the 3.x tables again and replaces the 4.x rows, so every
change made on 4.0 since the first migration is lost.

</Callout>

## The 3.x safety guard

From 3.7.1, a 3.x jar refuses to start on data 4.0 has taken over. It looks for `data/state.yml`, a
`data/metadata.yml` at version 999, `migration/pending.yml` or `legacy/v3/` in the folder, and on MySQL and
PostgreSQL for the 3.x lease row held by 4.0 or `active_dataset = v4` in a 4.x meta table. It logs what it
found and disables itself. Go back with `/uxm migrate restore-v3` instead.

## After the move

- Remove every 3.x jar from `plugins/` (the report's `I-DONE` line).
- Keep `legacy/v3/` and the 3.x tables until you are sure. `/uxm migrate cleanup-v3` deletes them, at the
  earliest 7 days after the migration.
- The importer keeps the three newest `backups/pre-4.0-*` folders.

## Notes

- The maintenance import still starts the modules on the empty tables while the copy runs; the lock-down and
  join guard cover it, and the restart afterwards is required.
- The maintenance kick text is always English for now, whatever the server language.
- The importer has been run live on SQLite; MySQL and PostgreSQL sources are not tested against a real 3.7.0
  server yet. Take your own backup.

Related: [Migration commands](migration-commands.md), [Rollback](rollback.md), [Behaviour changes](behaviour-changes.md)
