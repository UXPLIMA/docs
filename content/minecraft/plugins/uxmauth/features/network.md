---
title: A network
order: 313
description: Velocity and every server behind it as one door, signed claims, routing, Redis and the proxy's own waiting world.
icon: network
---

On a network, uxmAuth runs on the Velocity proxy and on every Paper server behind it. The proxy runs the premium check,
knows who has logged in and tells each server so with a claim it signs. A server takes a claim only with the right
signature, so a player who reaches it some other way is asked for the password as usual.

## Setting it up

1. Put the Velocity jar on the proxy and the Paper jar on every server. The proxy reads the same `config.conf`,
   `ownership.conf` and `network.conf`, so the files can be copied to every machine. It uses `language`, `storage`,
   `premium`, `ownership` and `network`; when players wait in its own world also `names`, `passwords`, `hashing`, `lane`
   and `timeouts`, and the words of `/login` and `/register` from `commands.conf`.
2. Point `storage.jdbc-url` of the proxy and every server at one MySQL, MariaDB or PostgreSQL database.
3. Start the proxy. With `network.secret` empty it makes one and writes it to `network-secret.txt` in its data folder,
   readable by its owner only, and says where in its console.
4. Copy that secret into `network.secret` on every server, set `network.enabled = true` and `network.server-name` to the
   server's name in the proxy's list. Keep the secret private: whoever has it can log anybody in. A server with the
   network on and no secret refuses to start.

Leave `network.enabled` off on a server that stands alone.

## Routing, on the proxy

| Key | Default | Meaning |
|---|---|---|
| `auth-servers` | `[]` | Where a player waits until logged in; empty, they wait at the dialog on whichever server they join and stay there |
| `lobbies` | `[]` | Where a player goes from an auth server once logged in, and where a player logged in by Mojang goes first |
| `balance` | `least-players` | `least-players`, `random` or `first` |
| `after-login` | `lobby` | `lobby`, `last-server` (the server they last left, or a lobby) or `by-domain` |
| `by-domain` | `[]` | `{ host = "eu.example.org", server = lobby-eu }` entries, read in order; a star stands for one part of a name |
| `allowed-commands` | `[]` | Proxy commands a player may run before logging in, such as `glist`. Every other proxy command, and every move to another server, waits |
| `claim-seconds` | `30` | How long a signed claim stays good; a fresh one is written before each connection |

## Redis

Redis carries what every proxy and server must hear at once, such as an account whose sessions ended after a password
change. Every member writes the same `network.redis.uri` (`redis://:password@host:6379/0`) and `channel` (`uxmauth`).
With an empty `uri` the proxy alone holds the logins and each server reads the database.

## The proxy's waiting world

`network.wait-in` on the proxy:

| Value | The player waits |
|---|---|
| `server` (default) | At the dialog or frozen world of the server they join, so every server behind the proxy must run uxmAuth |
| `limbo` | In a world built inside the proxy that no server sees, typing `/login` or `/register`. Needs LimboAPI 1.1 or 2 |

`limbo-file` (`limbo.schem`) names a schematic in the data folder the world is built from; without it the player stands
on a small platform at `limbo-spawn` (`[0.5, 100, 0.5]`). A proxy asked for its own world that cannot build it refuses
every login and says why: a server without uxmAuth would let a waiting player straight in. With the proxy's world, a
player who has not logged in reaches no server.

## Across the network

Alerts reach staff with `uxmauth.admin.alerts` on every server. `/uxmauth forcelogin` and the other staff commands act on
the account wherever its player waits. A ban made at the proxy is not sent to Discord; one made on a server is. The
`[console]` and `[player]` lines of `registered` and `logged-in` run once per login on the proxy.

Related: [network.conf](../config/network-conf.md), [Database](../database/overview.md)
