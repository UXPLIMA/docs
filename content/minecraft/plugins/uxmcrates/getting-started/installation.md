---
title: Installation
order: 202
description: Requirements, the optional plugins and what each adds, the first start, and the files that appear.
icon: download
---

uxmCrates is one jar with uxmLib inside it. Put it in `plugins/` and start: the two example crates, their keys, a
shared reward, an envoy and fourteen reveals are written for you. This manual describes uxmCrates after 0.1.0:
German, Spanish, French, Portuguese and Russian arrive with the next release, and 0.1.0 itself reads English and
Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; any fork of Paper, such as Purpur or Leaf |
| Database | none for one server: SQLite is built in. MySQL, MariaDB or PostgreSQL to share keys and limits across a network |

The MariaDB and PostgreSQL drivers are downloaded into the server's libraries folder on the first start, so the first
start needs internet access.

## Optional plugins

uxmCrates needs no other plugin. Each of these is used when it is installed and ignored when it is not.

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and other plugins' placeholders in a requirement line |
| Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury | A crate cost and a reward's money in that economy. See [Economies](../integrations/economies.md) |
| BentoBox, ExcellentClaims, Factions, GriefDefender, GriefPrevention, Homestead, HuskClaims, HuskTowns, Kingdoms, Lands, PlotSquared, RClaim, Residence, SimpleClaimSystem, SuperiorSkyblock2, Towny, uxmClaims, WorldGuard or XClaim | The [claim rules](../features/claims.md) for placing and opening crates |
| Oraxen, Nexo, ItemsAdder or CraftEngine | [Custom items](../integrations/custom-items.md) named in a content file |
| mcMMO | A skill level in a requirement line |
| Geyser or Floodgate | Every window as a Bedrock form, and a window reveal instead of a world one for Bedrock players |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Put `uxmCrates-<version>.jar` in `plugins/` and remove any older uxmCrates jar.
2. Start the server. The plugin writes its files into `plugins/uxmCrates/`.
3. Look at a block and run `/uxmcrates place common`. The block becomes the common crate, with a hologram over it.
4. Run `/uxmcrates key give <your name> common-key 3`, then right click the crate.

## The files

```
plugins/uxmCrates/
├── config.conf       language, storage, odds, pushback, claims, log, boards, rerolls, milestones, luck, purge, editor
├── opening.conf      who sees a reveal, skipping, the bulk cap, the reveal window panes, the preview wait
├── economy.conf      Treasury's wait, placeholder currencies and economy bridges
├── commands.conf     the name, aliases and switch of every command and subcommand
├── crates/           common.conf, elite.conf: one file per crate
├── keys/             common-key.conf, elite-key.conf: one file per key
├── rewards/          legendary-sword.conf: a reward written once and shared by crates
├── envoys/           supply-drop.conf: one file per envoy
├── animations/       fourteen reveals, one file each
├── editor/           new-crate.conf, new-key.conf: what the editor writes for a new crate or key
├── menus/            one file per window
├── messages/         messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── languages.conf    the language each player chose, when uxmLang is not installed
├── data/             crates.db while storage.type is sqlite, and logs/ with the opening log file
├── migrations/       the reports /uxmcrates migrate writes
└── backups/          the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf        the colours and glyphs every UXM plugin reads, written by the first one installed
```

The example crates, keys, rewards, envoys and animations are written only when their folder does not exist yet. Once
the folder is there it is yours: a file you delete stays deleted. A window file you delete is written back at the next
start.

## Updating

Stop the server, replace the jar and start it. A release that adds a setting writes it into your `config.conf`,
`opening.conf` or `economy.conf` with the comment that explains it, and never changes a value you set; the file as it
was goes to `backups/`. A block an earlier release kept in `config.conf` moves to `opening.conf` or `economy.conf` on
the first start. The tables are moved forward on their own. See
[What an update does](../config/index.md#what-an-update-does).

## Checking the install

`/uxmcrates doctor` (permission `uxmcrates.admin.doctor`) answers one line per check:

| Check | Says |
|---|---|
| `storage` | The backend, the file or `host:port/database`, the prefix and the schema version; fail when the database did not answer |
| `content` | How many crates, keys, rewards, envoys and animations were read, and how many lines were refused |
| `currencies` | The economies a crate names and whether this server runs them |
| `claims` | Which claim plugins answer |
| `windows` | How many window files were read |
| `placeholders` | `published`, or a warning that PlaceholderAPI is not on the server |

A line the plugin cannot read is named in the console with its file and key, and the rest of the file still loads.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34239). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
