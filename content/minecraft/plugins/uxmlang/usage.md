---
title: "Guide"
order: 10
description: "How uxmLang works, how to set it up and what each file holds."
---

uxmLang keeps one language per player for the whole server, and for a network that shares a database. It
registers the language service uxmLib reads, so every UXM plugin on the server uses the language a player chose
here. Players pick it with `/lang` or in the language window.

## Install

Put `uxmLang-<version>.jar` in `plugins/` and start the server. The first start writes `config.conf`,
`commands.conf`, `languages.conf`, `theme.conf`, `menus/language.conf` and the message files into
`plugins/uxmLang/`. `/uxmlang reload` reads them again.

## Which language a player reads

In this order: the language in `language.force`, the one the player chose, the one their client reports while
`language.follow-client` is on, and `language.default`. A player who runs `/lang reset` follows their client again.

The languages on offer are the message files in `messages/`. Adding a file adds the language. `languages.conf`
names them the way their readers write them, such as `tr = "Türkçe"`; a language not named there is named by the
server. Its `icons` block gives each language the icon `/lang` draws it with, a material or a head written as
`basehead:<value>`; the shipped file draws each language with a flag, and a language with no icon takes `default`.

## A network

The choice is a row in the database, read while the player logs in, so every server on the same database knows it
before the player's first message. With `network.transport = "redis"` a change is also published at once, so a
server the player is already on switches without waiting for the next login. A change for a player who is on
another server is skipped there and read from the row when they arrive. A lost message costs nothing, because the
row is the truth. The client language a player reports is written only when it changes.

## Configuration

| Key | Default | Meaning |
|---|---|---|
| `language.default` | `"en"` | The language of a player nothing else decides for. |
| `language.follow-client` | `true` | Follows the client of a player who chose nothing. |
| `language.force` | `""` | One language for everybody. Empty for none. |
| `storage` | SQLite | Where choices are kept. Point every server at one database to share them. |
| `network.transport` | `"none"` | `none` for one server, `redis` for a network. |
| `network.server-name` | `""` | This server's name on the channel. Empty makes a new one on every start. |
| `network.redis` | localhost | `host`, `port`, `user`, `password`, `database`, `ssl` and `channel`. |
| `effects.changed`, `already`, `reset` | a message | What choosing a language does, as action lines. `<language>` is its name. |

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/uxmlang` |  | Administer the plugin. |
| `/uxmlang reload` | `uxmlang.admin.reload` | Read the configuration and the content again. |
| `/uxmlang version` | `uxmlang.admin.version` | Read the version this server runs. |
| `/uxmlang get <player>` | `uxmlang.admin.get` | Say which language a player reads. |
| `/uxmlang set <player> <language>` | `uxmlang.admin.set` | Choose for a player, the way they would choose for themselves. |
| `/uxmlang reset <player>` | `uxmlang.admin.set` | Drop a player's choice. |
| `/uxmlang doctor` | `uxmlang.admin.doctor` | Say how this plugin is, in one line per thing that can be wrong. |
| `/lang` | `uxmlang.use` | Choose the language you read this server in. |
| `/lang list` |  | Say what this player reads now, and what else there is. Players only. |
| `/lang set <language>` |  | Read the server in one language, whatever the client is set to. Players only. |
| `/lang reset` |  | Follow the client again, as a player who never chose anything does. Players only. |

All nodes and their defaults are in [reference.md](reference.md).

## Examples

**A Turkish server.** Set `language.force = "tr"`.

**Players choose, nothing follows the client.** Set `language.follow-client = false` and keep `default = "en"`.

**A network.** Point `storage` of every server at one MariaDB, set `network.transport = "redis"` and the same
`network.redis` block everywhere.
