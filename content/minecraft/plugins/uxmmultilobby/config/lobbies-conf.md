---
title: lobbies.conf
order: 502
description: Every key of a lobby, the shipped five, slots, and naming a lobby in each language.
icon: list
---

The lobbies of the network, one block each. Keep the file the same on every lobby.

| Key | Meaning |
|---|---|
| the block name | The id: lower-case letters, digits, `-` or `_`, starting with a letter or digit, at most 32 characters |
| `fallback-name` | The name shown when the messages file has no `lobby.<id>.name` |
| `proxy-name` | The name the proxy knows the server by. It must match |
| `host`, `port` | Where the `ping` source connects; port 1 to 65535 |
| `maximum` | What full means, and what balancing measures against; at least 1 |
| `slot` | The place in the window |
| `icon` | The material of the icon |
| `colour` | The tile title colour: a wheel position such as `"0"`, a role such as `"good"`, or a named gradient |
| `permission` | The node a player needs to enter; empty means everybody |
| `weight` | Above 1 takes more players, below 1 fewer, `0` keeps the lobby out of `/quicklobby` |

## The shipped lobbies

| Id | Proxy name | Port | Maximum | Slot | Icon | Colour | Permission |
|---|---|---|---|---|---|---|---|
| `lobby-1` | `lobby1` | 25566 | 100 | 11 | `GRASS_BLOCK` | `0` | |
| `lobby-2` | `lobby2` | 25567 | 100 | 12 | `GRASS_BLOCK` | `1` | |
| `lobby-3` | `lobby3` | 25568 | 100 | 13 | `GRASS_BLOCK` | `2` | |
| `lobby-4` | `lobby4` | 25569 | 100 | 14 | `GRASS_BLOCK` | `3` | |
| `lobby-private` | `lobbyprivate` | 25570 | 50 | 15 | `GOLD_BLOCK` | `4` | `uxmmultilobby.lobby.private` |

All on `127.0.0.1` with weight `1.0`. Replace them with your own.

## Slots

A slot has to be one of the slots `menus/lobbies.conf` gives the list (10 to 16 as shipped). A lobby that names a slot
outside them, or one another lobby took, takes the next free slot instead, and the console says which lobby moved. Five
lobbies in 11 to 15 are centred in the row; for a sixth, give the window a second row rather than 10 to 15.

## Names in each language

```hocon
lobby {
  lobby-1 {
    name = "Lobby One"
  }
}
```

in each messages file. When you add a lobby, add its name there too, or it shows `fallback-name`.

Related: [Concepts](../getting-started/concepts.md#lobbies), [Messages](messages.md)
