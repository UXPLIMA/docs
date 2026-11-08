---
title: Installation
order: 202
description: Requirements, the right jar, the license key, the first start, and the files that appear.
icon: download
---

uxmBuilders ships as two jars. Pick the one that matches the server, start once to get the files, add
the license key and start again.

<Callout type="warning" title="Coming from 1.0.0?">

2.0.0 does not read anything 1.0.0 left behind: builds, crews, warehouses, trust lists and the
configuration all start fresh. Read [Upgrading from 1.0.0](upgrading.md) before replacing the jar.

</Callout>

## Requirements

| | |
|---|---|
| Server | Paper or Folia, 1.21.1 to 1.21.8, or 26.2. Any other version is refused at startup with the list of supported ones |
| Jar | `uxmBuilders-paper-<version>.jar` on Paper, `uxmBuilders-folia-<version>.jar` on Folia |
| Java | 21 or newer on 1.21.x; 25 or newer on 26.2 |
| Database | SQLite, nothing to set up. MySQL 8+, MariaDB or PostgreSQL 13+ optional, see [The database](../config/database.md) |
| License | A uxmBuilders key from [uxplima.com](https://www.uxplima.com) |
| Internet on first start | The plugin downloads its storage libraries (jOOQ, HikariCP, the MySQL and PostgreSQL drivers) from Maven Central into `plugins/uxmBuilders/libs/` |

The Paper jar declares `folia-supported: false`, so Folia refuses it. Use the Folia jar there.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault or VaultUnlocked, with an economy | Prices, refunds, builder and boost purchases. Without an economy everything is free |
| PlaceholderAPI | The `%uxmbuilders_...%` [placeholders](../placeholders.md) |
| WorldEdit or FastAsyncWorldEdit | A WorldEdit selection works for `/builders admin save`, beside the plugin's own wand |
| WorldGuard, GriefPrevention, Towny, PlotSquared, uxmClaims | Native [land protection](../features/protection.md) checks |
| LuckPerms | Nothing specific; any permissions plugin grants the nodes |

Every one of these is a soft dependency. A protection plugin that enables after uxmBuilders is picked up
when it loads.

## Installing

1. Put the jar for your server into `plugins/`, and remove any other uxmBuilders jar.
2. Start the server. uxmBuilders downloads its libraries, writes all its files, and then disables
   itself with `License key is not set! Plugin cannot start.`, because `general.licenseKey` still
   reads `LICENSE_KEY_HERE`.
3. Stop the server and open `plugins/uxmBuilders/config.yml`. Set `general.licenseKey`, and pick a
   language under `general.language` (see [Languages](../features/languages.md)).
4. Start the server again. The plugin creates its database tables and registers `/builders`.
5. Add your first package, as described in [Your first build](first-build.md).

No package is configured out of the box on purpose: a package points at a schematic file that only
exists on your server.

## The files

```
plugins/uxmBuilders/
├── config.yml          license, language, performance, preview, economy, protection, rules, database
├── messages.yml        every message: chat, action bar, boss bar, titles, sounds
├── builders.yml        builders, their levels and appearance
├── packages.yml        shop categories and packages
├── webhooks.yml        the Discord webhook and its texts
├── menu/*.yml          the nine menus: titles, slots, items, names and lore
├── schematics/         your .schem files
├── data/
│   ├── uxmbuilders.db  the SQLite database, when SQLite is the backend
│   ├── metadata.yml    the files' version and language; do not edit
│   ├── license.cache   license state, with instance.id
│   └── pending-returns.yml  blueprints owed to players who were offline
├── libs/               downloaded libraries
└── backups/            copies taken before a language switch
```

The files are written in English. Set `general.language` and, on the next reload or restart, every
text you have not edited is rewritten in that language. Each file has a page under
[Configuration](../config/index.md).

<Callout type="warning" title="The plugin does not start without a valid key">

With `general.licenseKey` empty or left at `LICENSE_KEY_HERE`, the plugin logs the reason and disables
itself before touching the database. A key the license server refuses ends with
`License validation failed!` in the console. A server moved to new hardware may need its key reissued.

</Callout>

## Reloading

`/builders admin reload` re-reads `config.yml`, `messages.yml`, `builders.yml`, `packages.yml`,
`webhooks.yml` and the nine menus. It also:

- switches the language, if `general.language` changed;
- clears the schematic cache and registers the generated permissions again;
- starts queued builds at once when `performance.maxConcurrentJobs` went up;
- closes open previews when `preview.enabled` was turned off;
- starts or stops the update check to follow `general.updateCheck`.

Running builds keep the builder, speed and settings they started with. The database connection and the
license do not reload; restart for those.

## Folia

On Folia every build runs on the thread of the region it stands in, so builds in different regions run
side by side. If a building job goes 30 seconds without being ticked, the plugin logs
`[Engine] Job … is building but was not ticked` and gives it a new ticker; that line is the one to
report.

## First checks

| Check | How |
|---|---|
| The plugin started | `/builders` opens the main menu |
| Protection is wired | `/builders admin protection` lists each provider as available or not |
| Prices work | The shop shows a price, not `0` |
| Performance is healthy | `/builders admin perf` shows the load factor and blocks per second |
| Placeholders are registered | `/papi parse me %uxmbuilders_active%` answers a number |
