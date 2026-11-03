---
title: Installation
order: 202
description: Requirements, the first start, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmMinions after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; a minion's action runs on the region that owns its block |

uxmLib is shaded into the jar, so the plugin needs nothing else on the server. Sales and costs are in the player's own
experience until `economy.currency` names an economy.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | The balance sales pay into and costs are taken from |
| uxmShop | A price source for the seller |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and placeholders in requirements |
| Nineteen claim plugins | Where a minion may stand and what it may touch; an island as the limit scope |
| uxmLang | One language choice for every UXM plugin |

The full list is on [Integrations](../integrations/overview.md).

## Installing

1. Drop `uxmMinions-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The plugin writes its folder on the first boot, with ten example types under `types/`.
3. Check `economy`, `prices` and `limits` in `config.conf`, then run `/uxmminions reload`.
4. Hand out a minion with `/uxmminions give <player> <type> [level]`. The player places the item to put it in the
   world.

## The files

```
plugins/uxmMinions/
├── config.conf            language, storage, tick, render, placeholders, fuel switch, experience, destinations,
│                          prices, economy, banking, loops, smelting, upgrades, limits, claims, teleport, ledger,
│                          audit, effects, editor, menus
├── hologram.conf          the hologram over a minion
├── tool.conf              tool wear and tool enchantments
├── fuel.conf              the fuel grades
├── prices.conf            the plugin's own price table for the seller
├── skins.conf             the suits a minion may wear
├── commands.conf          the names, aliases and switches of both commands
├── types/                 one file per type: collector, farmer, feeder, fisher, healer, lumberjack, miner,
│                          seller, slayer, spawner
├── menus/                 fourteen window files
├── messages/              messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf         the language each player chose, when uxmLang is not installed
├── data/minions.db        the SQLite file
└── backups/               the last five copies of a file a release added keys to
plugins/uxmTheme/
└── theme.conf             the colours and glyphs every UXM plugin reads
```

The template the editor starts a new type from, `editor/new-type.conf`, lives in the jar. A `hologram` or `tool` block
an earlier release kept in `config.conf` moves to its own file on the first start.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into `config.conf`,
`hologram.conf` or `tool.conf` with its comment and never changes a value you wrote; your file as it was is kept in
`backups/`. A quota you removed from `limits.per-type` is not written back.

## Checking the install

`/uxmminions doctor` (permission `uxmminions.admin.doctor`):

| Check | ok | warn or fail |
|---|---|---|
| `storage` | Where the rows are kept | fail: no minion, stock or bank can be read or written |
| `types` | How many types were read | fail: none, so nothing can be placed; warn: some files were refused |
| `economy` | An economy answered | warn: none, so a seller pays nothing |
| `claims` | A claim plugin answered, or none is installed | Always ok |
| `windows` | How many windows were read | fail: none |
| `placeholders` | Published | warn: PlaceholderAPI is not installed |

## Troubleshooting

- **A minion stands still.** Its panel and `%uxmminions_minion_<id>_why%` say why: full, no fuel, owner offline,
  blocked, no water, or a job nobody registered.
- **The seller does not sell an item.** No price source knows it; add it to `prices.conf`.
- **A type fails to load.** Its levels have a gap, a level improves nothing, or `job` is unknown; the console names the
  file.
- **Players cannot place more.** A limit is reached; `/uxmminions slots` grants more.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34250). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
