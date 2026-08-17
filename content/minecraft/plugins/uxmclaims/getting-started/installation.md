---
title: Installation
order: 102
description: Requirements, the first start, and the files that appear.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper, Spigot, Purpur, Folia or another Bukkit fork, 1.19.4 or newer |
| Java | 21 or newer |
| Database | SQLite out of the box; MySQL 8+ or PostgreSQL 13+ optional |

Paper is not required: `api-version` is `1.20` and the plugin declares `folia-supported: true`, so it
runs unchanged on a regionised server.

## Optional plugins

Every one of these is a `softdepend`. The plugin starts without all of them.

| Plugin | Adds |
|---|---|
| Vault | Charging money for claims, chunks, warps and time |
| A permission plugin | Reading `uxmclaims.limit.*` and `uxmclaims.ability.*` nodes |
| PlaceholderAPI | The `%uxmclaims_...%` placeholders, and placeholders inside messages |
| WorldGuard | Refusing claims that overlap a region |
| Dynmap, BlueMap, Pl3xMap, squaremap | Drawing claims on the web map |

Adventure and MiniMessage are downloaded at runtime through Paper's `libraries` mechanism: there is
nothing to shade or install.

## Installing

1. Drop `uxmClaims.jar` into `plugins/`.
2. Start the server once. The plugin writes its files and creates `data/claims.db`.
3. Stop the server.
4. Open `config.yml` and set at minimum `generalSettings.licenseKey`, `generalSettings.disabledWorlds`
   and `claimSettings.expireMode`.
5. Start again.

## The files

```
plugins/uxmClaims/
├── config.yml          settings, borders, holograms, blocks, database
├── roles.yml           the default roles new claims are created with
├── entitlements.yml    limits, costs and delays, and how permissions change them
├── messages.yml        every message the plugin sends
├── webhooks.yml        Discord webhooks per event
├── aliases.yml         short commands mapped onto /claim subcommands
├── menu/               28 menu layouts
└── data/claims.db      the SQLite database, when SQLite is the backend
```

Each has its own page under [Configuration](../config/).

## Reloading

`/claim reload` re-reads every file and rebuilds the menus, the alias table and the border colours.

<Callout type="warning" title="Reload does not move a claim to a new database">

`database` in `config.yml` is read at startup. Changing the backend and running `/claim reload` leaves
the plugin on the old connection. Restart the server, and read
[Database](../database/) before changing it on a live server: nothing migrates the rows for you.

</Callout>

## First checks

| Check | How |
|---|---|
| The plugin loaded | `/claim` opens a menu |
| Claiming works | Stand in a normal world and run `/claim create Test` |
| Economy is wired | `/claim` in a second chunk shows a cost, and the balance drops |
| Limits are read | Grant `uxmclaims.limit.claim.2` and confirm the limit rises |
| The map integration works | Open the web map and look for the claim outline |
