---
title: Installation
order: 102
description: Requirements, the license key, the first start, and the files that appear.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.21.1 to 1.21.8, or 26.2. Folia is not supported |
| Java | 21 or newer on 1.21.x; 25 or newer on 26.2 |
| Required | WorldEdit (FastAsyncWorldEdit works); schematics are read through its API |
| Database | SQLite, bundled. MySQL is optional |
| License | A uxmBuilders key from [uxplima.com](https://www.uxplima.com) |

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault or VaultUnlocked | Prices, refunds and boosts. Without an economy everything is free |
| PlaceholderAPI | The `%uxmbuilders_...%` placeholders |
| WorldGuard, GriefPrevention, Towny, PlotSquared | Native land protection checks |
| LuckPerms | Nothing specific; any permissions plugin grants the nodes |

Every one of these is a soft dependency. A protection plugin that enables after uxmBuilders is picked up
when it loads.

## Installing

1. Drop the jar into `plugins/` and start the server once. The plugin writes `config.yml` and disables
   itself, because no license key is set yet.
2. Put your key into `config.yml` under `general.licenseKey`.
3. Pick a language under `general.language`. See [Languages](../features/languages.md).
4. Start the server again. The console prints `License verified.` and the remaining files appear.
5. Put your `.schem` files into `plugins/uxmBuilders/schematics/`.
6. Create your first package, as described in [Your first build](first-build.md).

No package is configured out of the box on purpose: a package points at a schematic file that only
exists on your server.

## The files

```
plugins/uxmBuilders/
├── config.yml          performance, preview, economy, protection, rules, storage, license, language
├── messages.yml        every English message: chat, action bar, boss bar, titles, sounds
├── menu/*.yml          every English menu: titles, slots, items, names and lore
├── lang/<code>/        the same files in another language, written when it is first chosen
├── packages.yml        shop categories and packages
├── builders.yml        builders, their levels and appearance
├── schematics/         your .schem files
└── data/               the SQLite database, the license cache, pending blueprint returns
```

Each has a page under [Configuration](../config/).

<Callout type="warning" title="The plugin does not start without a valid key">

With `general.licenseKey` empty or left at `LICENSE_KEY_HERE`, the plugin logs the reason and disables
itself before touching the database. A server moved to new hardware may need its key reissued.

</Callout>

## Reloading

`/builders admin reload` re-reads `config.yml`, `messages.yml`, the menus, `packages.yml` and
`builders.yml`, clears the schematic cache and re-registers the generated permissions. Running builds are
unaffected. A language change applies on reload.

The database connection and the license do not reload; restart for those.

## First checks

| Check | How |
|---|---|
| The license is valid | The console prints `License verified.` at startup |
| The plugin loaded | `/builders` opens the main menu |
| Protection is wired | `/builders admin protection` lists each provider as on or off |
| Prices work | The shop shows a price, not `0` |
| Performance is healthy | `/builders admin perf` shows the load factor and blocks per second |
