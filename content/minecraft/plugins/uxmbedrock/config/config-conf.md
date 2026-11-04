---
title: config.conf
order: 601
description: Every key of config.conf with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers, and the fallback for a key a translation has not reached |
| `follow-client` | `true` | A player who chose nothing reads the language their client is set to |
| `force` | `""` | One language for everybody; empty for none |

## Server

| Key | Default | Meaning |
|---|---|---|
| `server-name` | `"server"` | This server's id on the network. On a network, its name in `velocity.toml`, in lower case. Letters, digits, `_` and `-` |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql`. A network needs one of the last three, so choices and glyph slots are shared |
| `file` | `"bedrock.db"` | The SQLite file, inside `data/` |
| `host` | `"localhost"` | The database server |
| `port` | `0` | `0` is the server's own: 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database` | `"minecraft"` | The database name |
| `username` | `"minecraft"` | The user |
| `password` | `""` | The password. Never put this file in git with a real one |
| `table-prefix` | `"uxmbedrock_"` | Written in front of every table name |
| `pool-size` | `4` | Connections the pool may open; SQLite uses one |
| `local-only` | `false` | This one server keeps its own rows in the SQLite file, whatever the block says |
| `copy-from-file` | `false` | Copy the SQLite file's rows into a new network database at the next start |

See [Database](../database/overview.md).

## network

| Key | Default | Meaning |
|---|---|---|
| `mode` | `"none"` | `none`: alone, or Geyser reads this server's folder. `direct`: to uxmBedrock-Velocity's broker. `redis`: through Redis |
| `host` | `"127.0.0.1"` | Where the proxy's broker listens |
| `port` | `25591` | The broker's port |
| `secret` | `""` | This server's secret from the proxy's `secrets.conf`. Never put it in git |
| `redis-uri` | `"redis://127.0.0.1:6379"` | The Redis server, for `redis` |

See [A network](../features/network.md).

## modules

One switch per feature. Off changes nothing for Java players; for Bedrock players that part looks the way Geyser draws
it.

| Key | Default | Off means |
|---|---|---|
| `java-font` | `true` | Bedrock's own font |
| `exact-colours` | `true` | Each colour becomes the nearest of Bedrock's sixteen |
| `safe-placeholder` | `true` | `%uxmbedrock_safe_...%` answers nothing |
| `emoji` | `true` | The pictures of other plugins' fonts are left out |
| `faces` | `true` | Faces are left out |
| `tab-list` | `true` | Geyser's plain player list |
| `java-chat` | `true` | Bedrock's chat layout |
| `scoreboard` | `true` | Geyser's sidebar, with score numbers |
| `scoreboard-art` | `true` | The plain sidebar |
| `name-tags` | `true` | Geyser's name tags |
| `titles` | `true` | Geyser's titles, action bar and boss bars |
| `holograms` | `true` | Geyser's text displays |
| `displays` | `true` | Bedrock shows nothing for item and block displays |
| `skulls` | `true` | Custom heads show as Steve |
| `tap-words` | `true` | Click stays click for touch and controller players |
| `menu-forms` | `true` | Other plugins' menus are always chests |
| `menu-art` | `true` | Plain chests, no menu pictures |
| `sign-forms` | `true` | Bedrock's sign editor |
| `anvil-forms` | `true` | The anvil |
| `command-completion` | `true` | Only command names complete |
| `name-finder` | `true` | Names are taken as typed |
| `placeholders` | `true` | PlaceholderAPI has none from this plugin |
| `converter` | `true` | No pack is built: vanilla textures, and the pictures in text are left out |
| `tooltips` | `true` | Bedrock's tooltips |
| `vanilla-hud` | `true` | Bedrock's HUD |
| `hud-plugins` | `true` | HUD plugins' boss bars show as bars |
| `animated-pack` | `true` | First frames only, and nobody is asked |
| `preview-page` | `false` | No preview page; on opens a port |

## player-choices

| Key | Default | Meaning |
|---|---|---|
| `allowed` | `["java-font", "java-chat", "menu-art", "vanilla-hud", "hud-plugins", "animated-pack"]` | The features a Bedrock player may switch off in `/bedrock`. Only these may be listed: `java-font`, `exact-colours`, `faces`, `tab-list`, `java-chat`, `scoreboard-art`, `displays`, `menu-forms`, `menu-art`, `tooltips`, `vanilla-hud`, `hud-plugins`, `animated-pack` |

## preview

| Key | Default | Meaning |
|---|---|---|
| `host` | `"127.0.0.1"` | The address the preview page listens on |
| `port` | `8170` | Its port; `0` for any free one |
| `address` | `""` | The address the link names, when the page is reached through another name |
