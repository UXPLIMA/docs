---
title: Rollback
order: 123
description: Going back from 4.0 to 3.x with /uxm migrate restore-v3, and what you lose by doing it.
icon: undo-2
---

The migration never changes the 3.x tables or `data/claims.db`, and it keeps the 3.x files in `legacy/v3/`.
`/uxm migrate restore-v3` puts those files back so the 3.x jar can start on its own data again.

<Callout type="danger" title="Everything made on 4.0 is lost">

3.x gets its data as it was at the moment of the migration. Lands claimed, members added, warps set, money
paid and every other change made on 4.0 since then are not carried back to 3.x. A copy of the 4.x land tables
is saved, but 3.x cannot read it.

</Callout>

## Going back

1. Run `/uxm migrate restore-v3` on the 4.0 server. It explains what it does; run `/uxm migrate restore-v3 confirm`.
2. Stop the server.
3. Replace the 4.0 jar with your 3.x jar (3.7.1 or later).
4. Start the server.

## What restore-v3 does

| Step | Result |
|---|---|
| Dump | The 4.x land tables are written as tab-separated files to `backups/pre-restore-v3-<time>/` |
| Database marks | `uxmc_meta` gets `active_dataset = v3` and `migration_state = ROLLED_BACK`; on MySQL and PostgreSQL the 3.x lease row is released |
| Files | Every file in `legacy/v3/` goes back to the plugin folder. The 4.x file it replaces, plus `modules.yml`, `modules/` and `lang/`, move to `legacy/v4-<time>/` |
| Marker | `data/state.yml` moves to `legacy/v4-<time>/` and `legacy/v3/` is deleted |

After this, the plugin folder passes the 3.x safety guard. The 4.x tables (`uxmc_*`) and, on SQLite,
`data/storage.db` stay where they are.

## Several servers on one database

Run `/uxm migrate restore-v3 confirm` on one server only: it changes the shared database. On every other
server that runs 4.0 on that database, run `/uxm migrate restore-v3 --local`, which moves only that server's
files, or stop it. A 4.0 server that finds the database rolled back by another server stops with
`E-ROLLED-BACK` and keeps the old claims locked down until you do one of the two.

## When 4.0 cannot start

`legacy/v3/RESTORE-V3.txt` lists the manual way back:

1. Stop the server and replace the 4.0 jar with your 3.x jar.
2. Move every file of `legacy/v3/` back to the plugin folder (`data/metadata.yml` replaces the tombstone).
3. Delete `data/state.yml`.
4. MySQL and PostgreSQL only, run:

```sql
UPDATE uxmclaims_server_lease SET heartbeat = 0 WHERE id = 1;
UPDATE uxmc_meta SET meta_value = 'v3' WHERE meta_key = 'active_dataset';
```

Use the meta table named in your own `RESTORE-V3.txt` if you changed the table prefix.

## Migrating again later

Starting the 4.0 jar on a restored folder runs the migration again from the 3.x data. The 4.x rows of the
rolled-back run are first copied to side tables (report code `W-ROLLED-BACK-KEPT`); drop them when you no
longer need them.

## Removing the 3.x leftovers

Once you stay on 4.0, `/uxm migrate cleanup-v3 confirm` deletes the `uxmclaims_*` tables, `data/claims.db`
and `legacy/v3/`, at the earliest 7 days after the migration (`force` skips the wait). After that,
restore-v3 is no longer possible; the only way back is the `backups/pre-4.0-*` backup, with the same loss of
everything made since.

## Notes

- When the 4.0 start halted, restore-v3 has no database open and only moves the files; do the database
  steps of `RESTORE-V3.txt` by hand.
- A 3.x jar started without restore-v3 refuses to start (3.7.1 and later). An older 3.x jar has no such guard
  and starts on data 4.0 owns: do not use one.

Related: [From 3.x](from-3x.md), [Migration commands](migration-commands.md)
