---
title: Overview
order: 901
description: What is stored, the tables, sharing one database across servers, keeping one server apart, and moving off SQLite.
icon: database
---

The database holds what belongs to players and what staff change in game: settings, channels joined, ignores, mutes,
friends, mail, quick replies, nicks, tags owned and worn, chat colours, the chat log, staff actions, game scores and
snapshots, and the channels, tags and announcement bundles seeded from their files.

| Backend | For |
|---|---|
| [SQLite](sqlite.md) | One server. The default |
| [MySQL and MariaDB](mysql.md) | A network sharing one chat |
| [PostgreSQL](postgresql.md) | The same, on PostgreSQL |

## Tables

Every table name starts with `storage.table-prefix`, `uxmchat_` by default.

| Table | Holds |
|---|---|
| `uxmchat_players` | Every player who has played: name, account kind, last seen, the server they were on |
| `uxmchat_profile` | Each player who changed anything: settings turned off, mention style, the channel they speak in and those they hear, the rules version accepted, ladder points |
| `uxmchat_ignores` | Who ignores whom, and what |
| `uxmchat_channels` | The channels, seeded from `channels.conf` |
| `uxmchat_chat_log` | The chat log, kept `chat-log.retention-days` |
| `uxmchat_mutes` | uxmChat's own mutes |
| `uxmchat_friends`, `uxmchat_friend_requests` | Friendships, favourites and waiting requests |
| `uxmchat_mail` | Letters |
| `uxmchat_quick_replies` | Saved quick replies |
| `uxmchat_nicks` | Nicknames |
| `uxmchat_tags` | The tags, seeded from `tags.conf`, and player-made ones |
| `uxmchat_tag_worn`, `uxmchat_tag_owned`, `uxmchat_tag_favourites` | The tag each player wears, the tags given or bought, the pinned ones |
| `uxmchat_colour` | Each player's chat colour and styles |
| `uxmchat_audit` | Staff actions, read by `/chatlog audit` |
| `uxmchat_bundles` | The announcement bundles, seeded from `announcements.conf` |
| `uxmchat_game_scores` | Chat game wins and streaks |
| `uxmchat_snapshots` | `[inv]`, `[ender]` and `[shulker]` snapshots, kept `snapshots.keep-minutes` |
| `uxmchat_schema_history` | The version the tables are at |

## A network

Point every server at the same database with the same `table-prefix`, and a player's settings, friends, mail, tags,
colour and mutes follow them; channels, tags and bundles changed in game reach every server. Give each server its own
`server-name`. The database is half of a network; the other half is the proxy plugin, see
[A network](../features/network.md).

## Keeping one server apart

`local-only = true` on one server keeps its rows in its own SQLite file, whatever `type` says, under the same prefix.
Keep the storage block otherwise the same on every server.

## Moving off SQLite

1. Create the database and a user on the database server.
2. Set `type`, `host`, `port`, `database`, `username` and `password`, and `copy-from-file = true`.
3. Restart once. The rows of the SQLite file are copied into the empty tables, and the console says how many.

Tables that already hold rows are left alone, so leaving `copy-from-file` on copies nothing twice. The SQLite file
stays where it is, as a backup.

## Growth

The chat log trims itself after `chat-log.retention-days` (30), read letters after `mail.retention-days` (60), and
snapshots after `snapshots.keep-minutes` (30).

Related: [config.conf](../config/config-conf.md#storage), [SQLite](sqlite.md), [MySQL and MariaDB](mysql.md)
