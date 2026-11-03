---
title: config.conf
order: 601
description: Every key of config.conf with its default and what it does.
icon: file-cog
---

The items themselves are not here: each is a file under `items/`, and each event a file under `events/`.

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers |
| `follow-client` | `true` | A player who chose nothing reads their client's language |
| `force` | `""` | One language for everybody; empty for none |

## items

| Key | Default | Meaning |
|---|---|---|
| `default-cooldown` | `"500ms"` | The cooldown of an item whose file names none; `"0s"` for none |

## network

| Key | Default | Meaning |
|---|---|---|
| `proxy-channel` | `"BungeeCord"` | The plugin messaging channel `[server]` writes on. BungeeCord, Waterfall and Velocity all listen on it; change it only if your proxy reads somewhere else |

Related: [Configuration](index.md)
