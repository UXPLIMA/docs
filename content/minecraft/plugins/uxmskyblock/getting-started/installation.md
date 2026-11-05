---
title: Installation
order: 102
description: Requirements, optional plugins, the island world, the first start, the files and updating.
icon: download
---

This manual describes uxmSkyblock 0.0.1, the first release under UXPLIMA.

## Requirements

| Requirement | Detail |
|---|---|
| Java | 25 or newer |
| Server | Paper 26.2 or newer, or Folia |
| Economy | Vault or VaultUnlocked with an economy plugin, for the bank, upgrades, the shop and upkeep |

uxmLib is shaded into the jar. The MariaDB and PostgreSQL drivers are fetched into the server's libraries folder
the first time the plugin loads.

## Optional plugins

| Plugin | Adds |
|---|---|
| Vault, VaultUnlocked | The island's money, and a second money currency |
| Treasury, PlayerPoints, EcoBits | Bank currencies of their own |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md) |
| floodgate, Geyser | Forms in place of chest windows for Bedrock players |
| dynmap, BlueMap, Pl3xMap | Islands drawn on the web map |
| ItemsAdder, Nexo, CraftEngine, MythicMobs | Custom blocks and creatures in OneBlock phases |
| mcMMO | Skill levels in requirements |

See [Integrations](../integrations/overview.md) for what each one turns on.

## The island world

Islands live in one world, named by `network.world-name` in `config.conf` (`world` as shipped). Give that world the
plugin's empty generator in `bukkit.yml` before the world is first made:

```yaml
worlds:
  world:
    generator: uxmSkyblock
```

A world that already has terrain keeps it. On Folia no world is made while the server runs, so there the island
world is the default world. The plugin says at startup when the island world is missing or is generated terrain.

Two game modes play on generated land in a world of their own, which you make the server's usual way:

| Mode | World, as shipped |
|---|---|
| [Boxed](../game-modes/boxed.md) | `boxed` |
| [StrangerRealms](../game-modes/strangerrealms.md) | `realms` |

A mode whose world is not loaded is not offered, and the console says so at start. A Folia server runs one world with
its Nether and End, so Boxed and StrangerRealms stay off there; a network plays them on a Paper or Leaf server.

## Installing

1. Put `uxmSkyblock-<version>.jar` in `plugins/` and remove any older jar.
2. Set the generator of the island world in `bukkit.yml` as above.
3. Start the server. The plugin writes its folder and its database.
4. Read `config.conf` and `modules.conf`, change what you need, and restart.
5. To offer game modes, list them in `presets.modes` in [presets.conf](../config/presets-conf.md). As shipped it is
   `["skyblock"]`, so players see the four classic presets; `["all"]` offers every mode.

## The files

```text
plugins/uxmSkyblock/
├── config.conf          the network, the grid, language, storage, player state, roles, homes,
│                        the REST API, island backups, the leaderboard and menu sounds
├── modules.conf         which parts of the plugin run
├── commands.conf        the words of /island and /isadmin
├── theme.conf           colours, glyphs and small capitals
├── text-input.conf      how a menu asks a player to write something
├── languages.conf       the language each player chose
├── messages/            messages_<language>.conf, one file per language
├── menus/               one file per window
├── modules/             one file per module and game mode
├── oneblock/phases/     one file per OneBlock phase
├── loot/                loot tables for chests
├── data/                skyblock.db and, by default, island-backups/
└── backups/             the last five copies of each file, taken before a start writes it
```

## Updating

Stop the server, replace the jar and start it. Each start adds the keys a new release brought to every file and keeps
what you wrote; the file as it was is kept beside it with `.bak`. A key you deleted stays deleted. See
[Configuration](../config/index.md).

## Checking the install

Run `/is doctor` as an operator. It prints one line each for the database, the windows read from `menus/`, the
placeholders, the island world and the economy, with `ok`, `warn` or `fail`. Whether the database would lose a commit on
power loss is checked at every start, as `storage.durability-profile` asks; see [Database](../database/overview.md).
