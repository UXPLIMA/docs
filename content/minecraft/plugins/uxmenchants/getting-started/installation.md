---
title: Installation
order: 202
description: Requirements, the first start, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmEnchants after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia; every effect runs on the scheduler that owns what it touches |

uxmLib is shaded into the jar, so the plugin needs nothing else on the server.

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and placeholders in conditions |
| Vault | `money` conditions and `[take-money]` actions |
| Nineteen claim plugins | Block effects respect claims; see [Protection](../features/protection.md) |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Their items in item lists: `"oraxen:ruby_sword"` |
| EcoEnchants | A conflict may name one of its enchantments: `"ecoenchants:telekinesis"` |
| mcMMO | Skill levels in conditions, read without PlaceholderAPI |
| uxmLang | One language choice for every UXM plugin |

The full list is on [Integrations](../integrations/overview.md).

## Installing

1. Drop `uxmEnchants-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server. The plugin writes its folder on the first boot, including twelve example enchantments.
3. Edit or add files under `enchantments/`, then run `/uxmenchants reload`.

When any content file is wrong, the reload is refused as a whole: the message names the file, the key and the line, and
the server keeps the content it had. At start a broken set is reported in the console and the server starts with no
enchantments.

## The files

```
plugins/uxmEnchants/
├── config.conf            language, storage, item data, limit, routes, anvil, books, display, charges, claims, editor
├── commands.conf          the name, aliases and switch of both commands and each subcommand
├── rarities.conf          common, rare, legendary: the chance on each route and the table level
├── types.conf             normal and curse: the format, the per item limit, the grindstone rule
├── targets.conf           the item lists, the named item sets, the slots
├── vanilla.conf           how vanilla enchantments are drawn beside ours
├── enchantments/          one file per enchantment; the file name is the id
├── menus/                 wiki.conf, grouped.conf, info.conf, editor.conf, editor-list.conf, editor-enchant.conf
├── messages/              messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf         the language each player chose, when uxmLang is not installed
├── data/data.db           the SQLite file
└── backups/               the last five copies of config.conf when a release added keys
plugins/uxmTheme/
└── theme.conf             the colours and glyphs every UXM plugin reads
```

The template the in-game editor starts a new enchantment from, `editor/new-enchantment.conf`, lives in the jar and is
never written into the folder.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/`. The other files are written only
when they are missing, so a shipped enchantment you deleted comes back on the next start: switch it off by closing its
routes instead, or keep the file and change it.

## Checking the install

`/uxmenchants doctor` (permission `uxmenchants.admin.doctor`):

| Check | ok | warn or fail |
|---|---|---|
| `storage` | Where the rows are kept | fail: the storage did not answer, so no preference and no cooldown survives a restart |
| `enchantments` | How many enchantments were read | fail: none, so the anvil, the table and the books offer nothing |
| `other enchantments` | Whether another enchantment plugin answered | Always ok; it says a conflict naming one matches nothing |
| `placeholders` | The expansion is registered | warn: PlaceholderAPI is not installed, or it loaded after us |
| `claims` | A claim plugin answered, or none is installed | Always ok |
| `windows` | How many windows were read | fail: none, so nothing this plugin draws can open |

## Troubleshooting

- **A reload is refused.** The message and the console name the file, the key and the line; the old content stays
  loaded.
- **An enchantment is struck through on an item.** The player lacks its node, so it does not fire for them.
- **The table never offers an enchantment.** The player lacks its `.table` node (see
  [Acquisition](../features/acquisition.md#the-enchanting-table)), its rarity's `table-chance` is `0`, the table level
  is below the rarity's `minimum-level`, the item is not in its `table-items`, or its `obtaining.table` is off.
- **Too many enchantments come out of one table roll.** Lower `acquisition.table.cap` or raise its `reduction`.
- **"The item is full".** `/enchants limit` prints the whole calculation.

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34242). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
