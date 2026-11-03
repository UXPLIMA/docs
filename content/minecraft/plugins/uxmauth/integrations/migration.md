---
title: Moving accounts
order: 902
description: Bringing accounts over from another login plugin once, premium lists, and moving the database.
icon: arrow-right-left
---

## From another login plugin

`import` in `config.conf` brings accounts over once, at the start, before anybody can join.

| Key | Meaning |
|---|---|
| `source` | A file in `imports/`: `authme`, `nlogin`, `openlogin`, `librelogin`, `limboauth`, `jpremium`, `loginto`, `authy`, `dynamicbungeeauth`, `loginsecurity` or `authplus`, or a file of your own. Empty imports nothing |
| `jdbc-url`, `username`, `password` | The database the source keeps its accounts in; a file database by its path, such as `jdbc:sqlite:plugins/AuthMe/authme.db`. Only read, never written. A source with a file of its own, such as `authplus`, needs none |
| `table` | Another table than the file names; `jpremium` and `dynamicbungeeauth` name none, so set it |

Passwords keep their hash and move to `hashing.algorithm` at the player's next login. Authenticator secrets and premium
ids come across. The start prints how many accounts came over and how many were refused, never a password. An import runs
once; every later start says it ran and does nothing. A failed import stops the plugin with its reason, marks nothing and
runs again at the next start. On a network every server may name the same source: the first to start imports and the
others wait and find it done.

## Premium lists

`import.premium` reads a list of premium names another plugin kept, such as `fastlogin`, after the accounts, with the same
five keys. It makes no account. Every name it lists is asked of the premium sources first, because a name changes hands: a
cracked account becomes premium under its Mojang id only when they give the name that id, and it keeps its password. A
name they cannot place stays as it was, and the door asks again when its player joins.

## Moving the database

1. Fill `storage.move-to` with the target database, empty of this plugin's tables. A target that holds an account is
   refused and nothing is written.
2. Run `/uxmauth database move` from the console while nobody plays. The website table moves too when it is on.
3. Put the target's three lines in place of `storage.jdbc-url`, `username` and `password`, empty `move-to`, and restart.

To change the UUID scheme instead, see [UUID schemes](../features/premium.md#uuid-schemes).

Related: [Import files](../config/import-files.md), [Database](../database/overview.md)
