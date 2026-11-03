---
title: Installation
order: 202
description: Which jar goes where, requirements, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmMOTD after 0.0.2: German, Spanish, French, Portuguese and Russian arrive with the next release,
and 0.0.2 itself reads English and Turkish.

## Which jar

| Setup | Jar | Where |
|---|---|---|
| One server | `uxmMOTD-paper-<version>.jar` | The server's `plugins/` |
| A network | `uxmMOTD-velocity-<version>.jar` | The proxy's `plugins/` |

On a network the proxy answers the ping, so the servers behind it are never asked and need no jar.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Paper | 26.3 or newer, or Folia |
| Velocity | 4 |

uxmLib is inside each jar, so nothing else needs to be installed.

## The files

The folder is `plugins/uxmMOTD/` on Paper and `plugins/uxmmotd/` on Velocity. The files are the same on both.

```
plugins/uxmMOTD/
├── config.conf        language, entries, player count, hover, version text, centring, icons, maintenance
├── commands.conf      the name, aliases and switch of the command and each word under it
├── messages/          messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── icons/             uxm-mark.png and uxm-glow.png, and any 64 by 64 PNG you add
├── maintenance.conf   whether maintenance is on; written by the command
└── backups/           the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf         the colours and glyphs every UXM plugin reads
```

Do not edit `maintenance.conf` while the server runs; use the command.

## Updating

Stop the server or proxy, replace the jar and start it. A release that adds a key writes it into your `config.conf`
with its comment and never changes a value you wrote; your file as it was is kept in `backups/`.

## Checking the install

`/uxmmotd doctor` (permission `uxmmotd.admin.doctor`):

| Check | ok | fail |
|---|---|---|
| `content` | How many entries were read | None: a ping is answered with nothing |
| `icons` | How many of the named icons could be read; or none named, so a ping carries the server's own picture | None of the named icons could be read, or only some: a ping that asks for a broken one falls back |

The console carries the whole reason for anything that went wrong at start.

## Metrics

The Paper jar reports to bStats as plugin 34251 and the Velocity jar as 34262. Turn it off in `plugins/bStats/`
(`config.yml` on Paper, `config.txt` on Velocity).

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
