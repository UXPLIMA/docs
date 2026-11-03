---
title: "Guide"
order: 10
description: "How uxmCrates works, how to set it up and what each file holds."
---

## Install

Put `uxmCrates-<version>.jar` in `plugins/` and start the server. uxmLib is inside the jar, so nothing else is
required. The plugin runs on Paper 26.2 or newer and on Folia.

These plugins are used when they are installed and ignored when they are not:

| Plugin | Used for |
|---|---|
| PlaceholderAPI | The `%uxmcrates_...%` placeholders. |
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | Paying a crate's cost or a reward's money. |
| Floodgate, Geyser | Bedrock players get a native form for every window and a window reveal instead of a world one. |
| BentoBox, ExcellentClaims, Factions, GriefDefender, GriefPrevention, Homestead, HuskClaims, HuskTowns, Kingdoms, Lands, PlotSquared, RClaim, Residence, SimpleClaimSystem, SuperiorSkyblock2, Towny, uxmClaims, WorldGuard, XClaim | The `claims` rules for placing and opening crates. |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Custom items named in a content file. |
| mcMMO | A skill level in a requirement line. |

## Files

The plugin writes these into `plugins/uxmCrates/` on first start:

| File or folder | What it holds |
|---|---|
| `config.conf` | Language, storage, odds display, pushback, claims, log, rerolls, milestones, luck, purge, retention, link tool, editor, menu sounds. |
| `opening.conf`, `economy.conf` | The opening rules and the economy, each under its own block name. A block an earlier release kept in `config.conf` moves here on the first start. |
| `commands.conf` | The name, aliases and on/off switch of every command and subcommand. |
| `messages/messages_<lang>.conf` | The text of every message, one file per language. English and Turkish ship. |
| `menus/*.conf` | One file per window the plugin draws. |
| `crates/<id>.conf` | One crate each. |
| `keys/<id>.conf` | One key each. |
| `rewards/<id>.conf` | A reward written once and shared by crates. |
| `envoys/<id>.conf` | One envoy each. |
| `animations/<id>.conf` | One reveal each. |
| `data/crates.db` | The SQLite database, when `storage.type` is `sqlite`. |
| `languages.conf` | The language each player chose. |
| `logs/` | The opening log file, when `log.to-file` is on. |
| `migrations/` | The reports `/uxmcrates migrate` writes. |

The colours are in `plugins/uxmTheme/theme.conf`, shared by every UXPLIMA plugin on the server.

The example crates, keys, rewards, envoys and animations are written only when their folder does not exist, so
an example that is deleted stays deleted. The menus are written again whenever a file is missing.

`/uxmcrates reload` reads every file again. A reload is applied as a whole or not at all: a file that does not
parse leaves the running configuration as it was. Command names in `commands.conf` need a restart. When a
release adds a key to `config.conf`, the next start adds it with its comment and keeps every value already
written; the previous file is kept as `config.conf.bak`.

`/uxmcrates doctor` reports one line per check: storage, content, currencies, claims, windows and placeholders.

## Storage

`storage.type` is `sqlite`, `mysql`, `mariadb` or `postgresql`. SQLite needs no setup and suits one server. A
database server is shared by every server pointed at it, which is what makes a limited reward ("three across
the whole server") and key balances network-wide. The MariaDB and PostgreSQL drivers are fetched into the
server's libraries folder on first start.

`storage.table-prefix` lets two copies share a database. `storage.local-only` keeps one server of a network on
its SQLite file. To move from the SQLite file to a database server, set `storage.copy-from-file` to `true` with
the new type and restart once: the rows are copied into the empty tables and the log says how many.

`purge` forgets players who stopped coming and `retention` trims the opening log and the key ledger. Both ship
off. A player who is owed a pending reward is never purged.

## Languages

A player reads the language they chose with `/uxmcrates lang set <language>`, else their client's language
when `language.follow-client` is on, else `language.default`. `language.force` puts everybody on one language.
A new language is one more file in `messages/`.

## The editor

`/uxmcrates editor` opens a window to build crates, keys and rewards without leaving the game. It writes the
same content files an operator edits by hand and never writes `config.conf` or a menu file. Set
`editor.enabled = false` to switch it off: its windows are not registered and the command says it is off.
