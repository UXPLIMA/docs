---
title: config.conf
order: 501
description: Every key of config.conf, block by block, with its default and what it does.
icon: file-cog
---

## language

| Key | Default | Meaning |
|---|---|---|
| `default` | `"en"` | What a player reads when nothing else answers |
| `follow-client` | `true` | A player who chose nothing reads their client's language |
| `force` | `""` | One language for everybody; empty for none |

## this-lobby

`this-lobby = "lobby-1"`: the id of the lobby this server is, from `lobbies.conf`. Empty on a server that is not a
lobby.

## status

| Key | Default | Meaning |
|---|---|---|
| `source` | `"proxy"` | `proxy`, `ping` or `redis` |
| `interval-ms` | `5000` | How often the counts refresh |
| `timeout-ms` | `1500` | How long a ping waits |
| `stale-after-ms` | `15000` | How long an answer is trusted; `0` for ever |
| `redis.uri` | `"redis://127.0.0.1:6379"` | Only for `redis` |
| `redis.channel` | `"uxm:multilobby:status"` | Only for `redis` |

See [Live counts](../features/live-counts.md).

## connect

| Key | Default | Meaning |
|---|---|---|
| `cooldown-ms` | `3000` | Between two moves; `0` turns it off |

## menu

| Key | Default | Meaning |
|---|---|---|
| `sounds.click` | `block.note_block.pling`, 0.6, 1.5 | A button that does something |
| `sounds.page` | `item.book.page_turn`, 0.7, 1.0 | A page arrow |
| `sounds.denied` | `block.note_block.bass`, 0.6, 0.9 | A button the player may not use |
| `hide-locked` | `false` | Leave out a lobby the player may not enter |
| `busy-at-percent` | `50` | When a lobby counts as busy |
| `load-icons` | blue, lime, yellow, red, gray candles | The material of each load |

## tool

The hotbar item; every key is in [The hotbar tool](../features/tool.md).

## effects

One list per outcome: `connecting`, `already-there`, `unreachable`, `full`, `not-permitted`, `cooldown`,
`no-such-lobby`, `nothing-available`. The verbs and placeholders are in [Outcomes](../features/outcomes.md).

Related: [Configuration](index.md)
