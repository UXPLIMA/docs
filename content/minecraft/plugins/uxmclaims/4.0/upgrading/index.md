---
title: Upgrading
order: 120
description: Moving a uxmClaims 3.x server to 4.0, and going back if you have to.
icon: arrow-up-circle
---

4.0 converts a 3.x plugin folder and database on its first start. Nothing is copied by hand: you swap the
jar, and the 4.0 jar finds the 3.x data, backs it up, converts the files and copies the database. Read the
pages in this order.

| Page | What it covers |
|---|---|
| [From 3.x](from-3x.md) | The checks to run on 3.x, what the first 4.0 start does, the report, failures and several servers |
| [Migration commands](migration-commands.md) | Every `/uxm migrate` and `/uxm db` command |
| [Rollback](rollback.md) | Going back to 3.x with `/uxm migrate restore-v3`, and removing the 3.x leftovers |
| [Behaviour changes](behaviour-changes.md) | What works differently in 4.0 on purpose: admin bypass, commands, files, menus |

Only 3.7.x data is converted. A server on 3.4, 3.5 or 3.6 installs 3.7.x first, starts it once, stops it,
and then installs 4.0.
