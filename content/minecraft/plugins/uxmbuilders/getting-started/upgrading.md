---
title: Upgrading from 1.0.0
order: 205
description: Moving a 1.0.0 server to uxmBuilders 2.0.0, which starts fresh, and what changed on the way.
icon: circle-arrow-up
---

2.0.0 is a rewrite, and it is a **fresh start**. Nothing 1.0.0 stored is imported and none of its files
are converted. Plan the switch for a moment when no builds are running, and set the plugin up again on
the new files.

## What does not carry over

| 1.0.0 | In 2.0.0 |
|---|---|
| Builds running, queued or paused | Gone. 2.0.0 neither resumes nor refunds them |
| Builders players bought, and their levels | Every player starts again with `builders.defaultBuilder` at level 1. Builders and levels granted by permission still count |
| Warehouse stock | Empty |
| Trust lists | Empty |
| Build history | Empty |
| `config.yml`, `messages.yml`, the menus, `builders.yml`, `packages.yml` | Not converted. A 1.0.0 file left in place is read as it is: renamed keys are not found, and 1.0.0 menu layouts are not drawn as they were |
| The `ub_*` database tables | Left where they are and never read. 2.0.0 uses its own `uxmbuilders_*` tables |
| The `lang/<code>/` folders | Not used. 2.0.0 writes the chosen language into the usual files |

## What does carry over

| Kept | Note |
|---|---|
| Your `.schem` files | Sponge schematics, versions 1 to 3, are read by the plugin itself. MCEdit `.schematic` files are not read. A file name may hold letters, digits, `_`, `.` and `-`, up to 64 characters, with no sub-folders |
| Package and builder entries | `packages.yml` and `builders.yml` use the same keys as 1.0.0, so entries can be copied across into the new files by hand |
| Permission nodes | The same names; `uxmbuilders.admin.wand` and `uxmbuilders.warehouse.pages.<n>` are new |
| Blueprints players hold | They carry the same item data, so they place the package of the same id once it exists again |
| The license key | Goes into the new `config.yml` |

## Before you start

1. Let running builds finish, or cancel them on 1.0.0 so the refunds are paid there.
2. Ask players to take their materials out of the warehouse, and tell them that bought builders and
   levels reset.
3. Stop the server and keep a copy of `plugins/uxmBuilders/` and of the database if it is MySQL.

## The switch

1. Rename `plugins/uxmBuilders/` to something like `plugins/uxmBuilders-1.0.0/`, so 2.0.0 starts with
   an empty folder. The old folder is your backup.
2. Replace the jar with `uxmBuilders-paper-<version>.jar` on Paper or `uxmBuilders-folia-<version>.jar`
   on Folia. WorldEdit can stay or go; it is no longer required.
3. Start once, set `general.licenseKey` and `general.language`, and start again, as in
   [Installation](installation.md).
4. Copy your `.schem` files from the old `schematics/` folder into the new one.
5. Add the packages again: copy the entries under `packages:` from the old `packages.yml`, or create
   them with `/builders admin package create`. Then `/builders admin reload`.
6. Carry over the settings you changed in the old `config.yml`, `builders.yml` and `messages.yml`,
   key by key, using the table below for the ones that moved. Start from the new menu files and make
   your layout changes again.
7. With MySQL, point `database.url` at the same database if you like: the new tables sit beside the
   old ones, which you can drop once you no longer need them.

## Where settings went

| 1.0.0 | 2.0.0 |
|---|---|
| `config.yml`: `builders.default` | `builders.defaultBuilder` |
| `config.yml`: `notifications.webhook.enabled`, `url`, `username`, `events` | `enabled`, `url`, `username`, `events` in [`webhooks.yml`](../config/webhooks-yml.md) |
| `messages.yml`: `webhookBuildStarted`, `webhookBuildCompleted`, `webhookBuildCancelled` | `buildStarted`, `buildCompleted`, `buildCancelled` in `webhooks.yml` |
| `config.yml`: `gui.fillMaterial` | Removed. The filler is the entry without a slot under `otherItems` in each menu file |
| Menu files: `entrySlots` | `itemSlots` |
| `lang/<code>/messages.yml` and `lang/<code>/menu/*.yml` | The usual `messages.yml` and `menu/*.yml`, rewritten in the chosen language |
| Database: `ub_*` tables, `ub_schema_version` | `uxmbuilders_*` tables, `uxmbuilders_schema_migrations` |
| API: `com.uxplima:builders-api`, Bukkit events | `com.uxplima.builders:bukkit-api`, its own events; see [Developer API](../developer.md) |

## What is new

| | |
|---|---|
| Folia | A separate jar; each build runs on its region's thread |
| No WorldEdit needed | `/builders admin wand` selects, `/builders admin save` saves; WorldEdit still works when installed |
| Databases | PostgreSQL besides SQLite and MySQL; `jdbc:mariadb:` URLs work |
| Several servers on one database | `database.serverId`; each server brings back only the builds it started |
| `webhooks.yml` | The Discord webhook settings and texts in their own file, in every language |
| Update check | `general.updateCheck`; the console and staff hear about a new release |
| uxmClaims | A native protection provider, `uxmclaims` |
| `/builders warehouse <player>` | Stock the warehouse of an owner who trusts you through a window |
| `/builders trust add <player>` | Another way to write `/builders trust <player>` |
| Menu items | `texture` for player heads, `slots` for several slots, `playerCommands` and `consoleCommands` run on click; see [Messages and menus](../config/messages-and-menus.md) |

## What behaves differently

| | 1.0.0 | 2.0.0 |
|---|---|---|
| Cooldowns | The longer of `start.cooldownSeconds` and the package's `cooldown` | Both apply on their own: the global one between any two starts, the package one between two starts of that package |
| `uxmbuilders.package.<id>` | Granted the package only when its `permission` named that node | Grants the package on its own |
| Builder levels from permissions | Written into the profile when the player loaded | Read live: taking the permission away lowers the level again |
| A staff member cancelling another player's build in the job menu | Refunded as an owner cancel | Refunded in full, as `/builders cancel` and `/builders admin cancel` do |
| A build in a world that is not loaded after a restart | Retried on the next start | Comes back as soon as the world loads |
| A language switch | Wrote `lang/<code>/` the first time; your copy stayed yours | Rewrites the texts you have not edited and keeps the ones you changed, after a backup |
| A sound name that is not a Bukkit sound | Logged as unknown | Played as a sound key, so resource-pack sounds work |
| `/builders` from the console | An error | The help list |

Related: [Installation](installation.md), [The database](../config/database.md)
