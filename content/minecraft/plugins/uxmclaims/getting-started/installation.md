---
title: Installation
order: 102
description: Requirements, the first start, and the files that appear.
icon: download
---

uxmClaims ships as two jars. Pick the one that matches the server, start once to get the files, add the
license key and start again.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 21 or newer |
| Spigot, Paper, Purpur | 1.19.4 or newer, with `uxmClaims-bukkit-<version>.jar` |
| Folia | 1.21 or newer, with `uxmClaims-folia-<version>.jar` (it also runs on Paper 1.21 and newer) |
| License | A uxmClaims license key, and outbound HTTPS to `www.uxplima.com` |
| Internet on first start | uxmClaims downloads jOOQ, HikariCP and the MySQL and PostgreSQL drivers into `plugins/uxmClaims/libs/` |
| Database | SQLite out of the box; MySQL 8+ or PostgreSQL 13+ optional, see [Database](../database/index.md) |

The bukkit jar declares `folia-supported: false`, so Folia refuses it. Use the folia jar there.

## Optional plugins

uxmClaims starts without any of these. Each one is used when it is installed and its switch in
[`config.yml`](../config/config-yml.md) is on.

| Plugin | Adds | Switch | Without it |
|---|---|---|---|
| Vault and an economy plugin | Costs for claims, chunks, time, warps, warp teleports, roles, invites and renames | `generalSettings.economySupport`, `economyProvider` | Everything is free |
| LuckPerms | Reads the `uxmclaims.ability.*`, `limit.*`, `cost.*` and `delay.*` nodes | `generalSettings.permissionSupport` | Every ability is allowed and every entitlement uses its default |
| PlaceholderAPI | The `%uxmclaims_...%` placeholders for other plugins | `generalSettings.placeholderSupport` | No placeholders outside uxmClaims |
| WorldGuard | Refuses claims on WorldGuard regions | `generalSettings.worldGuardSupport` | Regions are not checked; `disabledWorlds` still applies |
| Geyser or Floodgate | Bedrock forms for Bedrock players ([Bedrock menus](../menus/bedrock.md)) | `bedrock.enabled`, `bedrock.menus` | Bedrock players get the Java chest menus |
| Dynmap, BlueMap, Pl3xMap or squaremap | Claim outlines on the web map | `visualizer.enabled` | No map layer |

## Installing

1. Put the jar for your server in `plugins/`, and remove any older uxmClaims jar.
2. Start the server. uxmClaims downloads its libraries, writes its files, and then disables itself with
   `License validation failed`, because `generalSettings.licenseKey` still reads `LICENSE_KEY_HERE`.
3. Stop the server and open `plugins/uxmClaims/config.yml`. Set `generalSettings.licenseKey`. While you
   are there, decide `language`, `generalSettings.disabledWorlds` and `claimSettings.expireMode`.
4. With LuckPerms, grant the ability nodes players need (see the warning below).
5. Start again. The plugin creates its database tables and registers `/claim`.

<Callout type="warning" title="With LuckPerms, nobody can claim until you grant ability nodes">

uxmClaims registers no defaults for `uxmclaims.ability.*`. Under LuckPerms a node nobody granted is
denied, so on a fresh server every attempt to claim ends in a permission error. Grant
`uxmclaims.ability.claim.create` and the other nodes you want, or `uxmclaims.ability.*` for all of them, to
your default group. See [Ability permissions](../permissions/abilities.md).

</Callout>

## The files

```
plugins/uxmClaims/
├── config.yml          language, claim rules, borders, holograms, blocks, dialogs, Bedrock, database
├── messages.yml        every message the plugin sends
├── roles.yml           the roles every new claim starts with
├── entitlements.yml    limits, costs and delays, and the nodes that change them
├── webhooks.yml        Discord webhooks per event
├── aliases.yml         short commands mapped onto /claim subcommands
├── menu/               28 menu layouts
│   └── bedrock/        21 Bedrock form layouts
├── data/
│   ├── claims.db       the SQLite database, when SQLite is the backend
│   ├── metadata.yml    the files' version and language; do not edit
│   ├── license.cache   license state, with instance.id
│   └── server.id       this server's id, with MySQL or PostgreSQL
├── libs/               downloaded libraries
└── backups/            copies taken before a language switch
```

Each file has its own page under [Configuration](../config/index.md).

The files are written in English. Set `language` to `tr`, `es`, `pt-BR`, `de`, `fr`, `ru` or `pl` and
restart or run `/claim reload`: every text you have not edited is rewritten in that language, and the old
files are copied to `backups/language-<from>-<time>/` first. See [Languages](../config/languages.md).

When you update the jar, uxmClaims patches the files to the new version on the next start and keeps your
values. `data/metadata.yml` records which version the files are at.

## Reloading

`/claim reload` (needs `uxmclaims.admin`) re-reads the YAML files and menu layouts listed above, and
applies a changed `language`. A few things are only read at startup and need a restart:

| Setting | Why |
|---|---|
| `database` | The connection pool is built once. Changing the backend does not move existing claims, see [Database](../database/index.md) |
| `economySupport`, `economyProvider`, `permissionSupport`, `worldGuardSupport`, `placeholderSupport`, `visualizer.enabled`, `border.visualizerType` | The integrations are wired once |
| `dialog.timeoutSeconds`, `generalSettings.reminderIntervalSeconds` | The timers are started once |

## First checks

| Check | How |
|---|---|
| The plugin is enabled | `/plugins` lists uxmClaims in green, and the console shows no license error |
| Claiming works | Stand in a world that is not disabled and run `/claim create Test` |
| Economy works | Stand next to the claim, run `/claim chunk add`, and watch the balance drop by 25 |
| LuckPerms is read | Grant `uxmclaims.limit.claim.1` and create a second claim (the default limit is 1) |
| The web map works | Look for the claim outline on the map |
| Bedrock works | Join through Geyser and run `/claim menu`: a form opens instead of a chest |
