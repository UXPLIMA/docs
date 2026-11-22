---
title: Network
order: 1349
description: Reference for the network module: change log, node list, leader, vault server and Redis notices.
icon: network
---

The network module lets several servers write one MySQL, MariaDB or PostgreSQL database at once. Every write
records what it changed in a change log, and the other servers read the log and reload those entities. This page
lists the settings and parts; for setting a network up step by step, read [Network mode](../network-mode.md).

Module `network` · disabled by default · `modules/network/config.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm network status` | Shows this server's node id, the leader, the vault server, how far the change log is read, counts of reloaded and failed changes, the modules followed here, the Redis link and every server's heartbeat. | `uxmclaims.admin.network` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.network` | op | `/uxm network status` |

## Settings

| Key | Default | What it does |
|---|---|---|
| `node-id` | `""` | This server's name in the network, unique per server. Empty: the server's `ip:port`. |
| `poll-interval` | `2s` | How often this server reads changes the others wrote. Never below 250 ms. |
| `settle` | `10s` | How long a change may take to show up after it was numbered (a slow commit). Newer changes are read again on the next poll, so none is missed. |
| `catch-up` | `60s` | At start-up, changes this old are read again, so a change written while the server loaded its data is not missed. |
| `retention` | `1h` | How long change rows are kept. The leader deletes older ones every 10 minutes. |
| `batch-size` | `2000` | Most change rows one poll reads. |
| `heartbeat` | `10s` | How often this server writes its heartbeat into the node list. Never below 1 s. |
| `leader` | `auto` | Which server runs the scheduled jobs (tax day, expiry, purges, lapsing trust and invites, deleting old change rows). `auto`: the live server with the smallest node id. `always` or `never` pin it. |
| `vault-node` | `""` | Node id of the one server that may open land vaults. Every other server keeps them closed. Empty closes them everywhere. |
| `redis.enabled` | `false` | Sends a Redis notice after each write so the other servers read the log at once instead of at the next poll. |
| `redis.host` | `localhost` | Redis host. |
| `redis.port` | `6379` | Redis port. |
| `redis.user` | `""` | ACL user (Redis 6+). Empty logs in with the password alone. |
| `redis.password` | `""` | Empty connects without AUTH. |
| `redis.database` | `0` | Database number for SELECT. |
| `redis.tls` | `false` | Connects over TLS. Java must trust the certificate and it must match the host. |
| `redis.channel-prefix` | `uxmclaims:` | Notices go to the channel `<prefix>changes`. Networks that share one Redis need different prefixes. |
| `redis.timeout` | `5s` | How long connecting and each answer may take. Never below 500 ms. |

## Database tables and Redis

| Name | What it holds |
|---|---|
| `uxmc_network_change_log` | One row per changed entity: module, entity type, entity id, revision, the node that wrote it and when. Written inside the same transaction as the change. |
| `uxmc_network_nodes` | One row per server: node id, boot id, start time, last heartbeat, and how far it has read the log. |
| Redis channel `<prefix>changes` | Publish/subscribe only. Nothing is stored in Redis. |

The change log stays the source of truth. When Redis is down, changes still arrive by polling, and after the
link comes back the server reads the log at once to make up for notices it missed. No other Redis plugin is
needed.

## What is followed

Each module registers reloaders for the entity types it writes. A change is named by its lock key,
`<entity type>:<id>`, and the server calls every reloader of the module that wrote it.

| Module | Entity types |
|---|---|
| [Lands](lands.md) | `land`, `selection` |
| [Areas](areas.md), [Warps](warps.md), [Claim Block](claim-block.md), [Productive Land](productive-land.md) | `land` |
| [Bans](bans.md), [Expiry](expiry.md), [Invites](invites.md) | `land` and their own type |
| [Towns](towns.md), [Nations](nations.md) | `town`, `nation` |
| [Relations](relations.md) | `relation`, `relations` |
| [Bank](bank.md), [Taxes](taxes.md), [Inbox](inbox.md), [Levels](levels.md), [Plots](plots.md), [Ruins](ruins.md), [War](war.md), [Leaderboards](leaderboards.md), [Claim Points](claim-points.md) | their own type |
| Kernel | player profiles, player preferences, worlds |

A change from a module with no reloader is counted as "not followed here" in `/uxm network status`; the server
sees it after a restart.

## Notes

- The module refuses to start on SQLite. Turn it on in `modules.yml` on every server of the database and restart
  them all: turning it on while the server runs fails, because the database lease is still the single-writer one.
- Cross-server teleports and chat are not part of it.
- Not followed yet: [Newbie Protection](newbie-protection.md) given or removed on another server (the stored time
  applies at the player's next join), and claim block holograms when another server renames a land or moves its
  claim block.
- Claim points are saved whole per player. Two servers changing one player's points at the same moment keep the
  last save.
- [War](war.md) vassals, peaceful statuses and mutual-acceptance offers show on other servers after a restart.
- [Discord](discord.md) with its own bot should run on one server only.

Related: [Network mode](../network-mode.md), [Database](../configuration/database.md), [Land Vault](vault.md)
