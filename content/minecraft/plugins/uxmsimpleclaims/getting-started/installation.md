---
title: Installation
order: 102
description: Requirements, optional plugins, the files, updating and checking the install.
icon: download
---

This manual describes uxmSimpleClaims after 0.0.1: German, Spanish, French, Portuguese and Russian arrive with the
next release, and 0.0.1 itself reads English and Turkish.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia |

uxmLib is shaded into the jar, so the plugin needs nothing else.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | Money for land, rent, sales, warps, taxes and time |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| LuckPerms | [Contexts](../integrations/overview.md#luckperms-contexts) for where a player stands |
| WorldGuard | Claims never cover a region |
| BlueMap, Dynmap, Pl3xMap, squaremap | Claims on the [web map](../integrations/overview.md#web-maps) |
| Floodgate, Geyser | Forms for Bedrock players |
| uxmLang | One language choice for every UXM plugin |

Without a currency plugin, set `economy.currency` to `exp` or `levels`, or keep land free with `free-chunks`.

## Installing

1. Drop `uxmSimpleClaims-<version>.jar` into `plugins/` and remove any older jar.
2. Start the server once. The plugin writes its files into `plugins/uxmSimpleClaims/`.
3. Set `network.server-name` to this server's name, choose the world modes and prices, then `/claimadmin reload`.

Turkish readers see Turkish command words: `/arazi` for `/claim` and `/araziyonetim` for `/claimadmin`.

## The files

```
plugins/uxmSimpleClaims/
├── config.conf          language, storage, network, worlds, claims, members, economy, tax, expiry, inactivity,
│                        rent, flags, display, teleport, retention, limits, integrations, leaderboard, menu
├── roles.conf           the roles every new claim starts with
├── commands.conf        command and branch words, per language
├── menus/               fourteen windows
├── messages/            messages_<language>.conf: en, tr, de, es, fr, pt, ru
├── languages.conf       the language each player chose, written by the plugin
├── theme.conf           this plugin's colours on top of plugins/uxmTheme/theme.conf
├── backups/             config.conf as it was before an update added keys
└── data/claims.db       the SQLite database
```

A database password, Redis URL or webhook URL can be written as `"${VARIABLE}"` and is read from the server's
environment; a name that is not set stays as written.

## Updating

Replace the jar and restart. Keys a release adds are added to `config.conf` with their comments, and your values stay;
the file as it was goes into `backups/`. The database moves into `data/` by itself.

## Checking the install

`/claimadmin doctor` says whether storage, the windows, the claims and the placeholders are working.

## Troubleshooting

- **Nobody can claim in a world.** Its mode is `protected` or `disabled`.
- **Nobody can build outside claims.** The world's mode is `claims-required`.
- **A claim is refused next to another.** `claims.min-distance` keeps claims apart, and claims never cover a WorldGuard
  region.
- **Who owns this?** `/claimadmin inspect` and the inspection stick from `/claimadmin stick` say.
- **A word is missing in your language.** It falls back to the default the plugin carries.

Related: [Concepts](concepts.md), [config.conf](../config/config-conf.md)
