---
title: Network
order: 411
description: Several servers on one database, the REST API, Discord and web maps.
icon: network
---

## Several servers

Servers that share islands use one MySQL, MariaDB or PostgreSQL database and one Redis. Set `network.clustered = true`
and `network.redis-uri` in [config.conf](../config/config-conf.md), and give each server its own `server-name`.

Each server holds the islands it runs under a lease (`authority-lease`, 10 minutes) and renews it
(`authority-heartbeat-interval`, 2 minutes). A visitor to an island another server holds is sent there. When a server
stops, another takes its islands once the lease runs out.

## REST API

Off as shipped. `rest` in [config.conf](../config/config-conf.md) opens an HTTP API on `127.0.0.1:8080` that every
request reaches with `Authorization: Bearer <token>`. It refuses to start while the token is still `change-me`. A web
store deposits into an island's bank through it, and a retried deposit with the same Idempotency-Key is answered
rather than made twice.

## Discord

Island milestones, leaderboard changes, alliances and staff actions are posted to webhooks; see
[discord.conf](../config/discord-conf.md).

## Web maps

Islands are drawn on dynmap, BlueMap or Pl3xMap when one is installed; see [webmap.conf](../config/webmap-conf.md).
