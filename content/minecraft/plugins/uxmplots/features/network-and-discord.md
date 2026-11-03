---
title: Network and Discord
order: 315
description: Servers on one database hearing from each other, and webhook lines for plot and contest events.
icon: network
---

## The network

`network.conf`:

| Key | Default | Meaning |
|---|---|---|
| `transport` | `none` | `none`, `sql` (a polled table) or `redis` (a channel) |
| `server-name` | `""` | What this server calls itself; empty makes one up |
| `poll-seconds` | 2 | `sql`: how often the table is read |
| `keep-minutes` | 30 | `sql`: how long a row is kept |
| `redis-uri` | `""` | `redis://:password@host:6379/0`; a credential, never logged |
| `channel` | `uxmplots` | The channel |
| `server-stale-minutes` | 3 | How long a server's worlds are offered after it stops saying so |
| `handoff-seconds` | 60 | How long a note for a player crossing servers is meant |

What travels is what changed, never its contents: a claim, a vote, a contest moving on, a sale, a board. A lobby with no
plot world still shows the boards and announces the contest. Redis needs `io.lettuce:lettuce-core`; without it the
plugin says so once and runs on its own.

## Discord

`discord.conf`: one address and one line per event, each with `enabled`, `url` and `message`.

| Event | Tokens |
|---|---|
| `contest-announced` | `<contest>`, `<theme>`, `<opens>`, `<closes>` |
| `judging-opened` | `<contest>`, `<entries>`, `<closes>` |
| `contest-result` | `<contest>`, `<winner>`, `<place>`, `<score>` |
| `report-filed` | `<plot>`, `<reporter>`, `<reason>` |
| `report-decided` | `<plot>`, `<decision>`, `<staff>` |
| `plot-claimed` | `<plot>`, `<player>` |
| `plot-sold` | `<plot>`, `<seller>`, `<buyer>`, `<price>` |

Every address ships empty, which is off. A date is written the way Discord writes one, so each reader sees their own
time. An address is a credential: keep this file out of version control.

Related: [network.conf](../config/network-conf.md)
