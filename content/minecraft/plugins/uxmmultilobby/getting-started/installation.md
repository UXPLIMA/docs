---
title: Installation
order: 202
description: Requirements, the proxy setting, the files that appear, updating and checking the install.
icon: download
---

This manual describes uxmMultiLobby after 0.1.0: German, Spanish, French, Portuguese and Russian arrive with the next
release, and 0.1.0 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia, on every lobby |
| Proxy | Velocity or BungeeCord. On Velocity, keep `bungee-plugin-message-channel = true` in `velocity.toml` |
| Redis | Only for the `redis` source, with `lettuce-core` on the server |

A move goes through the proxy over the `BungeeCord` plugin messaging channel, which both proxies understand.

## Optional plugins

| Plugin | Adds |
|---|---|
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and placeholders in effect lines |
| Geyser or Floodgate | The window as a Bedrock form |
| uxmLang | One language choice for every UXM plugin |

## Installing

1. Drop `uxmMultiLobby-<version>.jar` into `plugins/` on every lobby server behind the proxy.
2. Start each server once. The plugin writes `plugins/uxmMultiLobby/`.
3. Fill `lobbies.conf` with your lobbies, the same on every lobby.
4. Set `this-lobby` in `config.conf` to the id of the server you are editing, different on each lobby.
5. `/uxmmultilobby reload`, or restart.

## The files

```
plugins/uxmMultiLobby/
├── config.conf        language, this lobby, status, cooldown, menu, hotbar tool, effects
├── lobbies.conf       the lobbies of the network, one block each
├── commands.conf      the name, aliases and switch of every command
├── menus/lobbies.conf the lobby window
├── messages/          messages_<language>.conf, one per language: en, tr, de, es, fr, pt, ru
├── languages.conf     the language each player chose, when uxmLang is not installed
└── backups/           the last five copies of each file a release rewrote
plugins/uxmTheme/
└── theme.conf         the colours and glyphs every UXM plugin reads
```

A `config.conf` from a release that held the lobby list gives its list to `lobbies.conf` on the first start.

## Updating

Stop the server, replace the jar and start it. A release that adds a key writes it into your `config.conf` with its
comment and never changes a value you wrote; your file as it was is kept in `backups/`. The lobby list is yours: a lobby
you removed is not written back.

## Checking the install

`/uxmmultilobby doctor` (permission `uxmmultilobby.admin.doctor`):

| Check | ok | fail or warn |
|---|---|---|
| `windows` | How many window files were read | fail: none, so the window cannot open |
| `placeholders` | `published` | warn: PlaceholderAPI is not on the server; everything else works |

## Metrics

The plugin sends anonymous usage counts to bStats (plugin id 34252). Turn it off for every plugin in
`plugins/bStats/config.yml`.

Related: [Concepts](concepts.md), [Configuration](../config/index.md)
