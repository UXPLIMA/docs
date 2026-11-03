---
title: Installation
order: 202
description: The jars, requirements, the first start, the files that appear, a start that fails, updating and the doctor.
icon: download
---

This manual describes uxmAuth after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next release,
and 0.1.0 itself reads English and Turkish.

## The jars

| Setup | Jar | Where |
|---|---|---|
| One server | `uxmAuth-paper-<version>.jar` | The server's `plugins/` |
| A network | `uxmAuth-velocity-<version>.jar` and `uxmAuth-paper-<version>.jar` | The proxy, and every server behind it; see [A network](../features/network.md) |

uxmLib and every JDBC driver are inside each jar.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia, in offline mode for the handshake to run on a lone server |
| Proxy | Velocity, with LimboAPI 1.1 or 2 for the proxy's own waiting world |
| Database | None for one server: a SQLite file. MySQL, MariaDB, PostgreSQL or H2 by JDBC URL |

## Optional plugins

| Plugin | Adds |
|---|---|
| Floodgate, Geyser | Bedrock players, native forms |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and placeholders in effects |
| ViaVersion | Older clients; a client older than 1.21.6 cannot show the dialog and waits frozen |
| Vault | Money in effect lines |
| LimboAPI (Velocity) | The proxy's own waiting world |

## Installing

1. Put the Paper jar in `plugins/` and start the server. The first start writes the files below into
   `plugins/uxmAuth/`.
2. Edit `config.conf`, then `/uxmauth reload`. A broken edit leaves the running configuration as it was and the console
   says why.
3. Run `/uxmauth doctor`.

A lone server needs nothing else. The default database is the SQLite file `plugins/uxmAuth/data/auth.db`.

## The files

```
plugins/uxmAuth/
├── config.conf          every setting but the four below
├── protection.conf      sessions aside, what protects the door: filters, bans, limits, countries, captcha
├── ownership.conf       who owns a name, UUID schemes, conflicts
├── effects.conf         what happens at each moment
├── network.conf         the network
├── screens.conf         where a waiting player stands, and every dialog
├── commands.conf        names, aliases and switches, with Turkish words
├── messages/            messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── mail/                <kind>_<language>.txt: login-code, recovery, verify
├── menus/info.conf      the staff window of /uxmauth info
├── passwords/common.txt passwords too common to accept
├── imports/             how each supported login plugin keeps its accounts
├── data/                auth.db, second-factor.key, GeoLite2-Country.mmdb
└── backups/             the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf           the colours and glyphs every UXM plugin reads
```

A block an earlier release kept in `config.conf` moves to its own file on the first start.

<Callout type="warning" title="Back up second-factor.key">

A lone server seals every authenticator secret with `data/second-factor.key`. Lose the file and no app code works
again; every player with the app has to be reset with `/uxmauth disable2fa`. Keep it with your database backups. On a
network the network secret seals them instead.

</Callout>

## A start that fails

A start that fails does not leave the server open: the plugin stays enabled with only a door that refuses every login,
and the console names the cause. Fix it and restart.

## Settings that need a restart

The `hashing` block, `lane.threads` and `lane.max-threads`, `mail.threads`, `website-table`, the `storage` URL, and
`commands.conf`. Everything else applies at `/uxmauth reload`.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your file with its comment and
never changes a value you wrote; your file as it was is kept in `backups/`. On a network, update the proxy and every
server together.

## Checking the install

`/uxmauth doctor` (permission `uxmauth.admin.doctor`):

| Check | ok | fail or warn |
|---|---|---|
| `languages` | How many language files were found | fail: none, every line is the code's default |
| `storage` | The database answered | fail: nobody can register or log in |
| `windows` | How many window files were read | fail: none |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server |

The premium sources and mail are not doctor checks: a source that fails passes the name to the next and the console
says so; a mail that fails is told to the player and once to the console.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
