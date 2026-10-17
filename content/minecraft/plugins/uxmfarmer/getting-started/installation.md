---
title: Installation
order: 11
description: Requirements, the two hooks it cannot work without, and verifying the install.
icon: download
---

## Requirements

| | |
|---|---|
| Server | Paper 1.20 or newer. Folia is supported |
| Java | 17 or newer |
| Required | A region plugin from the [supported list](../integrations.md#region-hook) |
| Recommended | PlaceholderAPI, Vault, an NPC plugin |

Libraries are downloaded by the server at startup: ORMLite, SnakeYAML and XSeries are declared in
`plugin.yml` and do not need to be shaded or installed by hand.

## First run

1. Drop `uxmFarmer.jar` into `plugins/` and start the server. The default configuration is written
   to `plugins/uxmFarmer/`.
2. Stop the server and open `config.yml`.
3. Set `license-key` to the key from your UXPLIMA panel.
4. Set `hooks.region-hook` to the plugin that owns claims or islands on your server. This is not
   optional: a farmer belongs to a region, and without this hook there is nowhere to put one.
5. Set `hooks.interaction-hook` to the NPC plugin you run. With `default` or `""` the farmer has no
   body and is reachable only through commands.
6. Set `hooks.money-hook`: `Vault`, `PlayerPoints`, or `SuperiorSkyblock2` to charge the island
   bank.
7. Set `world-management.world-list` and `list-type` so farmers exist only where you want them.
8. Start the server.

## Verifying

```
/uxmfarmer givefarmer <player>
/uxmfarmer nearfarmers
```

`givefarmer` places a farmer for a player who has a region; `nearfarmers` lists the farmers around
you with their owners and UUIDs. If both work, the region hook and the NPC hook are talking to each
other.

<Callout type="warning" title="Set the world list before you open the server">

`world-management.list-type` defaults to `BLACKLIST` with `world` in the list, which means farmers
are blocked in your main world and allowed everywhere else. On most servers this is backwards:
switch to `WHITELIST` and list your island or survival world.

</Callout>

## Database

SQLite is the default and needs no setup. For a network, set `database.driver` to `MYSQL`, fill in
the connection details, and set `multi-server.enabled: true` with a `redis-host` so servers stay in
step. See [Database and multi-server](../config/database.md).
