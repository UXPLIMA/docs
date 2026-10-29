---
title: Installation
order: 11
description: Requirements, the license key, and the files the plugin creates.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.21.11 or newer. Folia is supported |
| Java | 21 or newer |
| Optional | PlaceholderAPI, for the [placeholders](../placeholders.md) and for placeholders in rewards |
| Optional | MySQL or MariaDB, instead of the built-in SQLite file |
| Network only | MySQL or MariaDB, and Redis 5.0 or newer (6.2 or newer recommended) |

## First run

1. Drop the jar into `plugins/` and start the server. The files are written to
   `plugins/uxmSupplyRush/`.
2. Stop the server, open `config.yml`, and set `license-key` to the key from your UXPLIMA
   purchase. Without a valid key the plugin turns itself off.
3. Pick the language players see with `language`. See [Languages and menus](../config/languages.md).
4. Start the server and type `/sr`. The tournament menu opens.

Then follow [Your first event](first-event.md).

## MySQL

SQLite needs no setup and is right for a single server. To use MySQL or MariaDB instead, create the
database with full character support, so event and player names keep every character:

```sql
CREATE DATABASE supplyrush CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Then fill in the `database` section of `config.yml`. See [Database](../config/database.md).

## Files

```
plugins/uxmSupplyRush/
├── config.yml              license, language, database, network
├── general-settings.yml    tournament rules
├── auto-start.yml          the weekly schedule
├── events/                 one file per event: rewards and announcements
├── languages/              messages and menus, one folder per language
├── history/                exported summaries and run logs
└── data/                   files the plugin keeps for itself
```

Leave `data/` alone. If you copy this folder to set up a new server, delete
`data/server-id` in the copy before starting it.
