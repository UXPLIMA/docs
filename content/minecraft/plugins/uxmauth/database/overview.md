---
title: Overview
order: 1001
description: What is stored, the tables, sharing one database across a network, and moving.
icon: database
---

Everything the door knows is in one database, so every member of a network reads it alike.

## Tables

| Table | Holds |
|---|---|
| `uxmauth_account` | One row per account: the name as first written, the name in lower case (its unique index decides a race between two registrations), the kind, the UUID, the hash, dates |
| `uxmauth_meta` | Facts the plugin keeps about itself, such as the UUID scheme of the first start |
| `uxmauth_rename` | Accounts moved to a marked name by a conflict |
| `uxmauth_dispute` | Names a cracked holder and a paid owner both want, and those staff settled |
| `uxmauth_address` | What each address did wrong: failures, ban step, banned until |
| `uxmauth_session` | Remembered sessions |
| `uxmauth_session_choice` | What each account chose, where players choose |
| `uxmauth_factor` | App secrets, sealed with AES-GCM, and the last step a code was taken for |
| `uxmauth_backup_code` | Backup codes, hashed as passwords are |
| `uxmauth_email` | Confirmed addresses, and whether the login code is mailed |
| `uxmauth_mail_code` | Codes mailed for confirming, logging in or recovering, hashed |
| `uxmauth_login` | Every login: when, from where, how; kept `history.keep-days` |
| `uxmauth_last_server` | The server each player was on when they last left the network |
| `uxmauth_website_link` | Which website-table rows are known to be an account's |

## A network

Point the proxy and every server at the same database. Accounts, sessions, bans, factors and the history are then the
same everywhere; see [A network](../features/network.md).

## Moving

`/uxmauth database move` copies every account to `storage.move-to`; see
[Moving accounts](../integrations/migration.md#moving-the-database).

## Backing up

Back up the database with `data/second-factor.key` on a lone server: without the key no sealed app secret opens.

Related: [Backends](backends.md)
