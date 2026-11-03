---
title: Restoring
order: 407
description: The three restore levels, the five protections, every refusal, and the restore log.
icon: undo-2
---

Putting an archive back is the most dangerous thing this plugin does, and it is guarded like one.

## Three levels

1. `/uxmbackup restore <id>` changes nothing. It lists what an in-place restore would replace, each item with its size,
   and gives a token.
2. `/uxmbackup restore <id> --to <folder>` extracts the archive beside the server and touches nothing live. Most of the
   time somebody wants one region file back, not a whole world, and this is the level that answers it.
3. `/uxmbackup restore <id> --token <token> --passphrase <passphrase>` puts the archive back in place, within
   `restore.token-minutes` (10) of the plan.

## Five protections

| Protection | Answers |
|---|---|
| Console only | A stolen operator account: no permission grants a restore in game |
| The passphrase | Somebody who can read `config.conf`: only its hash is stored |
| No player online (levels 1 and 3, which also need the passphrase hash set) | A mistake on a live server |
| Two steps with a token | A wrong archive id |
| The old data is renamed aside, not removed | All four having failed |

None of it protects a machine somebody else has root on. It protects against a stolen account, a mistaken operator and
a rogue staff member.

## The passphrase

In the console: `/uxmbackup hash <passphrase>`. Paste the line it prints into `restore.passphrase-hash`. Without it, an
in-place restore is refused outright.

## Refusals

| Answer | When |
|---|---|
| `restore.console-only` | Run in game |
| `restore.players-online` | With the count; empty the server first |
| `restore.no-passphrase` | `passphrase-hash` is empty |
| `restore.wrong-passphrase` | The passphrase does not match |
| `restore.no-plan` | No plan for this archive; run level 1 again |
| `restore.wrong-token` | The token belongs to another archive |
| `restore.expired` | The token ran out |
| `restore.no-room` | Not enough disk for both copies |
| `restore.damaged` | The archive is not whole |
| `restore.busy` | The archive is already in flight |
| `restore.not-found` | No storage holds it |
| `restore.failed` | With the reason |

## After an in-place restore

`restore.done`, and one `restore.aside` line per item renamed aside. A database dump goes to
`restore.database-folder` and is never applied (`restore.database-hint`). Every restore and extraction is written to
`restores.log` with who ran it and what it touched.

The plan's footer says to type `restore <id> <token> <passphrase>`; the command takes them as `--token` and
`--passphrase`.

Related: [Operator commands](../commands/admin.md#restore), [config.conf](../config/config-conf.md#restore)
