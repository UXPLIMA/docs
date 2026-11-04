---
title: Overview
order: 1001
description: What is stored, the tables, and sharing one database across servers.
icon: database
---

The database holds each Bedrock player's choices, the glyph slots, the items learned and the heads seen.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `uxmbedrock_` by default.

| Table | Holds |
|---|---|
| `uxmbedrock_players` | Each Bedrock player's choices: forms or chests, the features switched off, the animated pack, whether they answered the first question |
| `uxmbedrock_glyphs` | The letter each colour, face and font picture is drawn with |
| `uxmbedrock_learned_items` | The vanilla item, stack size, durability, food and equipment learned for each item model |
| `uxmbedrock_skulls` | The skin of every custom head seen |
| `uxmbedrock_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`. A player's choices then follow them, every
server draws a colour with the same letters, and the items and heads one server learned reach every build. Give each
server its own `server-name`.

## Keeping one server apart

`storage.local-only = true` keeps this one server's rows in its own SQLite file while the block stays the same as the
other servers'.

## Moving off SQLite

Set the new `type` and its connection, set `copy-from-file = true`, and restart once. The rows of the SQLite file are
copied into the empty tables and the log says how many. Tables that already hold rows are left alone.
