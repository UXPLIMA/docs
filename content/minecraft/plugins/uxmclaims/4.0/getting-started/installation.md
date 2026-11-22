---
title: Installation
order: 111
description: Requirements, the license key, and the files uxmClaims 4 writes.
icon: download
---

uxmClaims 4 ships as one jar, `uxmClaims-<version>.jar`, for Spigot, Paper and Folia. Put it in
`plugins/`, start once to get the files, add the license key and start again.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 21 or newer |
| Paper, Purpur and other Paper forks | 1.19.4 or newer |
| Spigot | 1.19.4 or newer, on a build that runs on Java 21. Spigot 1.19.4 itself refuses Java 21 ("Only up to Java 20"), so use Paper there |
| Folia | Supported by the same jar (`folia-supported: true`) |
| License | A uxmClaims license key, and outbound HTTPS to `www.uxplima.com` |
| Internet on the first start | uxmClaims downloads jOOQ, HikariCP, fastutil and the MySQL and PostgreSQL drivers into `plugins/uxmClaims/libs/` |
| Database | SQLite out of the box; MySQL, MariaDB or PostgreSQL optional, see [Database](../configuration/database.md) |

On an older Java the plugin does not crash: it logs which Java it needs and disables itself.

## Optional plugins

uxmClaims starts without any of these. Each is used by a module when the plugin is installed and the
module is on.

| Plugin | Used by |
|---|---|
| Vault and an economy plugin | [Economy](../modules/economy.md), and through it every price, bank and tax |
| LuckPerms | [LuckPerms](../modules/luckperms.md): offline limits and LuckPerms contexts |
| PlaceholderAPI | [PlaceholderAPI](../modules/placeholders.md) |
| WorldGuard | [WorldGuard](../modules/worldguard.md): keeps claims out of regions |
| Dynmap, BlueMap, Pl3xMap, squaremap | [Web Maps](../modules/webmap.md) |
| Geyser or Floodgate | [Bedrock](../modules/bedrock.md): forms for Bedrock players |
| DiscordSRV | [Discord](../modules/discord.md), unless you use its own bot |

The full list, with CoreProtect and Skript, is on [Integrations](../integrations.md).

## Installing

1. Put `uxmClaims-<version>.jar` in `plugins/` and remove any older uxmClaims jar.
2. Start the server. uxmClaims downloads its libraries, writes `config.yml`, and then disables itself
   with `No license key in config.yml (license.key)`.
3. Stop the server, open `plugins/uxmClaims/config.yml` and set `license.key`. While you are there,
   decide `language.default` and the `storage` block.
4. Start again. The kernel opens the database, creates the tables of every module that is on, and
   registers `/claim` and `/uxm`.
5. Join as an admin and pick a preset: see [First start](first-start.md).

A server that already ran uxmClaims 3.x is different: its data is moved on the first start. Read
[Upgrading from 3.x](../upgrading/from-3x.md) first.

## The files

```
plugins/uxmClaims/
├── config.yml          kernel settings: language, storage, license, protection, menus
├── modules.yml         which modules are on
├── modules/
│   └── <module>/       one folder per module that has run once
│       ├── config.yml  the module's settings
│       └── lang/<code>/ its messages, command names and menus
├── lang/               the kernel's own texts
├── presets/            your own presets, next to the shipped ones
├── data/
│   ├── storage.db      the SQLite database, when SQLite is the backend
│   ├── setup.yml       first-start and last-preset record
│   └── license.cache   license state, with instance.id
├── migration/          reports of the 3.x move and of database commands
├── libs/               downloaded libraries
└── backups/            copies of every file the plugin rewrites
```

A module's folder appears the first time the module runs. Turning a module off never deletes its
folder or its tables.

When you update the jar, missing keys are added to your files with their comments, and keys the plugin
does not know are reported, never deleted. Texts you never changed follow the new version; texts that
may be yours are kept and listed by `/uxm lang update`. See [Languages](../configuration/languages.md).

## Reloading

`/uxm reload` (needs `uxmclaims.admin`) reads the config, language and menu files again. Turning a
module on or off, changing `storage`, and the license key need a restart.

## First checks

| Check | How |
|---|---|
| The plugin is enabled | `/plugins` lists uxmClaims in green, and the console shows no license or storage error |
| The modules started | `/uxm modules` lists each module as `enabled`, `disabled` or failed, with the reason |
| Claiming works | Stand in a protected world and run `/claim create Test` |
| Economy works | `/uxm modules` shows `economy` enabled; with Vault missing it explains why it is off |
| The database is current | `/uxm db status` shows every module at its latest migration |
