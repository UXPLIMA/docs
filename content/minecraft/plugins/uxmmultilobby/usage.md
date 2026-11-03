---
title: "Guide"
order: 10
description: "How uxmMultiLobby works, how to set it up and what each file holds."
---

uxmMultiLobby puts a menu of your other lobbies on every lobby server, with live player counts, and moves a player to the one they click. `/quicklobby` picks the least loaded lobby for them.

## Install

1. Put the jar in `plugins/` on every lobby server behind the proxy.
2. Start the server once. The plugin writes its files into `plugins/uxmMultiLobby/`.
3. Fill `lobbies.conf` with your lobby servers, and set `this-lobby` in `config.conf` to the id of the server you are editing.
4. Run `/uxmmultilobby reload`, or restart.

The move goes through the proxy over the `BungeeCord` plugin messaging channel, which both BungeeCord and Velocity understand. On Velocity, keep `bungee-plugin-message-channel = true` in `velocity.toml`.

PlaceholderAPI is optional. When it is installed the plugin registers its placeholders.

## Files

| File | What it holds |
|---|---|
| `config.conf` | which lobby this server is, where the counts come from, the cooldown, the hotbar tool, the effect of every outcome |
| `lobbies.conf` | the lobbies of the network, one block each. A `config.conf` from an earlier release gives its list to this file on the first start |
| `menus/lobbies.conf` | the lobby window: rows, the slots the lobbies fill, panes and buttons |
| `commands.conf` | the name, aliases and on or off switch of every command |
| `messages/messages_<language>.conf` | the words players read, one file per language |
| `theme.conf` | the colours the messages and tiles use |

A reload reads every file again and swaps them in as one snapshot. A file that does not parse leaves the previous snapshot in place.

## Lobbies

Each lobby is one block under `lobbies`. The key is the id: lower-case letters, digits, a hyphen or an underscore, starting with a letter or a digit, at most 32 characters. It appears in placeholders and in `/lobby <id>`, so keep it stable.

| Field | Meaning |
|---|---|
| `fallback-name` | the name shown when the messages file has no `lobby.<id>.name` line |
| `proxy-name` | the name the proxy knows the server by; it must match |
| `host`, `port` | where the `ping` source connects; port 1 to 65535 |
| `maximum` | what full means, and what balancing measures against; at least 1 |
| `slot` | the menu slot, 0 to 53; two lobbies cannot share one |
| `icon` | the material of the menu icon |
| `colour` | the tile title colour: a theme wheel position such as `"0"`, a role such as `"good"`, or a named gradient |
| `permission` | the node a player needs to enter; empty means everybody |
| `weight` | above 1 takes more players, below 1 fewer, `0` keeps the lobby out of `/quicklobby` |

A lobby with a permission the player lacks is shown locked. Set `menu.hide-locked = true` to leave it out of the menu instead.

To colour every lobby by how full it is instead of drawing its own icon, write `material = "%lobby_load_icon%"` in `menus/lobbies.conf`. A lobby is then `current` (the one the player is on), `open`, `busy` (at least `menu.busy-at-percent` of its maximum, 50 by default), `full` or `closed` (offline, or not heard from yet), and `menu.load-icons` gives the material for each: blue, lime, yellow, red and gray candles as shipped. `%lobby_load%` is the word itself.

## Live counts

`status.source` chooses where the counts come from:

- `proxy` asks the proxy. It needs nothing else, but the proxy reports no maximum and reports zero for a crashed lobby, so a dead lobby looks empty.
- `ping` reads each lobby's server list ping on its `host` and `port`. It is the only source that tells a dead lobby from an empty one.
- `redis` listens on `status.redis.channel` for frames that the lobbies publish themselves, written as `<lobby-id>;<players>;<maximum>`. A maximum of `0` means the configured one. It needs `lettuce-core` on the server; without it the plugin logs a warning and falls back to `proxy`.

`interval-ms` is how often the counts refresh, `timeout-ms` how long one ping waits, and `stale-after-ms` how long an answer is trusted. A lobby that has not answered within `stale-after-ms` is shown offline. `0` trusts an answer for ever, which suits `redis` only.

A lobby is full when its count reaches its maximum. A lobby that is full, offline or not heard from yet cannot be joined.

## Choosing a lobby automatically

`/quicklobby` sends the player to the lobby with the lowest load among the ones they may enter, leaving out the lobby they are on. The load is the player count divided by the capacity times the weight, where the capacity is the maximum the lobby reports, or the configured `maximum` when it reports none. A tie goes to the lobby with fewer players, then to the lower id. When no lobby is open, the player gets the `nothing-available` outcome.

## Cooldown

