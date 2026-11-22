---
title: Skript
order: 1354
description: Skript events, expressions, conditions and effects for lands, towns and nations, from a separate add-on jar.
icon: scroll-text
---

The Skript add-on lets scripts react to land, town and nation events, read lands, towns and nations, and make
changes such as trusting a player or renaming a town. It is a separate plugin, `uxmClaims-skript-<version>.jar`,
that you put in `plugins/` next to uxmClaims; the main jar contains no Skript code.

Add-on `uxmClaims-skript` · registers module `skript` (enabled by default, switch in `modules.yml`) · needs Skript 2.12 or newer

## Installing

1. Put `uxmClaims-skript-<version>.jar` in `plugins/`, next to `uxmClaims` and `Skript`.
2. Restart the server. The add-on refuses to load its syntax after start-up, or with a Skript older than 2.12,
   and says so in the log.

Turning the `skript` module off in `modules.yml` leaves the syntax loaded but inert: expressions are empty,
conditions are false and effects report a failed result.

## Types

| Type | What it is |
|---|---|
| `uxmland` | A land, as it was when it was read. |
| `uxmtown` | A town, as it was when it was read. |
| `uxmnation` | A nation, as it was when it was read. |

Values are snapshots: read them again after a change. They print as their name, cannot be typed in a script
and cannot be kept in saved global variables; look them up with `land named "..."` instead.

## Events

Every event may start with `uxm` or `uxmclaims`: `on land create:` and `on uxmclaims land create:` are the same.

| Event | Event values |
|---|---|
| `on land create` | `uxmland` |
| `on land delete` | `uxmland` |
| `on before land delete` | `uxmland`. `cancel event` stops the delete. |
| `on land rename` | `uxmland` |
| `on land enter`, `on land leave` | `uxmland`, `player` |
| `on land chunk claim`, `on land chunk unclaim` | `uxmland` |
| `on land trust` (`on land member add`) | `uxmland`, `offlineplayer`, `player` |
| `on land untrust` (`on land member remove`) | `uxmland`, `offlineplayer`, `player` |
| `on land transfer` | `uxmland` |
| `on town create`, `on town delete`, `on town rename` | `uxmtown` |
| `on town join`, `on town leave`, `on town kick` | `uxmtown`, `offlineplayer`, `player` |
| `on before town join` | `uxmtown`, `offlineplayer`, `player`. `cancel event` stops the join. |
| `on town mayor change` | `uxmtown`, the new mayor as `offlineplayer` and `player` |
| `on nation create`, `on nation delete`, `on nation rename` | `uxmnation` |
| `on nation join`, `on nation leave` | `uxmnation`, the town as `uxmtown` |
| `on <action> result`, `on uxmclaims result` | How an effect ended, see below. |

## Expressions

| Expression | Returns |
|---|---|
| `land at %location%` | The land at a place. |
| `land named %string%` | A land by name. |
| `land name of %uxmlands%` | Its name. |
| `owner of %uxmlands%` | The owner as an offline player, when a player owns it. |
| `owner name of %uxmlands%` | The owner's name, also for town, nation and admin lands. |
| `members of %uxmlands%` | The owner and members, as offline players. |
| `lands of %offlineplayers%` (also `uxm lands of`) | The lands a player is a member of, owned ones included. |
| `role of %offlineplayer% in %uxmland%` | The player's role name there, the visitor role for outsiders. |
| `town of %offlineplayers%` | The player's town. |
| `town named %string%` | A town by name. |
| `owning town of %uxmlands%` | The town that owns a land. |
| `town name of %uxmtowns%`, `mayor of %uxmtowns%` | Name and mayor. |
| `residents of %uxmtowns%` | The residents, as offline players. |
| `nation of %offlineplayers%` | The player's nation. |
| `nation of town %uxmtown%` | A town's nation. |
| `nation named %string%` | A nation by name. |
| `nation name of %uxmnations%` | Its name. |
| `member towns of %uxmnations%` | The towns in a nation. |
| `result action`, `failure reason` | Inside a result event: the action, and why it failed. |

The `of` forms also work the other way round, as in `%uxmland%'s land name`.

## Conditions

| Condition | True when |
|---|---|
| `%player% can build at %location%` | The full protection chain allows the player to place a block there. |
| `%offlineplayers% is a member of %uxmland%` | The player owns the land or is a member. |
| `%offlineplayers% is a resident of %uxmtown%` | The player lives in the town. |
| `%locations% is in the wilderness` | No land claims the place. |
| `the action succeeded`, `the action failed` | Inside a result event. |

Each has a negated form (`can't build`, `isn't a member of`, `isn't in the wilderness`).

## Effects

| Effect | Action name |
|---|---|
| `make %player% join town %uxmtown%` | `town join` |
| `make %player% leave town %uxmtown%` | `town leave` |
| `kick %offlineplayer% from town %uxmtown%` | `town kick` |
| `rename town %uxmtown% to %string%` | `town rename` |
| `trust %offlineplayer% in land %uxmland%` | `land trust` (with the land's member role) |
| `untrust %offlineplayer% from land %uxmland%` | `land untrust` |
| `rename land %uxmland% to %string%` | `land rename` |
| `delete land %uxmland%` (or `unclaim`) | `land delete` |
| `create land [named %string%] for %offlineplayer% at %location%` | `land create` (a personal land on that chunk) |

Effects only start a change; it finishes after the database has saved it. To act on the outcome, listen to
`on <action name> result`, or `on uxmclaims result` for all of them:

```
on town join result:
    if the action failed:
        send "Could not join: %failure reason%" to player
```

`failure reason` is the message key of a refused change (`towns.error.not-invited`) or one of
`skript.module-off`, `skript.missing-value`, `skript.module-missing` and `skript.error`.

## Notes

- Effects run as the plugin: permission, limit, cost and cooldown checks are skipped. The rules of the
  action itself still apply.
- Events listen to the Bukkit copies of uxmClaims events. They stop when `api.bukkit-events.enabled` is off in
  the root `config.yml`; land enter and leave also need `api.bukkit-events.movement`, and `before` events need
  `api.bukkit-events.pre-events`.
- Town and nation syntax needs the [Towns](towns.md) and [Nations](nations.md) modules; without them the
  expressions are empty and the effects fail with `skript.module-missing`.
- The add-on does not run on Folia.
- There is no Skript language file yet, so Skript errors show the code names `uxmland`, `uxmtown` and
  `uxmnation`. The add-on has been loaded on a live server with Skript 2.17.1; an event firing in game has not
  been tried yet.
- The add-on is first-party, so its module keeps the plain id `skript`. Other add-ons need a vendor prefix in
  their module id, such as `example-shops`.

Related: [Towns](towns.md), [Nations](nations.md), [Events](../developer/events.md), [Integrations](../integrations.md)
