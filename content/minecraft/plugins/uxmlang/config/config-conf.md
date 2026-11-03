---
title: config.conf
order: 601
description: Every key of config.conf, block by block, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers: they chose none, and their client reports a language this server does not have, or `follow-client` is off |
| `follow-client` | `true` | A player who chose nothing reads the language their client is set to. Off starts every player in the default |
| `force` | `""` | One language for everybody, whatever they chose and whatever their client says; empty for none |

## storage

| Key | Default | Meaning |
|---|---|---|
| `type` | `"sqlite"` | `sqlite`, `mysql`, `mariadb` or `postgresql` |
| `file` | `"languages.db"` | The SQLite file, inside `data/` |
| `host`, `port` | `"localhost"`, `0` | Port 0 is 3306 for MySQL and MariaDB, 5432 for PostgreSQL |
| `database`, `username`, `password` | `"minecraft"`, `"minecraft"`, `""` | Never put the file in git with a real password |
| `table-prefix` | `"uxmlang_"` | Before every table name |
| `pool-size` | `4` | Connections the pool may open; SQLite uses one |
| `local-only` | `false` | This one server keeps its rows in the SQLite file |
| `copy-from-file` | `false` | Copies the SQLite file's rows into an empty network database at the next start |

## network

| Key | Default | Meaning |
|---|---|---|
| `transport` | `"none"` | `none` or `redis`; the driver is in the jar |
| `server-name` | `""` | This server's id on the channel; empty makes a new one on every start |
| `redis.host` | `"127.0.0.1"` | |
| `redis.port` | `6379` | |
| `redis.user` | `""` | Empty sends the password alone |
| `redis.password` | `""` | |
| `redis.database` | `0` | |
| `redis.ssl` | `false` | |
| `redis.channel` | `"uxmlang"` | |

## effects

```hocon
effects {
  changed = [ "[message] @language.changed" ]
  already = [ "[message] @language.already" ]
  reset   = [ "[message] @language.reset" ]
}
```

Each list runs in order through uxmLib's action engine, with no limit on its length. The verbs are `[message]`,
`[broadcast]`, `[actionbar]`, `[title]`, `[subtitle]`, `[bossbar]`, `[particle]`, `[sound]`, `[effect]`, `[console]`
and `[player]`, plus `[give-money]` and `[take-money]` with Vault; each may carry `{delay=, chance=, repeat=, every=,
if=}` after the verb. Text that starts with `@` names a line of the messages files. `<language>` is the language's
name. An empty list is silence and stays empty; only a block you delete comes back.

## menu

| Key | Default |
|---|---|
| `menu.sounds.click` | `block.note_block.pling`, 0.6, 1.5 |
| `menu.sounds.page` | `item.book.page_turn`, 0.7, 1.0 |
| `menu.sounds.denied` | `block.note_block.bass`, 0.6, 0.9 |

A sound is named by its vanilla key; an empty name silences it. A button whose own actions play a sound plays that sound
alone.

Related: [Configuration](index.md)