`connect.cooldown-ms` is how long a player waits between two moves. `0` turns it off. A move that is refused for another reason (full, offline, no permission) does not start the cooldown.

## The hotbar tool

The `tool` block hands out an item that opens the menu: `material`, the hotbar `slot` 0 to 8, whether it is `locked` in place, and whether it is given on join and on a world change. `permission` limits it to players with that node, empty meaning everybody. `worlds.mode` is `everywhere`, `only` (only the worlds in `list`) or `except` (every world but those); world names are matched without case.

## Outcomes

Every move ends in one of eight outcomes, and `effects` holds a list of actions for each: `connecting`, `already-there`, `unreachable`, `full`, `not-permitted`, `cooldown`, `no-such-lobby` and `nothing-available`. Each line is `[verb] payload`, run in order. An empty list silences the outcome.

| Verb | Payload |
|---|---|
| `[message]` | a chat line |
| `[actionbar]` | a line over the hotbar |
| `[subtitle]` | `<title> \| <subtitle> <fade-in> <stay> <fade-out>`, the times in seconds |
| `[sound]` | `<key> <volume> <pitch>` |
| `[particle]` | `<name> <count> <spread> [data]`, the data a colour, a block or an item |
| `[console]` | a command run by the server |
| `[player]` | a command run by the player |

Text that starts with `@` names a line of the messages file, so it is read in the player's language. The placeholders are `<lobby>` for the lobby named, `<players>` and `<maximum>` for `full`, `<seconds>` for `cooldown` and `<name>` for `no-such-lobby`.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/lobby` | `uxmmultilobby.use` | open the lobby menu |
| `/lobby <id>` | `uxmmultilobby.use` | join that lobby; the id is matched without case |
| `/quicklobby` | `uxmmultilobby.use` | join the least loaded lobby |
| `/lobby lang` | `uxmmultilobby.language` | say which language you read and which ones exist |
| `/lobby lang set <language>` | `uxmmultilobby.language` | read the plugin in that language |
| `/lobby lang reset` | `uxmmultilobby.language` | go back to the server's choice |
| `/lobby lang set-player <player> <language>` | `uxmmultilobby.admin.language` | set another player's language |
| `/lobby lang reset-player <player>` | `uxmmultilobby.admin.language` | clear another player's language |
| `/uxmmultilobby reload` | `uxmmultilobby.admin.reload` | read every file again |
| `/uxmmultilobby version` | `uxmmultilobby.admin.version` | show the running version |
| `/uxmmultilobby doctor` | `uxmmultilobby.admin.doctor` | report the plugin's health |

The names and aliases come from `commands.conf`, so a server that already has a `/lobby` can rename this one. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmmultilobby_players_<id>%` | the count on that lobby |
| `%uxmmultilobby_maximum_<id>%` | the maximum that lobby reports, or the configured one |
| `%uxmmultilobby_status_<id>%` | `online`, `full`, `offline` or `unknown` |
| `%uxmmultilobby_total%` | the count over every configured lobby |
| `%uxmmultilobby_lobbies%` | how many lobbies are configured |

Every value is read from the cache, so a scoreboard does not open a socket. An unknown id leaves the placeholder as written.

## Example

Two public lobbies and a staff lobby that `/quicklobby` never picks. `config.conf` says `this-lobby = "hub-1"`, and `lobbies.conf` holds:

```hocon
lobbies {
  hub-1 {
    fallback-name = "Hub One"
    proxy-name    = "hub1"
    host          = "10.0.0.11"
    port          = 25566
    maximum       = 120
    slot          = 12
    icon          = "GRASS_BLOCK"
    colour        = "0"
    permission    = ""
    weight        = 1.0
  }
  hub-2 {
    fallback-name = "Hub Two"
    proxy-name    = "hub2"
    host          = "10.0.0.12"
    port          = 25566
    maximum       = 120
    slot          = 13
    icon          = "GRASS_BLOCK"
    colour        = "1"
    permission    = ""
    weight        = 1.0
  }
  staff {
    fallback-name = "Staff"
    proxy-name    = "staff"
    host          = "10.0.0.20"
    port          = 25566
    maximum       = 20
    slot          = 14
    icon          = "COMMAND_BLOCK"
    colour        = "good"
    permission    = "server.staff"
    weight        = 0.0
  }
}
```

## Troubleshooting

- **Every lobby shows offline.** With `proxy`, check that each `proxy-name` matches the proxy's server list. With `ping`, check `host`, `port` and the firewall.
- **A crashed lobby shows as empty.** That is the `proxy` source; switch to `ping`.
- **The console says the source fell back to the proxy.** `status.source` is `redis` and `lettuce-core` is not installed.
- **Clicking does nothing.** The player is on that lobby already, or on cooldown; the `effects` lists decide what they are told.
