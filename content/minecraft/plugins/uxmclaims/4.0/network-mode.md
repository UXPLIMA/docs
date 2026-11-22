---
title: Network mode
order: 175
description: Run several servers behind a proxy on one uxmClaims database.
icon: network
---

Without network mode, one database belongs to one server: a second server that opens the same
MySQL, MariaDB or PostgreSQL database is refused at start-up, so two servers never overwrite each
other. Network mode lets every server of a network write the same database and see each other's
changes. The settings are on [Network](modules/network.md); this page is the procedure.

## What it does and does not do

| Shared across servers | Not part of network mode |
|---|---|
| Lands, chunks, roles, members, flags, warps, bans, invites and the data of every module that follows changes | Teleporting a player to another server |
| Scheduled jobs (tax day, expiry, purges), run by one server only | Chat between servers |
| A land deleted or restored on one server, on all of them | Opening a land vault on more than one server |

Each server writes a change log row for everything it changes. The others read the log every
`poll-interval` (2 seconds) and reload what changed. With Redis, a server announces its change at
once, and the others read the log without waiting for the next poll. The log stays the source of
truth: when Redis is down, changes still arrive by polling.

## Requirements

| Requirement | Detail |
|---|---|
| Storage | MySQL, MariaDB or PostgreSQL on every server, same database and same `table-prefix`. SQLite is never shared, and the network module refuses to start on it |
| The same jar | Every server on the same uxmClaims version |
| Node ids | A different `node-id` per server; empty uses the server's address and port |
| Redis | Optional, 6 or newer for ACL users. No other Redis plugin is needed |

## Setting it up

1. Back up the database.
2. On every server, set `storage` in `config.yml` to the shared database.
3. Apply the `network` preset on one server (`/uxm preset apply network`), or turn `network` on in
   `modules.yml` by hand. Copy `modules.yml` to every server: they should run the same modules.
4. On every server, set `node-id` in `modules/network/config.yml`.
5. Pick the server that may open land vaults and put its node id in `vault-node` on every server.
   Leave it empty to keep vaults closed everywhere.
6. With Redis, fill the `redis` block the same way on every server and set `redis.enabled: true`.
   Networks sharing one Redis need different `channel-prefix` values.
7. Stop all servers, then start them. The shared database lease is taken at start-up, so a server
   that still runs without the network module refuses the others, and the other way round.
8. Run `/uxm network status` on each server.

<Callout type="warning" title="Turn it on everywhere, or nowhere">

The network module is read before the database lease is taken. A server without it holds the
database alone and the network servers are refused, or it is refused itself. Restart every server
after changing the module on any of them.

</Callout>

## Reading `/uxm network status`

| Line | Tells you |
|---|---|
| Header | This server's node id, whether it leads the scheduled jobs, whether it may open vaults |
| Change log | How far this server has read the log, and when it last read it |
| Changes from other servers | How many were received, reloaded, failed, and not followed here |
| Followed here | The modules and data types this server reloads when another server changes them |
| Scheduled jobs | Which server runs tax day, expiry and purges |
| Redis | Connected or not, the channel, messages sent and received |
| Servers | Every node with its last heartbeat; a silent node is marked as stopped |

A rising "not followed here" count means another server changes something this server's modules do
not reload, usually because the two servers run different modules.

## The leader

`leader: auto` lets the live server with the smallest node id run the scheduled jobs. Set
`leader: always` on one server and `never` on the rest to choose it yourself. When the leader stops,
another server takes over once its heartbeat is missed.

## Known limits

- Some changes reach other servers only after a restart: newbie protection given or removed on
  another server, claim-block holograms after a rename elsewhere, new vassal pairs and peaceful
  statuses in wars, and Discord links.
- Claim points are saved per player as a whole: two servers changing one player's points at the
  same moment keep the last save.
- With the Discord module on its own bot, run the module on one server only; every server would
  otherwise start its own bot.

Related: [Network](modules/network.md), [Database](configuration/database.md), [Presets](configuration/presets.md)
