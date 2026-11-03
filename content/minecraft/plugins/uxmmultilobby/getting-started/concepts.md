---
title: Concepts
order: 103
description: Lobbies and their ids, this lobby, where counts come from, the states of a lobby, and outcomes.
icon: lightbulb
---

## Lobbies

A lobby is one block in `lobbies.conf`. Its key is the id: lower-case letters, digits, a hyphen or an underscore,
starting with a letter or a digit, at most 32 characters. The id appears in `/lobby <id>`, in placeholders and in the
messages file, so keep it stable. `proxy-name` is the name the proxy knows the server by, and must match it.

Every lobby server carries the same `lobbies.conf`.

## This lobby

`this-lobby` in `config.conf` says which lobby this server is. It marks that icon as "you are here" and keeps
`/quicklobby` from sending a player to the server they are on. Leave it empty on a server that is not itself a lobby.

## Where counts come from

| Source | How | Tells a dead lobby from an empty one |
|---|---|---|
| `proxy` | Asks the proxy over the plugin messaging channel | No: the proxy reports zero for a crashed lobby, and no maximum |
| `ping` | Reads each lobby's server list ping on its `host` and `port` | Yes |
| `redis` | Listens to counts the lobbies publish themselves | Yes |

The counts refresh on the async scheduler every `interval-ms` and the window reads them from a cache, so a click never
waits. See [Live counts](../features/live-counts.md).

## States

| State | When |
|---|---|
| online | It answered and has room |
| full | Its count reached its maximum |
| offline | It has not answered within `stale-after-ms` |
| unknown | It has not answered since this server started |

A lobby that is full, offline or unknown cannot be joined. For the window's colours, a lobby is also `current`, `open`
or `busy`; see [The lobby window](../menus/lobbies.md#colouring-by-load).

## Outcomes

Every move ends in one of eight outcomes: `connecting`, `already-there`, `unreachable`, `full`, `not-permitted`,
`cooldown`, `no-such-lobby` and `nothing-available`. What each says and plays is a list in `effects`. See
[Outcomes](../features/outcomes.md).

Related: [Your first network](first-network.md), [lobbies.conf](../config/lobbies-conf.md)
